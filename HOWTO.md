# How to Generate Outputs

## Prerequisites

Install `pos-decorate` from the companion repo:

```bash
cd ~/repos/pos-decorate
python -m venv venv
source venv/bin/activate
pip install -e .
```

Or, if already installed:

```bash
source ~/repos/pos-decorate/venv/bin/activate
```

## File Naming Convention

| Prefix    | Meaning                                 |
| --------- | --------------------------------------- |
| `brutus_` | Shakespeare source — Antony's oration   |
| `hn_`     | Hungarian Notation parallel — our piece |

## Generate Outputs from the HN Speech

All commands run from the root of this repo.

### Verify the tokenizer round-trips cleanly

```bash
pos-decorate hn_speech.md --verify-parse-and-regenerate
```

Parses `hn_speech.md`, regenerates the text from the parse tree, and diffs the result
against the original. Expects `OK`. Exit code 0 = clean.

### Inspect the parse tree (debug)

```bash
pos-decorate hn_speech.md --style=parse-tree
```

Shows each token with its POS tag and readable abbreviation.

### Plain text with POS prefixes

```bash
pos-decorate hn_speech.md --style=plain > hn_plain.txt
```

### Interactive HTML (all four view modes)

```bash
pos-decorate hn_speech.md --style=html > hn_output.html
open hn_output.html
```

| View         | Description                                |
| ------------ | ------------------------------------------ |
| As Written   | Plain prose, no annotation                 |
| POS-Tagged   | POS prefix annotations as black/white text |
| Tag-Receding | Annotations dimmed, prose in full black    |
| Grammar-Lit  | Words color-coded by POS class             |

### Other style variants

```bash
pos-decorate hn_speech.md --style=raw-nltk   # NNS_Friends, VBP_come
pos-decorate hn_speech.md --style=camel      # nFriends, vCome
```

### Run the same styles against the Shakespeare source

```bash
pos-decorate brutus_speech.md --style=plain
pos-decorate brutus_speech.md --style=html > brutus_output.html
```

## Regenerate after editing the HN speech

If `hn_speech.md` is unfrozen and edited, rerun:

```bash
pos-decorate hn_speech.md --verify-parse-and-regenerate && \
pos-decorate hn_speech.md --style=html  > hn_output.html && \
pos-decorate hn_speech.md --style=plain > hn_plain.txt
```
