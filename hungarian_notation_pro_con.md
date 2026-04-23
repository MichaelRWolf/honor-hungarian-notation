# Hungarian Notation: Pro and Con

## Historical Timeline

| Year | Event                                                               |
| ---- | ------------------------------------------------------------------- |
| 1967 | BCPL appears — single type, the machine word. No type system.       |
| 1969 | B appears — typeless language descended from BCPL.                  |
| 1972 | C appears — weak types, no enforcement at the edges.                |
| 1984 | Microsoft Word 1.1 ships — Simonyi's HN is in its source.           |
| 1985 | C++ appears — the first typed successor C programmers encounter.    |
| 1990 | Windows API (Win32) codifies Systems Hungarian as house style.      |
| 1993 | Visual Basic arrives with IDE type hints. The case starts to close. |
| 2005 | Joel Spolsky publishes "Making Wrong Code Look Wrong" — draws the   |
|      | Apps/Systems distinction. The mutation is named.                    |
| 2008 | IntelliJ, Eclipse, Visual Studio — hover type display is standard.  |
| Now  | Every mainstream language has a type system. Every IDE shows it.    |

---

## The Two Hungarian Notations (critical distinction)

Simonyi invented **Apps Hungarian**. The Windows team deployed **Systems Hungarian**.
They are not the same thing. Almost everyone practicing HN today is practicing the mutation.

### Apps Hungarian (Simonyi's original)

Prefixes encode **kind** or **purpose** — a semantic distinction, not a type.

Examples:

- `xPos`, `yPos` — both integers, but one is a column, one is a row; mixing them is a bug
- `usName`, `unsName` — "unsafe" vs. "safe" string (security context)
- `rwRecord`, `colRecord` — row vs. column index of the same record

The prefix answers: "what is this FOR?" — not "what type is it?"
Simonyi's insight: the compiler can't catch `xPos = yPos` (both `int`); the human prefix can.
This was a legitimate tool for catching semantic errors the type system couldn't see.

### Systems Hungarian (the Microsoft mutation)

Prefixes encode **type** — information the compiler already has.

Examples:

- `nCount` — integer
- `szName` — null-terminated string
- `lpszTitle` — long pointer to null-terminated string
- `bFlag` — boolean
- `dwSize` — DWORD (unsigned 32-bit int)
- `cbBuffer` — count of bytes

The prefix answers: "what type is this?" — information redundant with the declaration.
This is what nearly everyone means when they say "Hungarian Notation."
This is what The Practitioners practice and defend.

**The deepest irony:** When The Practitioners defend HN, they are defending an idea
(Simonyi's Apps Hungarian) while practicing its mutation (Systems Hungarian). They have
the wrong thing right and the right thing wrong.

---

## The Original Case For HN (sincere — era-appropriate)

These were real problems. HN solved them. The era that contained them is gone.

### 1. No type system

BCPL had one type: the machine word. B had none. Early C was weakly typed and
inconsistently enforced. A variable could hold anything. There was no compiler to catch
a type mismatch. The programmer's memory was the only type checker.

A prefix on the variable name was a mnemonic against misuse. In this context, it was
not noise — it was the only signal.

### 2. No IDE

No hover-to-inspect. No autocomplete with type signatures. No inline error highlighting.
No "go to definition" across files. The programmer read code from a printout or a terminal
without syntax color, context, or tooling assistance.

In a 10,000-line C file, `lpszTitle` told you what `Title` was without forcing you to
scroll to its declaration. This was a genuine comprehension aid.

### 3. No static analysis

No linters. No type checkers. No Mypy, Pyright, ESLint, or clang-tidy. If you passed
the wrong thing, you found out at runtime — or not at all, if the memory layout happened
to look right.

### 4. Cognitive load reduction

The prefix offloaded type-tracking from working memory. In a session with 40 variables,
a systematic mnemonic system reduced the chance of a class of error. This was a practical
engineering choice, not cargo cult ritual.

### 5. Code review and pair programming

A reviewer reading someone else's C code needed to know, at a glance, what each variable
held. The declaration might be a screen away. A prefix brought the type to the point of use.

### 6. Cross-function and cross-file readability

Functions were long. Files were long. Libraries had no doc strings that an IDE could surface.
The prefix was a compressed documentation format that survived copy-paste and refactoring
in an era when refactoring meant search-and-replace.

---

## The Practitioners' Present Case (what they claim today)

The Practitioners defend HN on these grounds. Each is examined below.

### Claim A: "It makes the type visible at the point of use"

**Then:** True. The compiler did not do this. The IDE did not do this.
**Now:** The compiler does this. The IDE does this. The claim is redundant.

### Claim B: "It reduces cognitive load"

**Then:** True. No tooling offloaded this burden; the programmer carried it.
**Now:** The tooling carries it. HN adds a second thing to read, not a reduction.
Memorizing `lpsz`, `dw`, `cb`, `cch`, `h`, `r`, `w` is a cognitive load, not a reduction.

### Claim C: "It makes code self-documenting"

**Then:** Partially true. Better than nothing.
**Now:** Good names, types, and short functions document themselves. HN's contribution
is negative: `nCount` is less readable than `count`; `bIsValid` is less readable than
`isValid`. The prefix obscures the semantic name by prepending a type annotation.

### Claim D: "It helps spot type mismatches in code review"

**Then:** True and useful. The reviewer had no other tool.
**Now:** The CI pipeline has a linter. The editor has a type checker. The PR has
inline type inference. The reviewer has better tools than eye-scanning prefixes.

### Claim E: "We've always done it this way"

This is not a defense. This is the cargo cult.

---

## The Case Against HN Today

### 1. Redundancy with type systems

Every mainstream language — Java, C#, Python 3, TypeScript, Go, Rust, Kotlin, Swift —
has a type system that tracks types more reliably than a naming convention. The prefix
duplicates information the compiler already holds. Duplication invites divergence.

### 2. Prefix rot on refactoring

`nCount` becomes a float when requirements change. The prefix remains. The name now lies.
A lying name is worse than no annotation: it actively misleads the reader. The longer
the codebase lives, the more `nCount`-style lies accumulate.

This is the inverse of the original promise. HN was supposed to add clarity. After
refactoring, it subtracts it.

### 3. The prefix dictionary problem

To read Win32 code, you must memorize:

| Prefix | Meaning                                |
| ------ | -------------------------------------- |
| lpsz   | long pointer to zero-terminated string |
| dw     | DWORD (unsigned 32-bit int)            |
| cb     | count of bytes                         |
| cch    | count of characters                    |
| h      | handle                                 |
| b      | BOOL (not bool — a 32-bit int)         |
| f      | flag (also a BOOL)                     |
| p      | pointer                                |
| lp     | long pointer                           |
| w      | WORD (unsigned 16-bit int)             |

This is a second language that sits on top of C. Fluency requires learning both.
A reader who does not know the dictionary cannot parse the code. This is the opposite
of self-documenting.

### 4. Visual noise and name burial

The semantic content of the variable — the part that tells you what it means — is
in the name, not the prefix. `nWindowCount` tells you as much as `windowCount` and
less than `openWindowCount`. The prefix occupies characters that could carry meaning.

### 5. Training-wheel effect

HN prevents learning to write self-documenting names. If the prefix carries the type,
the name need not. The result: `nX`, `bFlag`, `szBuf` pass review. The discipline
of naming — the most valuable skill in software — atrophies.

### 6. The literacy argument

A skilled reader of prose infers part of speech from context: grammar, position, relation
to other words. No annotation is needed. A skilled reader of code infers type from context:
the function called, the operation performed, the value returned. HN is training wheels
for readers who have not yet learned to read code.

If the text needs annotations to be read, the text is badly written. If the code needs
prefixes to be understood, the code is badly named. The fix is better naming, not more
prefixes.

---

## Summary Table

| Virtue claimed by HN         | Era-appropriate? | True today?                          |
| ---------------------------- | ---------------- | ------------------------------------ |
| Type visible at point of use | Yes (BCPL/C era) | No — compiler and IDE do this        |
| Cognitive load reduction     | Yes              | No — prefix dictionary adds load     |
| Self-documenting code        | Partial          | No — good names do this without cost |
| Code review aid              | Yes              | No — static analysis does this       |
| Refactoring safety           | Neutral          | No — prefix rot makes it harmful     |

---

## Notes for the Speech Draft

- **The founding irony:** Simonyi's original (Apps Hungarian) was genuinely clever.
  The mutation (Systems Hungarian) is what The Practitioners actually use.
  They are defending a thing they are not doing.

- **The stanza A hook:** "It was our friend... in the days before compilers tracked types."
  The virtue is real. The era that contained it is over.

- **The stanza B hook:** Comprehension / cognitive load were genuine gifts. Today
  the prefix dictionary is itself a comprehension burden. "Obscurity should be made
  of sterner stuff" — and yet, `lpszWindowTitle`.

- **The stanza C hook:** The IDE presents the type three times without asking.
  The prefix is a fourth presentation no one requested.

- **The refactoring rot hook (Move 2):** `nCount` becomes a float. The prefix stays.
  The lie accumulates. This is the deepest damage — HN turns helpful annotation into
  misinformation over time.

- **The prefix dictionary hook:** "You must memorize `lpsz` to read my code." This
  is not documentation. This is an access fee.

- **The training-wheels hook (Move 5):** HN prevents learning the thing it pretends to teach.
  Good naming makes type annotations unnecessary. HN makes that discipline optional, then
  atrophies it.
