# Syntax-Based Testing

## 1. What is Syntax-Based Testing?

This is a testing approach that uses the formal **syntax** (grammar) of a software artifact to systematically generate test cases.

- **The Core Idea**: Almost every software artifact—source code, inputs (like XML), or design models—is defined by a formal grammar. We use this grammar as a blueprint for testing.

- **The Goal**: We leverage these grammar rules to generate both **valid tests** (inputs that conform to the syntax) and **invalid tests** (inputs that violate the syntax).

## 2. The Three Levels of Syntax

A language's grammar is typically defined at three levels:

### I. Lexical Level (Words)

Defines how individual characters form valid "tokens" (like keywords, identifiers, or numbers). This level is specified using **Regular Expressions**.

### II. Syntactic Level (Phrases)

Defines how tokens are combined to form valid "statements" (like an `if` statement or a `for` loop). This is specified using a **Context-Free Grammar (CFG)**.

### III. Semantic Level (Context)

Defines context-sensitive rules that syntax alone can't cover (e.g., a variable must be declared before it is used, type-checking). This is specified by **Context-Sensitive Grammars**.

Syntax-based testing focuses mostly on the first two levels.

## 3. Context-Free Grammars (CFGs)

CFGs are used to define the "phrases" or sentence structure of a language.

- **Components**: A CFG has four parts:
  1. **Terminals**: The alphabet or final symbols (e.g., $a$, $b$, `if`, `(`).
  2. **Non-terminals**: Variables that represent parts of the structure (e.g., $\langle S \rangle$, $\langle X \rangle$, $\langle statement \rangle$).
  3. **Production Rules**: Rules for replacing non-terminals (e.g., $\langle S \rangle \rightarrow a\langle X \rangle$).
  4. **Start Symbol**: The non-terminal where all derivations begin (e.g., $\langle S \rangle$).

- **"Context-Free"**: This name means that a production rule (like $\langle X \rangle \rightarrow b\langle X \rangle$) can be applied to its non-terminal ($\langle X \rangle$) **regardless of the context** (the symbols surrounding it).

## 4. Grammar-Based Coverage Criteria

We can define coverage criteria based on the grammar rules themselves:

- **Terminal Symbol Coverage (TSC)**: Requires that each **terminal symbol** (e.g., $a$, $b$, $c$) in the grammar is included in at least one generated test case.

- **Production Coverage (PC)**: Requires that each **production rule** (e.g., $\langle S \rangle \rightarrow a\langle X \rangle$) in the grammar is used at least once to generate the test cases.

- **Derivation Coverage (DC)**: Requires that **every possible string** that can be derived from the grammar is tested.
  - This is almost always **infeasible** because grammars with recursion (e.g., $\langle X \rangle \rightarrow a\langle X \rangle$) can generate an infinite number of strings.

**Subsumption**: **Production Coverage (PC) subsumes Terminal Symbol Coverage (TSC)**, because if you use every rule, you will necessarily have to use every terminal symbol.
