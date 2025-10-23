# Input Grammar Mutation

## 1. Input Grammar Mutation vs. Program Mutation

It's crucial to understand the difference:

- **Program Mutation**: Mutate the **source code** → create a **mutant program**. Then, write **test cases** to kill the mutant program. The test case is *separate* from the mutant.

- **Input Grammar Mutation**: Mutate the **grammar rules** of the *input* (e.g., for XML, commands, etc.) → generate a **mutated input string**. This string **is the test case** itself. There is no "killing"; you simply run this invalid input and check if the program handles it correctly (e.g., throws an error, rejects it gracefully).

- **Think of it like this**: Program mutation asks "Does my test catch bugs in the code?" Input grammar mutation asks "Does my program handle bad input correctly?"

## 2. Why Mutate Input Grammars?

Programs often accept complex, structured inputs defined by a grammar (like CFGs). While deriving valid inputs from the grammar is important, mutating the grammar is the primary way to generate **invalid (malformed) inputs**.

- **Purpose**: Testing with invalid inputs is crucial for checking:
  - Error handling
  - Fault tolerance
  - Security vulnerabilities (e.g., buffer overflows, injection attacks caused by unexpected input structure)

- **Example: XML**: An XML file's structure is defined by its schema (XSD), which acts as the grammar. Mutating the schema rules generates malformed XML files used to test the robustness of an XML parser.

## 3. Four Operators for Mutating Grammars

These operators modify the production rules of the input grammar to generate invalid strings.

### I. Non-Terminal Replacement (NTR)

- **What it does**: Replaces a non-terminal on the right-hand side (RHS) of a rule with a *different* non-terminal.

- **Example**:
  - **Rule**: $\langle deposit \rangle \rightarrow \text{'deposit'}\ \langle amount \rangle\ \langle account \rangle$
  - **Mutant Rule**: $\langle deposit \rangle \rightarrow \text{'deposit'}\ \langle amount \rangle\ \langle amount \rangle$ (Replaced $\langle account \rangle$ with $\langle amount \rangle$)
  - **Resulting Invalid Input**: "deposit 12.35 12.35"

### II. Terminal Replacement (TR)

- **What it does**: Replaces a terminal on the RHS of a rule with a *different* terminal.

- **Example**:
  - **Rule**: $\langle amount \rangle \rightarrow \text{'\$'}\ \langle digits \rangle\ \text{'.'}\ \langle digits \rangle$
  - **Mutant Rule**: $\langle amount \rangle \rightarrow \text{'.'}\ \langle digits \rangle\ \text{'.'}\ \langle digits \rangle$ (Replaced '\$' with '.')
  - **Resulting Invalid Input**: ".12.35"

### III. Terminal/Non-Terminal Deletion (TND/NTD)

- **What it does**: Deletes a terminal or non-terminal from the RHS of a rule.

- **Example**:
  - **Rule**: $\langle deposit \rangle \rightarrow \text{'deposit'}\ \langle amount \rangle\ \langle account \rangle$
  - **Mutant Rule**: $\langle deposit \rangle \rightarrow \text{'deposit'}\ \langle amount \rangle$ (Deleted $\langle account \rangle$)
  - **Resulting Invalid Input**: "deposit 12.35"

### IV. Terminal/Non-Terminal Duplication (TNR/NTR) - "Stuttering"

- **What it does**: Duplicates a terminal or non-terminal on the RHS of a rule.

- **Example**:
  - **Rule**: $\langle deposit \rangle \rightarrow \text{'deposit'}\ \langle amount \rangle\ \langle account \rangle$
  - **Mutant Rule**: $\langle deposit \rangle \rightarrow \text{'deposit'}\ \text{'deposit'}\ \langle amount \rangle\ \langle account \rangle$ (Duplicated 'deposit')
  - **Resulting Invalid Input**: "deposit deposit 12.35 739"

- **Think of it like this**: The grammar "stutters" and repeats a symbol, creating malformed input that tests if the parser can detect and reject the repetition.

---

**Key Insight**: These mutated rules are then used in the derivation process to generate malformed input strings, which serve directly as test cases.
