---
name: code-comments
description: A guide to writing better code comments and docstrings — precise, concise comments that explain what the code does and why, and that stay true as the code evolves. Builds on the serre-style skill and adds rules for keeping comments free of change history and unsupported claims, for updating comments during refactoring, and for preserving existing ones. Use whenever writing, editing, or reviewing comments or docstrings, and when refactoring or moving commented code.
---

# Code Comments

A comment states what the code is and why it is so; the version-control
history records how the code came to be so.

**Conventions.** *Comment* includes docstrings and file or module headers.
*The diff* is the change being written or reviewed. "Rule 2.3" refers to this
skill; "serre-style 6.4" refers to rule 6.4 of `serre-style`.

**Prerequisite.** This skill extends `serre-style`. Apply all of serre-style to
every comment; the rules most often relevant are serre-style 2.4 (replace vague
words), 3.4 and 3.5 (a statement is complete and usable in isolation), 6.4
(comments say *why*), and 7.5 (keep the text true as things change). The rules
below add what serre-style does not cover: how comments relate to time and
change.

---

## 1. Describe the present, never the history

**1.1** A comment describes the code as it now stands. It never says what the
code used to be, what it replaced, where in the repository it came from, or
when it changed.

**1.2** History belongs in the commit message, the pull-request description,
or the changelog. A comment that records it states it a second time
(serre-style 1.3), and after the next diff the comment no longer describes the
code (serre-style 7.5).

**1.3** If a comment about a change contains a fact or a reason that holds for
the present code, state that fact or reason in the present tense and drop the
rest.

*Example (1.3).* Before:

```python
# Fixed: used to crash on empty files, now returns [] instead.
def load_records(path):
```

After:

```python
# Returns [] if the file at `path` is empty.
def load_records(path):
```

The comment states only the contract of `load_records` (serre-style 3.4, 3.5).
Why a caller depends on that contract, if worth recording, is said at the
caller.

**1.4** The following are not history, and are allowed:
- the source of the present code (serre-style 7.3): "Binary GCD; Knuth,
  TAOCP Vol. 2, §4.5.2, Algorithm B";
- the status of the present code (serre-style 7.1): `TODO(#123)`,
  *Experimental*, a deprecation notice naming the replacement and the tracking
  issue;
- a present fact about an older format or interface: "Also accepts v1
  records, which have no `version` field."

## 2. No claims of being better or improved

**2.1** Do not write that code is better, improved, enhanced, cleaner, more
robust, optimised, or modern. Each of these words compares the code with a
version the reader cannot see, and none says what the code does (serre-style
2.4).

**2.2** Replace the claim by the property it stands for: a complexity, a bound,
a guarantee, a measured figure.

*Example (2.2).* Before:

```python
# Improved, much faster lookup.
def find_user(index, user_id):
```

After:

```python
# Returns the user with id `user_id` in `index`, or None if there is none.
# O(1) on average.
def find_user(index, user_id):
```

**2.3** A comparison is allowed only with a named alternative that exists now,
and only together with the condition under which it holds and how it was
established (serre-style 6.1, 7.1): "About 3× faster than `json.loads` on
inputs over 1 MB (`bench/parse.py`)." A comparison with a past version of the
same code is history (1.1).

## 3. When refactoring

**3.1** Before finishing the diff, reread every comment that describes code the
diff changes. If a comment is no longer true, correct or delete it in the same
diff (serre-style 7.5).

**3.2** Delete comments that describe behaviour the code no longer has. Do not
replace them with a comment about the change.

**3.3** Do not add comments that narrate the refactoring ("extracted from
`main`", "split into two functions"). If the new code needs a comment, it is a
*why* comment about the new code (serre-style 6.4).

**3.4** When code moves, its comments move with it, unchanged unless 3.1
requires otherwise. Do not add "moved from …". Check that references to moved
or renamed code ("see `parse_header` in `io.py`") are still correct
(serre-style 5.2).

*Example (3.2, 3.3).* Before:

```python
# OLD: compared timestamps as strings.
# NEW: refactored to parse them first, which is more robust.
def is_expired(token, now):
```

After:

```python
# Compares `token.expires_at` and `now` as datetimes, not as strings: string
# order differs from time order when the two have different UTC offsets.
def is_expired(token, now):
```

## 4. Preserve existing comments

**4.1** Keep every existing comment, except as 4.2–4.4 provide. A comment you
did not write may record a reason you do not know.

**4.2** If an existing comment is false, correct or delete it in the same diff
(serre-style 7.5).

**4.3** If an existing comment describes history or makes a comparison,
rewrite it by 1.3 or 2.2. Delete it only if nothing in it is true of the
present code.

**4.4** A comment that paraphrases the code is noise (serre-style 6.4), but
remove it only when the task is to review or clean up comments. In a diff with
another purpose, the deletion enlarges the diff and hides its real change.

**4.5** If you cannot tell whether a comment is still true, keep it and tell
the user which comment is in doubt and why. Do not guess.

*Example (4.1).* After a rewrite of `fetch` that keeps its retry policy, this
comment stays unchanged:

```python
# Retries a failed request up to 3 times: the upstream gateway drops about 1%
# of requests (INC-2291).
def fetch(url):
```

## 5. Red flags

Each word below signals a probable violation of §1 or §2. On meeting one,
check whether it describes the history or the quality of the code; if it does,
rewrite the comment by 1.3 or 2.2.

- Change: *refactored*, *rewritten*, *replaced*, *moved*, *renamed*,
  *migrated*, *switched to*, *changed from*, *unified*, *fixed*.
- Time: *now*, *new*, *old*, *legacy*, *previously*, *originally*, *used to*,
  *was*, *recently*, *no longer*, *anymore*, *as of v2*.
- Comparison: *better*, *improved*, *enhanced*, *cleaner*, *more robust*,
  *optimised*, *modern*, and *faster* or *simpler* without a named
  alternative (2.3).

The same words may describe what the code does: "creates a new connection"
and "returns the old value of the counter" are correct.

## 6. Common rationalisations

| The thought | Why it fails |
|---|---|
| "Reviewers need to see what I changed." | The commit message and the pull request show reviewers the change. The comment stays in the code after the review ends. |
| "The comment explains why I refactored." | Explain why the present code is as it is (serre-style 6.4). Why it changed goes in the commit message (1.2). |
| "This historical comment still tells the reader something." | Keep what is true of the present code, rewritten by 1.3; delete the rest (4.3). |
| "I'll mark it as new for now." | The mark stays after the code is no longer new. Describe what the code is; if it is provisional, mark its status (serre-style 7.1). |
| "Saying it is faster reassures the reader." | An unquantified claim is vague (serre-style 2.4) and cannot be checked (serre-style 7.2). State the bound or the measurement (2.2, 2.3). |
| "This redundant comment is harmless to delete." | Not in a diff with another purpose (4.4). |

## 7. Verification

Before finishing the diff, check:

- [ ] Every added or changed comment describes the code as it now is, and says
  what it is or why it is so (§1).
- [ ] No red-flag word (§5) describes the history or the quality of the code.
- [ ] Every comparison names a present alternative, the condition under which
  it holds, and how it was established (2.3).
- [ ] Every comment that describes changed code is still true; comments about
  removed behaviour are gone; references to moved or renamed code still
  resolve (§3).
- [ ] Every comment the diff removes was false or held nothing true of the
  present code, or the task was a comment cleanup (§4).
- [ ] What the diff changes, and why, is stated in the commit message, not in
  the code (1.2).
- [ ] **serre-style check.** Every added or changed comment passes the
  serre-style checklist (serre-style 9.3) and is a self-contained statement
  about the code it describes (serre-style 3.4, 3.5).

---

## Applying this skill

**When writing.** Draft the comment as serre-style prescribes, then check it
against §7.

**When reviewing.** Cite the rule and give the rewrite, as serre-style does:

> (1.1) "now uses a heap instead of sorting" → "keeps a heap of size k, so the
> cost is O(n log k)".

**Existing conventions win.** If a project requires history in source files,
for example a mandated change-log header, follow the project.
