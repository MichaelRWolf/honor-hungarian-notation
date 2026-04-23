# honor-hungarian-notation — Claude Context

## Project

The HN takedown speech — a parallel to Mark Antony's Caesar funeral oration.
Part of a two-repo project; the tooling lives in `~/repos/pos-decorate`.

## Plan File

`~/.claude/plans/i-have-long-wanted-compiled-robin.md`

## Phase Status

| Phase | Description                          | Status    |
| ----- | ------------------------------------ | --------- |
| -1A   | Create pos-decorate repo             | ✅ done   |
| -1B   | Create honor-hungarian-notation repo | ✅ done   |
| 0     | Copy speech source → speech.md       | ✅ done   |
| 1     | Rhetorical analysis                  | ✅ done   |
| 2     | HN pro/con research                  | ✅ done   |
| 3     | Speech draft                         | ✅ FROZEN |
| 4     | pos-decorate CLI (Repo A)            | ✅ done   |
| 4B    | Generate speech_output.html          | ✅ done   |
| 5     | Blog post → michaelrwolf.github.io   | ⬜ next   |

## Resume

Say `continue` to pick up Phase 5 (blog post).

## Key Artifacts

- `speech_draft.md` — frozen speech (do not edit without unfreezing)
- `speech_output.html` — interactive HTML, open directly in browser
- `speech_plain.txt` — plain POS-decorated text
- `speech_analysis.md` — rhetorical move map, character mapping, irony gradient
- `hungarian_notation_pro_con.md` — research: era-appropriate virtues + modern cons

## Key Decisions

- Speaker = Mark Antony (not Brutus) — surface praise, real takedown
- Refrain: "...for Hungarian Notation is an honorable practice" (curdles with each repetition)
- The Practitioners = proper noun — modern cargo-culters; Simonyi NOT implicated
- Apps Hungarian (kind/purpose) vs Systems Hungarian (type) — The Practitioners defend
  the former while practicing the latter; they have the wrong thing right and right thing wrong
- Audience: programmers who know HN — no explanation, honor their intelligence
- Default view: As Written; button adds decoration (meta-joke)
- Button label (plain → tagged): "Add the helpful Hungarian Notation annotations →"
- Button label (tagged → plain): "Press to see how much less readable this is without the decoration →"
- Closing riff: "This text was decorated by the same code it mocks"
- Published at: <https://michaelrwolf.github.io/>

## Tool Setup

pos-decorate lives in `~/repos/pos-decorate` with its own venv:

```bash
source ~/repos/pos-decorate/venv/bin/activate
pos-decorate speech_draft.md --style html > speech_output.html
```
