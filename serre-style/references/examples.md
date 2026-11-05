# Serre Style: Examples

Before/after pairs. Rule numbers refer to `SKILL.md`.

Contents:
1. Mathematics (E1–E5)
2. Code (E6–E10)
3. Documentation (E11–E14)
4. Domain notes

---

## 1. Mathematics

### E1. Hidden hypothesis, vague reference, unjustified "obviously" (3.1, 5.2, 6.2)

Before:

> Obviously, by the above, f is injective since its kernel is trivial, so it is
> an isomorphism.

After:

> **Proposition 3.2.** Let G and H be finite groups of the same order, and let
> f : G → H be a homomorphism with trivial kernel. Then f is an isomorphism.
>
> *Proof.* By Lemma 3.1, f is injective. An injective map between finite sets
> of the same cardinality is bijective. ∎

The hypothesis "finite, of the same order" was used but never stated.

### E2. Quantifier order (2.2)

Before:

> We have |f(x)| ≤ M for all x for some M.

After:

> There is a constant M such that |f(x)| ≤ M for every x.

### E3. Symbols in prose, sentence starting with a formula (4.5, 4.6)

Before:

> ∀ε > 0 ∃δ > 0 s.t. |x − y| < δ ⇒ |f(x) − f(y)| < ε. f is then called
> uniformly continuous.

After:

> The function f is *uniformly continuous* if for every ε > 0 there is δ > 0
> such that |f(x) − f(y)| < ε whenever |x − y| < δ.

### E4. One letter, two meanings (4.1)

Before:

> Let n = dim V. … (three pages later) … For every n ≥ 1, the map φ_n is
> surjective.

After:

> Let d = dim V. … For every integer k ≥ 1, the map φ_k is surjective.

### E5. Definition without example; hypothesis without counterexample (8.1, 8.2)

Before:

> **Definition.** A ring A is *reduced* if it has no nonzero nilpotent elements.
>
> **Proposition.** If A is reduced and Noetherian, then A has finitely many
> minimal primes, and their intersection is 0.

After:

> **Definition 2.1.** A ring A is *reduced* if it has no nonzero nilpotent
> elements.
>
> *Example.* The ring ℤ is reduced; ℤ/4ℤ is not, since 2² = 0.
>
> **Proposition 2.2.** Let A be a reduced Noetherian ring. Then A has finitely
> many minimal prime ideals, and their intersection is 0.
>
> *Remark 2.3.* The intersection of all minimal primes of any ring is its
> nilradical, so the second assertion uses only that A is reduced. The
> Noetherian hypothesis is needed for the first: the ring of functions
> ℕ → 𝔽₂ is reduced and has infinitely many minimal primes.

---

## 2. Code

### E6. Comments that say what, not why (6.4, 1.1)

Before:

```python
# increment the counter
count += 1
# drop first row
rows = rows[1:]
```

After:

```python
count += 1
# The export tool always writes a header row, even for empty files.
rows = rows[1:]
```

### E7. Docstring as a theorem statement (3.4, 2.3, 2.4, 2.6)

Before:

```cpp
// Handles finding stuff in the vector. Returns the index.
int find(const std::vector<int>& v, int x);
```

After:

```cpp
/// Returns the smallest i with xs[i] == x, or std::nullopt if x does not occur in xs.
std::optional<std::size_t> first_index(std::span<const int> xs, int x);
```

"The index" was false when x occurs twice; "handles" said nothing; the case
"x absent" was missing, and the caller had to guess whether it returns -1.
The types now carry part of the statement: `std::span<const int>` says xs is
only read, and `std::optional` says the result may be absent. The comment
states the rest.

### E8. One concept, three names (4.1, 4.7)

Before:

```python
def load_user(uid): ...
def delete_account(account_id): ...
def member_permissions(m): ...
```

After:

```python
def load_user(user_id): ...
def delete_user(user_id): ...
def user_permissions(user_id): ...
```

All three take the same identifier, so it has one name. A parameter holding a
`User` object would be a different concept and would get a different name,
such as `user`.

### E9. Status of a claim (7.1, 7.2, 2.6)

Before:

```rust
// should work for negative n too
fn digits(n: i64) -> Vec<u8>
```

After:

```rust
/// Returns the decimal digits of `n`, most significant first; `digits(0)` is `[0]`.
fn digits(n: u64) -> Vec<u8>
```

Either prove the claim (test it) or restrict the statement. "Should work" is
neither. Here the type makes the restriction: with `u64`, the hypothesis
n ≥ 0 is checked by the compiler, so neither a comment nor a runtime check is
needed.

### E10. Generality nobody uses (1.5)

Before:

```python
def format_amount(
    value: float,
    currency_symbol: str = "€",
    symbol_position: str = "before",
    decimals: int = 2,
    thousands_sep: str = ",",
    decimal_sep: str = ".",
    negative_style: str = "minus",
) -> str:
    """Format an amount of money."""
```

in an application whose only caller prints euro totals on invoices, always
with the defaults.

After:

```python
def format_euros(cents: int) -> str:
    """Return cents as a euro amount, e.g. format_euros(123456) == '€1,234.56'."""
```

The first version asks the reader to understand six options that no caller
uses, and makes promises about combinations (`decimals=0` with
`negative_style="parentheses"`) that no test checks. As with a theorem, state
and prove it in the generality you use. Add a parameter when its first caller
exists.

---

## 3. Documentation

### E11. README opening (1.1, 5.3, 8.3)

Before:

> # TabIO
>
> Welcome to TabIO! TabIO is a powerful, flexible, and blazing-fast library
> that aims to make working with tabular data easier than ever. It is worth
> noting that TabIO handles many different formats.

After:

> # TabIO
>
> TabIO reads and writes CSV and Parquet files as NumPy arrays.
> Requires Python ≥ 3.10.
>
> ```python
> import tabio
> table = tabio.read("data.csv")      # dict[str, numpy.ndarray]
> tabio.write("data.parquet", table)
> ```

### E12. Imprecise cross-reference (5.2)

Before:

> As mentioned above, the cache must be configured first.

After:

> Set `cache_dir` before calling `load` (see §2.1, *Configuration*).

### E13. Overclaiming (7.2, 7.4)

Before:

> Supports Linux, macOS, and Windows.

After:

> Tested on Linux and macOS. Windows is not tested; see issue #57.

### E14. Answer first, derivation after (3.3, 5.4)

Before:

> We first tried a lock per bucket, but contention under the benchmark in
> `bench/hot.py` was high. Then we looked at lock-free lists, which were
> correct but slower for small maps. In the end we chose striped locks.

After:

> **Decision.** The map uses 64 striped locks.
>
> **Reason.** A lock per bucket showed high contention in `bench/hot.py`;
> lock-free lists were slower for maps under 10⁴ entries.
>
> *Remark.* Lock-free lists win above roughly 10⁶ entries; revisit if that
> becomes the typical size.

---

## 4. Domain notes

These specialize the general rules; they do not add new ones.

**Mathematics.**
- Punctuate displayed formulas as parts of sentences (4.6).
- Display only formulas that will be referred to or that are hard to read
  inline; number the ones that are referred to (5.1).
- Introduce objects with "Let …": "Let G be a finite group." State results with
  "Then …" (3.1, 3.3).
- Write "if and only if" in full in statements (4.5).

**Code.**
- The signature is part of the statement: precise types are hypotheses that
  the compiler checks (3.4).
- Prefer an assertion or a type over a comment saying "x must be positive";
  a checked hypothesis cannot become false silently (7.5).
- Delete dead and commented-out code; version control remembers it (1.1).
- Commit messages: first line states what the change does, in the imperative;
  the body says why (3.3, 6.4).

**Documentation.**
- One page answers one question; its title is that question or its answer
  (5.6).
- Give each concept its definition once, on one page, and link to it (1.3).
- Every code sample is complete and runs as shown (8.3).
