---
name: serre-style
description: Write, rewrite, or review text in the style of Jean-Pierre Serre — precise, economical, well-ordered, and honest about what is known. Applies equally to mathematics (papers, proofs, lecture notes), to code (naming, structure, comments, docstrings), and to documentation (READMEs, API references, design notes). Use when the user asks for "Serre style" or "write like Serre", or asks to write, tighten, restructure, or review mathematical text, code comments, docstrings, or technical documentation for clarity and concision.
---

# Serre Style

Say exactly what is true, in as few words as the reader needs, in an order the reader can follow.

**Terminology.** *Text* means any written artifact: a proof, a function, a
comment, a README. *Statement* means any claim a text makes: a theorem, a
function's contract, a sentence of documentation. *Reader* means whoever must
understand the text: a mathematician, a maintainer, a user, an agent.

**Principle 0.** The reader's time is worth more than the writer's. Every rule
below follows from it. If, in a concrete case, a rule makes the text harder for
the intended reader, drop the rule.

---

## 1. Economy

**1.1** Delete every word that carries no information.
Typical filler: "it is worth noting that", "note that", "basically", "in order
to", "actually", "very", "the fact that", "it should be mentioned that",
"powerful", "simply", "just".

**1.2** Write short sentences, one idea each. A sentence with three clauses is
usually three sentences.

**1.3** Say each thing once, in the right place. A fact stated twice will
eventually be stated two different ways. In code and documentation, link to the
single source instead of copying it.

**1.4** Economy is not compression. Never omit a step the intended reader cannot
supply alone. Terse is good; cryptic is not.

**1.5** Choose the level of generality the applications need, no more. A theorem
proved in a generality nobody uses, a function with parameters nobody passes, an
abstraction with one implementation: each costs the reader and buys nothing.

## 2. Precision

**2.1** Each statement has exactly one meaning. If a second reading is possible,
rewrite until it is not.

**2.2** Make the order and scope of quantifiers explicit. "There is M such that
|f(x)| ≤ M for every x" and "for every x there is M such that |f(x)| ≤ M" say
different things; "|f(x)| ≤ M for all x for some M" says neither clearly. The
same holds for "once per request" versus "once for all requests".

**2.3** Use "the" only for a unique object; otherwise use "a". "Returns the
index of x" is false if x can occur twice.

**2.4** Replace vague words by the condition they stand for: "suitable",
"appropriate", "nice", "reasonable", "properly", "handles", "deals with",
"various", "etc.", "and so on". "Handles invalid input" → "raises `ValueError`
if `n < 0`".

**2.5** Every pronoun ("it", "this", "they", "which") has one obvious
antecedent. Otherwise repeat the noun. A bare "This" at the start of a sentence
almost always needs a noun: "This bound", "This call".

**2.6** State the boundaries: strict or non-strict inequality, inclusive or
exclusive range, units, the empty case, zero, what happens on failure.

## 3. Statements: hypotheses and conclusion

**3.1** Put every hypothesis in the statement itself. Do not hide one in a
convention twenty pages earlier, in the proof, or in a footnote. If a global
convention is unavoidable, state it once in a clearly marked *Conventions*
paragraph and recall it where it matters.

**3.2** Every hypothesis is used. If a hypothesis is not needed, drop it, or say
why it is there ("the hypothesis p ≠ 2 is needed only in §4").

**3.3** State before proving. The reader must know what is being shown before
the argument starts. In documentation, give the answer first and the derivation
after.

**3.4** A function is a theorem. Its preconditions are the hypotheses; its
return value and effects are the conclusion; its body is the proof. Write the
signature and docstring as a statement: complete, self-contained, and silent
about *how*.

**3.5** A statement must be usable in isolation. A reader who sees only the
theorem (or only the signature and docstring, or only one section of the
documentation) must be able to apply it correctly.

## 4. Notation and naming

**4.1** One name per concept, one concept per name, throughout the whole text.
Do not let `n` be the degree on page 3 and the dimension on page 7; do not call
the same object `user`, `account`, and `member` in three modules.

**4.2** Define every symbol and term at or before its first use, never after.

**4.3** Do not introduce notation used only once or twice. Every name costs the
reader memory.

**4.4** Keep notation light and conventional: few indices, no nested
sub-subscripts, the letters and names the field already uses (n for an integer,
G for a group, `i` for a loop index, the standard library's vocabulary).
Break a convention only for a reason, and say so.

**4.5** Words in prose, symbols in formulas. In sentences, do not use ∀, ∃, ⇒,
∴, "iff", "s.t.", "w.r.t.", "cfg", "b/c" as abbreviations.

**4.6** Do not begin a sentence with a symbol. Do not let two formulas touch
with only a comma between them; put words there. "For p odd p² ≡ 1 (mod 8)"
→ "If p is odd, then p² ≡ 1 (mod 8)."

**4.7** A name says what a thing *is* or *does*, not how it is built or what type
it has. Abbreviate only what every reader already knows.

## 5. Structure

**5.1** Number everything the reader may need to refer to: sections, theorems,
equations, steps, rules. Use hierarchical numbering (2.3, 2.3.1).

**5.2** Refer precisely: "by Lemma 2.3", "see §4.1", "see `parse_header` in
`io.py`", "RFC 9110, §15.5.1", "[Serre, *Local Fields*, Ch. IV, §1]". Never "by
the above", "as we saw", "as mentioned earlier", "see elsewhere".

**5.3** Begin every unit (chapter, section, module, README) with one or two
sentences saying what it does and what it assumes. List prerequisites
explicitly.

**5.4** Separate the main line from digressions. Remarks, variants, history,
alternative proofs go in clearly marked *Remarks* that can be skipped. Tell the
reader what can be skipped: "The reader interested only in X may go directly to
§5." In code, keep the main path visible and isolate edge-case handling; in a
README, put the quick start before the details.

**5.5** Order by dependency: nothing relies on what comes later.

**5.6** Keep units short. A proof longer than a page becomes lemmas; a function
that does three things becomes three functions; a section that answers two
questions becomes two sections.

## 6. Justification

Applies to proofs, implementations, and design rationales alike.

**6.1** Justify every step, by an argument or by a precise reference.

**6.2** Write "clearly", "obviously", "trivially", "it is easy to see" only if
you have just checked it and the check is short *for the intended reader*.
Better: replace the word by the one-line reason. "Clearly f is injective" →
"f is injective, since g ∘ f = id."

**6.3** A routine verification may be omitted only if it is named: "a direct
computation shows", "the proof of (b) is the same as that of (a), with ≤
replaced by <". Never omit silently.

**6.4** Comments say *why*; the code says *what*. A comment that paraphrases the
code is noise: delete it. A comment that records a reason, an invariant, or an
external constraint is essential: keep it next to the line it explains.

**6.5** Every non-obvious step carries its justification where it occurs: the
clever substitution, the unusual loop bound, the deliberate absence of a lock.

## 7. Honesty

**7.1** Mark the status of every claim: proved, conjectured, believed, tested,
untested, planned. Use explicit words: *Conjecture*, "I do not know whether",
`TODO(#123)`, *Experimental*, "not yet implemented".

**7.2** Never claim more than you have. A README does not describe features that
do not exist; a docstring does not promise behaviour the code lacks; a paper
does not present folklore as an established result.

**7.3** Credit precisely: who, and where (with theorem or page number). In code,
link to the source of an algorithm or adapted snippet.

**7.4** State limitations: where the method fails, what is not covered, known
bugs. A counterexample showing that a hypothesis cannot be dropped is part of
the result.

**7.5** Keep the text true as things change. An outdated comment or document is
worse than none: update it or delete it in the same change that made it false.

## 8. Examples

**8.1** Follow each definition with an example: the simplest non-trivial one.

**8.2** Where a hypothesis or definition might look too strong, give a
counterexample showing it is needed.

**8.3** In documentation, put a minimal, complete, runnable example near the
top. It must work when copied.

**8.4** Examples illustrate a statement; they never replace it.

## 9. Revision

**9.1** Reread as the reader, not as the writer, preferably after some time has
passed.

**9.2** Cutting is the main act of revision. If unsure whether a sentence is
needed, delete it and reread; restore it only if something is missing.

**9.3** Checklist:
- Is every word needed? (1.1)
- Does every statement have one meaning? (2.1–2.5)
- Is every symbol and name defined before use, and used consistently? (4.1, 4.2)
- Is every hypothesis in the statement, and is each one used? (3.1, 3.2)
- Is every reference precise, and still correct after the last edit? (5.2)
- Is every "clearly" justified? (6.2)
- Is the status of every claim marked? (7.1)
- Can each unit be read knowing only what precedes it? (5.5)

---

## Applying this skill

**When writing.** Draft for the intended reader, then revise with the checklist
(9.3). Write the statement (theorem, signature, docstring, section summary)
before the proof or body.

**When reviewing someone's text.** Cite the rule and give the rewrite, not only
the complaint:

> (2.4) "handles empty input" → "returns `[]` if `items` is empty".

Order the findings by consequence: violations of precision (§2), hypotheses
(§3), and honesty (§7) make a text *wrong* and come first; structure (§4, §5)
next; economy (§1) last.

**Existing conventions win.** A journal's style, a project's style guide, or a
language's idioms take precedence over these rules where they conflict.

**Examples.** `references/examples.md` contains before/after pairs for
mathematics, code, and documentation, each tagged with the rules it
illustrates. Read it when the user wants a rewrite or when a rule's application
is unclear.
