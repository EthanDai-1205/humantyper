# Architecture

HumanTyper is one app built from a platform-neutral core and a thin macOS
shell. The core owns the timing model, the error model and the run loop, and it
never touches AppKit, SwiftUI or the window server. The shell owns the window,
the menu bar extra, the event tap and the settings store. Everything the core
needs from the host arrives through two small protocols, which is also what
makes the whole pipeline testable offline.

## The split

```
Sources/Core                      Sources/Mac
  Keymap                            Library.swift          (bundle entry)
  TimingModel                       ContentView.swift      (main window)
  TypoModel                         MenuPanelView.swift    (menu bar panel)
  MouseGlide                        EngineModel.swift      (observable run state)
  EventPosting    <- protocol       EventPosting/HID       (CGEventPost to HID)
  TypingEngine                      LicenseStore           (Keychain)
  License         (Ed25519)         EnvironmentObserver    (frontmost, Secure Input)
  SelfTest, StatsReport             PrefKey                (@AppStorage keys)
```

Two host protocols keep the core portable:

- `EventPosting` posts a keystroke, with a **recording sink** implementation used
  by the tests. That is how the pipeline runs end to end with no permissions and
  no window server.
- `LicenseStore` persists a licence. macOS backs it with the Keychain; the
  Windows target uses a file under the app data directory.

The same core also builds as an SPM package (`Package.swift`) and runs its
behavioural suite on Windows CI, ahead of a Windows release. `swift-crypto`
supplies Ed25519 on platforms without CryptoKit, so one set of vendor keys
verifies on both platforms.

## Why the HID event tap

Keystrokes are posted with `CGEventPost` to the HID event tap, the same path
physical keyboard hardware uses. Target apps therefore see ordinary keyboard
activity: discrete Shift press and release events, real keycodes, real dwell
times. Nothing is inserted through the accessibility API, nothing is pasted, and
no clipboard is touched, which is the difference between this and the usual
"type for me" utility.

## A run, step by step

1. **Snapshot.** Settings are read once at Start, so a slider moved mid-run
   cannot change the model under a running job.
2. **Countdown**, then a **focus re-check**: if the frontmost app changed while
   the user was switching windows, the run stops instead of typing into the
   wrong place.
3. **Calibration.** The engine runs the timing pipeline offline, without posting
   anything, and solves for the scale factor that makes its own delivered speed
   match the slider. The solve is memoised per setting, and it rides base
   intervals, dwell times and micro-pauses together, while long cognitive
   pauses stay absolute. This is what makes the speed dial honest rather than
   decorative.
4. **The typing loop.** Text is walked as words. Each character asks the layout
   for its keycode and shift state, asks the timing model for an interval, and
   posts a key-down, a dwell, and a key-up. Between words the typo model may
   inject a slip; after a slip the repair planner decides when and how it is
   fixed. Mouse glides are scheduled between words when the model says a human
   would have moved.
5. **Guards, twice a second.** The loop and the guards share one cadence: stop
   if the frontmost app changed, pause on Secure Input, honour a user pause,
   and keep the app awake against App Nap.
6. **Accounting.** Elapsed time, live speed and the estimate of remaining time
   are computed from engine time, so paused wall time never inflates the
   numbers, and the first couple of seconds of a run report progress rather than
   a fake speed reading.

## Slips and repairs

A slip is not just a wrong character; it is a plan. The typo model draws from
the error taxonomy (near-miss substitutions, omissions, reversals,
double-strikes, perseverations), picks the wrong key from the active layout's
geometry and finger assignment, and then a planner decides the outcome: repaired
within a keystroke or two, repaired after the word completes, or left standing.
Retrospective repairs choose between a backspace drum and a whole-word delete,
and the whole-word path only fires when every pending slip sits inside the
current word and the character before the word is not a tab, because word
semantics differ between text views and IDEs. Anything else falls back to the
drum.

This is the part that most often desyncs a simulation: the model believes it
typed one string while the target app holds another. The fix is structural. The
engine keeps a screen buffer that is updated only by the same operations that
post events, and an end-to-end test replays the raw event stream back into text
and asserts equality with both the input and the buffer.

## Two surfaces, one state

The main window and the menu bar panel are separate SwiftUI views over one
observable run model, and every setting is an `@AppStorage` key, so changing the
speed in the panel updates the window and the next run with no synchronisation
code. The menu bar icon itself reflects the phase (idle, typing, finished), and
the panel deliberately contains no editable text field, because a text field in
a menu bar extra steals focus from the target app.

## Licence and activation

Licence keys are Ed25519-signed offline artefacts. The app verifies them with an
embedded public key and stores the result in the Keychain. A Pro licence is
paired to one device through a second, separate signing key: the app shows a
device identifier, the vendor signs an activation certificate for that
identifier, and the app verifies the certificate against the activation public
key. Two keys, so a future activation service could hold only the activation
key, which is enough to pair licences with devices and never enough to mint a
licence.

No activation request is ever sent anywhere. The price of that design is that a
licence cannot be revoked remotely; the benefit is that the app can promise,
truthfully, that nothing leaves the Mac.
