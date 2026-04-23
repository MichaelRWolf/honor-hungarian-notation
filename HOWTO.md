# How to Generate Outputs

## Prerequisites

Install `pos-decorate` from the companion repo:

```bash
git clone https://github.com/MichaelRWolf/pos-decorate.git
cd pos-decorate
pip install -e .
```

## Generate Outputs from the Speech

All commands run from the root of this repo.

### Verify the tokenizer round-trips cleanly

```bash
pos-decorate speech.md --verify-parse-and-regenerate
```

Parses `speech.md`, regenerates the text from the parse tree, and diffs the result
against the original. Expects an empty diff. Exit code 0 = clean.

### Inspect the parse tree (debug)

```bash
pos-decorate speech.md --style=parse-tree
```

Shows each token with its POS tag and trailing whitespace. Useful for verifying that
punctuation and line breaks are preserved correctly.

### Plain text with POS prefixes (default)

```bash
pos-decorate speech.md
# or explicitly:
pos-decorate speech.md --style=plain
```

### HTML with all four view modes

```bash
pos-decorate speech.md --style=html > speech_output.html
open speech_output.html
```

The HTML file supports four interactive views toggled by buttons in the page:

| View         | Description                                |
| ------------ | ------------------------------------------ |
| As Written   | Plain prose, no annotation                 |
| POS-Tagged   | POS prefix annotations as black/white text |
| Tag-Receding | Annotations dimmed, prose in full black    |
| Grammar-Lit  | Words color-coded by POS class             |

### Other style variants

```bash
pos-decorate speech.md --style=raw-nltk   # NNS_Friends, VBP_come
pos-decorate speech.md --style=camel      # strFriends, vCome
```

## Regenerate after editing the speech

If `speech_draft.md` changes, rerun:

```bash
pos-decorate speech_draft.md --verify-parse-and-regenerate && \
pos-decorate speech_draft.md --style=html > speech_output.html
```
