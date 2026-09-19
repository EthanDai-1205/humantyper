# Research

Every behavioural parameter in HumanTyper's timing model traces to published
research. Where a study establishes a direction or a shape but not a
per-keystroke magnitude (most of these studies measure at coarser scales than a
single key interval), the model uses a conservative bounded range around the
direction the source establishes.

**The calibrated numbers are not published here.** This page lists the sources,
what each one establishes, and what the model does with it. The magnitudes, the
calibration constants and the parameter tables are part of the product and stay
private; what follows is enough to see that the model is built on evidence
rather than invented.

## Timing and motor control

| Behaviour | Source | What the source establishes |
|---|---|---|
| Skewed inter-key intervals | Keystroke-dynamics literature | Inter-key timing distributions are skewed and heavily right-tailed, not uniform |
| Motor bursts and the pauses between them | Terzuolo & Viviani (1980) | Typing is organised in short motor patterns separated by pauses |
| Word-initiation delay | Pinet et al. (2022); Salthouse (1986) | The first key of a word waits on central lexical retrieval while later keys overlap with peripheral motor execution, so words start slow and finish fast |
| Two-component fluency | Keystroke corpora and keystroke logging (Wengelin 2006; Dhakal et al. 2018) | A fluent mode plus an occasional far more variable disfluent stroke, which is what gives real data its heavy tail |
| Fast common sequences | Keystroke-dynamics literature | High-frequency sequences are typed far faster than the same letters in another order |
| Digraph latency by finger pair | Mahar et al. (1995) | Digraph latencies differ systematically by which fingers and hands are involved |
| Post-error slowing | Kalfaoğlu & Stafford (2014) | Inter-key intervals lengthen right after an error, most strongly on the next keystroke, decaying within a stroke or two |
| Per-typist variability | Killourhy & Maxion (2009) | Error rate is an individual trait, not a constant, so runs should differ around a setting |
| Speed drifts within a session | Fang et al. (2026) | Speed rises and then declines over a long session |

## Errors and corrections

| Behaviour | Source | What the source establishes |
|---|---|---|
| Error taxonomy | Grudin (1983); Soukoreff & MacKenzie (2001) | Entry errors fall into substitutions, omissions, transpositions and double-strikes, with substitutions most common |
| Substitutions come from finger crosstalk | Rumelhart & Norman (1982) | Adjacent-key errors arise from the wrong finger firing, so the neighbourhood is finger-shaped, not uniform |
| Near misses spread over a neighbourhood | Layout-derived typo generators (2025) | Real slips land on the intended key's neighbours with directional weighting rather than on one deterministic key |
| Sequencing errors | Rumelhart & Norman (1982) | Serial ordering produces both anticipations and perseverations, systematically |
| Corrected is the norm, uncorrected is real | Robinson et al. (1998); Soukoreff & MacKenzie (2001) | Corrections dominate but uncorrected errors are non-zero, and detection often lags until after the word |
| Longer words carry more errors | Anastaseni et al. (2025) | Without word-level support, long words produce more errors and more corrections |
| Case slips | Standard error taxonomies | Capitals landing lowercase is a routine class of slip |
| Word doubling | Typo corpora | A completed word occasionally repeats and is caught |

## Geometry, dwell and pointing

| Behaviour | Source | What the source establishes |
|---|---|---|
| Key hold duration | Dhakal et al. (2018), across roughly 168k keystrokes | Press durations are distributed, not constant |
| Hold and flight are the measured features | Park (2024); Robinson et al. (1998) | Classifiers work on hold and flight time, so both have to be right |
| Row reach | Fitts (1954) | Movement time grows with distance to the target |
| Row effects stay small | Swanson et al. (1997) | Large keyboard geometry changes barely affect productivity, so row factors must stay subtle |
| Pointer duration | Soukoreff & MacKenzie (2004); Fitts (1954); MacKenzie (1992) | Pointing time follows a distance and width law |
| Pointer path shape | Plamondon & Alimi (1997) | Handwriting and pointing kinematics have a characteristic velocity profile |
| Two-phase movements | Nieuwenhuizen et al. (2009) | Real movements often undershoot and then correct |
| Sporadic pointer motion | Ahmed & Traore (2007) | Human pointer activity is smooth and intermittent, not constant jitter |
| Individual timing profiles | Tsimperidis et al. (2015) | Users are distinguishable by their timing profiles |

## Deliberately not modelled

Honesty cuts both ways, so this is the list of things the evidence argued
against modelling, or where the cost outweighed the realism:

- **Strong row and geometry effects.** The productivity study found none.
- **Every typo corrected instantly.** The uncorrected-error metric shows real
  typing leaves errors standing, so some slips survive and some are repaired
  only after the word.
- **Constant tiny mouse wiggling.** Human pointer motion is smooth and
  sporadic. The app glides occasionally and is otherwise still.
- **Word-meaning substitutions** (their/there, to/too). They would change the
  semantics of the delivered text, which is dangerous in chat and command
  contexts.
- **Space omissions.** The repair machinery is word-anchored, and a missing
  separator breaks offset bookkeeping. Other slip classes cover the realistic
  per-key mistakes.
- **Phonetic misspellings** (enywhere for anywhere). There is no clean citation
  tying them to copy typing at per-keystroke rates, so they are not faked.
