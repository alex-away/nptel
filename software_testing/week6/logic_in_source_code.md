# Applying Logic Coverage to Source Code

This focuses on the practical application of logic criteria to predicates in source code like `if`, `while`, and `switch` statements.

## 1. The Two Main Challenges

Applying logic criteria to source code is harder than to abstract specifications. Why?

**Reachability**: A test case consists of program inputs. These inputs must successfully execute the program path to reach the predicate you want to test.
- **Think of it like this**: You can't test a predicate inside an if statement if your inputs never make that if condition true.
- Example: If you want to test line 50, but it's inside `if (x > 100)`, your test input must have `x > 100` or you'll never reach line 50.

**Controllability**: Predicates often depend on internal variables (not direct program inputs). Your test case must indirectly control these internal variables to get the specific True/False clause values your test requires.
- **Think of it like this**: The predicate uses variable `x`, but `x` is calculated inside the program. You can't directly set `x = 5`. You have to figure out what inputs will make `x` become 5.
- Example: A predicate checks `if (balance > 0)`, but `balance` is calculated from `deposits - withdrawals`. You control `deposits` and `withdrawals`, not `balance` directly.

## 2. Solving for Internal Variables

The main technique is to map internal variables back to program inputs.

### The 4-Step Process

**Step 1: Identify Internal Variables**

Find predicates that use internal variables, not direct inputs.

**Example 1: Thermostat**

This program controls heating based on temperature and override settings.

```java
public void checkHeating(int currentTemp, int desiredTemp, 
                         boolean override, int overrideTemp) {
    int dTemp = desiredTemp - currentTemp;
    
    // Predicate: P = (A || (B && C)) && D
    // A: dTemp < 0 (current is warmer than desired)
    // B: override is true
    // C: currentTemp < overrideTemp
    // D: heaterWorking is true
    
    if (dTemp < 0 || (override && currentTemp < overrideTemp)) {
        if (heaterWorking) {
            turnOnHeater();
        }
    }
}
```

**The problem**:
- Predicate: $P = (A \vee (B \wedge C)) \wedge D$
- Clause $A$ is $dTemp < 0$, where `dTemp` is an internal variable.
- `dTemp` is calculated as: `dTemp = desiredTemp - currentTemp`
- You can't directly set `dTemp`. You control `desiredTemp` and `currentTemp` (the inputs).

**Example 2: TriType (Triangle Classifier)**

This program classifies triangles based on their side lengths.

```java
public int triType(int Side1, int Side2, int Side3) {
    int triout;
    
    // Count how many sides are equal
    if (Side1 == Side2)
        triout = 1;
    else
        triout = 0;
    
    if (Side1 == Side3)
        triout = triout + 2;
    
    if (Side2 == Side3)
        triout = triout + 3;
    
    // triout values:
    // 0 = no sides equal (scalene)
    // 1 = Side1 == Side2
    // 2 = Side1 == Side3
    // 3 = Side2 == Side3
    
    // Line 29: Check if it's an invalid triangle
    if (triout == 0) {
        if (Side1 + Side2 <= Side3 || Side2 + Side3 <= Side1 || Side1 + Side3 <= Side2)
            return 4; // Invalid triangle
        else
            return 1; // Scalene
    }
    // ... more code for other cases
}
```

**The problem**:
- Uses internal variable `triout` as a state counter.
- `triout` tracks how many sides are equal.
- You can't set `triout` directly. You control `Side1`, `Side2`, `Side3` (the inputs).

**Step 2: Create a Solution Table**

Map the internal variable's state to the input conditions that create it.

**TriType Solution Table**:

| $triout$ value | What it means | Input condition |
|----------------|---------------|-----------------|
| $triout = 0$ | No sides equal | $(Side1 \neq Side2) \wedge (Side1 \neq Side3) \wedge (Side2 \neq Side3)$ |
| $triout = 1$ | Exactly 2 sides equal | $(Side1 = Side2) \wedge (Side1 \neq Side3)$ |
| $triout = 2$ | All 3 sides equal | $(Side1 = Side2) \wedge (Side2 = Side3)$ |

**Why this helps**: Now when you see a predicate like `if (triout == 0)`, you know you need inputs where all three sides are different.

**Step 3: Create an Augmented Predicate**

Replace the internal variable with its input-based equivalent. This creates a bigger predicate, but one you can actually test with inputs.

**TriType Example**:
- Original predicate at line 29: $(triOut = 0) \wedge (Side1 + Side2 \leq Side3 \vee ...)$
- This uses internal variable `triOut`.
- From the solution table, `triOut = 0` means $(Side1 \neq Side2) \wedge (Side1 \neq Side3) \wedge (Side2 \neq Side3)$
- **Augmented predicate**: $((Side1 \neq Side2) \wedge (Side1 \neq Side3) \wedge (Side2 \neq Side3)) \wedge (Side1 + Side2 \leq Side3 \vee ...)$

**What changed**: We replaced `triOut = 0` with the actual input conditions. Now the predicate only uses inputs (`Side1`, `Side2`, `Side3`).

**Step 4: Generate Test Cases**

Now you can generate concrete test inputs for this augmented predicate using the techniques from Week 5.

**Thermostat Example**:
- Abstract test requirement: $(A = True, B = False, C = True, D = True)$
- Clause $A$ is $dTemp < 0$, which means $desiredTemp - currentTemp < 0$, which means $currentTemp > desiredTemp$.
- Concrete test inputs:
  - `setCurTemp(65)` → makes `currentTemp = 65`
  - `setDesiredTemp(60)` → makes `desiredTemp = 60`
  - Now `dTemp = 60 - 65 = -5 < 0`, so $A = True$
  - Set other inputs to make $B = False$, $C = True$, $D = True$

**The key insight**: You work backwards from the abstract test requirement to find concrete inputs that will create the right internal variable values.

## 3. The Predicate Transformation Pitfall

Some people try to "simplify" testing by transforming the code. This doesn't work.

**The Idea**: Transform code to avoid complex CACC testing.

Original:
```java
if (A && B) {
    S1;
}
```

Transformed:
```java
if (A) {
    if (B) {
        S1;
    }
}
```

**The Flawed Conjecture**: "If I apply simple Predicate Coverage (PC) to the transformed code, it's the same as applying CACC to the original code."

**Why people think this**: The transformed code has two separate predicates ($A$ and $B$), so testing each one True/False seems like it would cover everything.

**The Reality**: This is FALSE. The test requirements don't match.

**Counterexample**:
- For the transformed code with PC, you need:
  - Test 1: Make $A = True$ (to test the first if)
  - Test 2: Make $A = False$ (to test the first if)
  - Test 3: Make $B = True$ (to test the second if, requires $A = True$ to reach it)
  - Test 4: Make $B = False$ (to test the second if, requires $A = True$ to reach it)
  
- But for CACC on the original $(A \wedge B)$, you need different tests that show each clause determining the outcome.

**The problem**: You can satisfy PC on the transformed code without ever testing the case where $A = False$ and $B = True$, which CACC requires.

**Conclusion**: Don't modify code to avoid complex testing. It doesn't achieve the same test strength and makes code harder to maintain. Just apply CACC to the original code properly.
