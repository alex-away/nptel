# Logic Coverage Basics

Logic-based testing focuses on testing the **logical decisions** in code (like `if` statements) rather than just the flow.

## 1. Core Terms

- **Predicate**: A logical expression that evaluates to True or False. Found in `if`, `while`, `for` conditions.
  - Example: `(x > y) && (z == 0)`

- **Clause**: An atomic part of a predicate without logical operators. The smallest piece you can test.
  - Example: In `(x > y) && (z == 0)`, the clauses are `(x > y)` and `(z == 0)`.

## 2. Basic Coverage Criteria

### Predicate Coverage (PC)
- The predicate must be True at least once and False at least once.
- Same as Branch Coverage.
- **Weakness**: Can miss bugs in individual clauses.

**Example**: For $P = a \vee b$:
- Test 1: $a = True, b = True$ → True
- Test 2: $a = False, b = False$ → False
- Satisfies PC, but never tested $b$ independently.

### Clause Coverage (CC)
- Each clause must be True at least once and False at least once.

**Example**: For $P = a \vee b$:
- Test 1: $a = True, b = False$
- Test 2: $a = False, b = True$
- Both clauses tested.

### Combinatorial Coverage (CoC)
- Test every possible combination (entire truth table).
- Requires $2^n$ tests for $n$ clauses.
- Often infeasible.

## 3. The Problem: CC Does NOT Subsume PC

**Counterexample**: For $P = a \vee b$:

Tests for CC:
1. $a = True, b = False$ → $P = True$
2. $a = False, b = True$ → $P = True$

Both clauses tested, but $P$ never evaluated to False. Fails PC!

## 4. Determination (The Key Concept)

A clause **determines** a predicate if flipping that clause's value (while keeping others fixed) causes the predicate to flip.

**Think of it like this**: Does changing this one clause actually matter? Or is the outcome already decided by the other clauses?

**Example**: For $P = a \wedge b$:
- If $b = True$:
  - $a = True$ → $P = True$
  - $a = False$ → $P = False$
  - Flipping $a$ flipped the result! So $a$ determines $P$ when $b = True$.

- If $b = False$:
  - $a = True$ → $P = False$
  - $a = False$ → $P = False$
  - Flipping $a$ did nothing! The result is always False because $b = False$. So $a$ does NOT determine $P$ when $b = False$.

**The pattern for AND**: A clause determines an AND only when all other clauses are True.
**The pattern for OR**: A clause determines an OR only when all other clauses are False.

## 5. The XOR Formula (The Shortcut)

Instead of manually checking all cases, use this formula to find when clause $c$ determines predicate $P$:

$$P_c = P_{c=true} \oplus P_{c=false}$$

**What XOR means**: XOR (⊕) returns True only when the two inputs are different. So this formula asks: "Are the two versions of P different?"

**Steps**:
1. Replace $c$ with True in $P$ → get $P_{c=true}$
2. Replace $c$ with False in $P$ → get $P_{c=false}$
3. XOR them → this tells you WHEN $c$ determines $P$

### Simplification Rules (Important!)

After applying the XOR formula, you'll need to simplify. Here are the key rules:

**XOR Properties**:
- $X \oplus False = X$
- $X \oplus True = \neg X$
- $X \oplus X = False$

**De Morgan's Laws**:
- $\neg(A \wedge B) = \neg A \vee \neg B$
- $\neg(A \vee B) = \neg A \wedge \neg B$

**Advanced Simplifications** (these show up in complex predicates):
- $A \oplus (A \vee C) = \neg A \wedge C$
- $A \oplus (A \wedge C) = A \wedge \neg C$

### Example 1: For $P = a \vee b$, when does $a$ determine $P$?

**Step 1 & 2**: Create the two versions:
- $P_{a=true} = True \vee b = True$ (OR with True is always True)
- $P_{a=false} = False \vee b = b$ (OR with False is just $b$)

**Step 3**: XOR them:
- $P_a = True \oplus b = \neg b$

**Answer**: $a$ determines $P$ when $b = False$.

**Why this makes sense**: If $b = True$, then $P$ is always True regardless of $a$. But if $b = False$, then $a$ actually controls the outcome.

### Example 2: For $P = a \wedge b$, when does $a$ determine $P$?

**Step 1 & 2**: Create the two versions:
- $P_{a=true} = True \wedge b = b$ (AND with True is just $b$)
- $P_{a=false} = False \wedge b = False$ (AND with False is always False)

**Step 3**: XOR them:
- $P_a = b \oplus False = b$

**Answer**: $a$ determines $P$ when $b = True$.

**Why this makes sense**: If $b = False$, then $P$ is always False regardless of $a$. But if $b = True$, then $a$ actually controls the outcome.

### Example 3: Complex predicate $P = a \wedge (b \vee c)$

**For clause $a$**:
- $P_{a=true} = True \wedge (b \vee c) = (b \vee c)$
- $P_{a=false} = False \wedge (b \vee c) = False$
- $P_a = (b \vee c) \oplus False = b \vee c$
- **Answer**: $a$ determines $P$ when at least one of $b$ or $c$ is True.

**For clause $b$**:
- $P_{b=true} = a \wedge (True \vee c) = a \wedge True = a$
- $P_{b=false} = a \wedge (False \vee c) = a \wedge c$
- $P_b = a \oplus (a \wedge c) = a \wedge \neg c$
- **Answer**: $b$ determines $P$ when $a = True$ AND $c = False$.

**For clause $c$**:
- By symmetry: $P_c = a \wedge \neg b$
- **Answer**: $c$ determines $P$ when $a = True$ AND $b = False$.
