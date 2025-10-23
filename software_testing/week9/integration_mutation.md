# Integration Mutation Testing

## 1. What is Integration Mutation Testing?

Integration mutation testing applies the principles of mutation testing to the **interfaces** and **connections** between different software modules or methods.

- **Focus**: Instead of mutating code *within* a single unit, it mutates the "call site" (where a method is called) or the "callee" (the method being called) to test the integrity of their interaction.

- **Think of it like this**: Unit mutation tests if a single function works correctly. Integration mutation tests if two functions work correctly *together* when one calls the other.

## 2. Generic Integration Mutation Operators

These operators can be applied to procedural languages (like C) and test the basic function/method call mechanism.

### I. IPVR (Integration Parameter Variable Replacement)

- **What it does**: Replaces a parameter variable in a method call with another variable of a *compatible type* from the current scope.

- **Example**: `G(a, b)` mutated to `G(c, b)` (where `c` is compatible with `a`).

- **Purpose**: Tests if the wrong variable is accidentally passed.

### II. IUOI (Integration Unary Operator Insertion)

- **What it does**: Inserts a unary operator (like `-`, `!`) into a parameter within a method call.

- **Example**: `G(a, b, c)` mutated to `G(-a, b, c)`.

- **Purpose**: Tests sensitivity to sign or boolean negation errors at the interface.

### III. IPE (Integration Parameter Exchange)

- **What it does**: Swaps two parameters of *compatible types* within a method call.

- **Example**: `G(a, b)` mutated to `G(b, a)`.

- **Purpose**: Tests for errors caused by incorrect parameter order.

### IV. IMCD (Integration Method Call Deletion)

- **What it does**: Deletes the entire method call.

- **Important**: If the original call returned a value that was used, the deleted call is replaced with a **constant or default value** to ensure the mutant still compiles.

- **Purpose**: Tests if the call was actually necessary or if its return value dependency is handled correctly.

### V. IREM (Integration Return Expression Modification)

- **What it does**: Mutates the `return` statement *inside the called method* (the callee).

- **How**: It re-uses the unit-level mutation operators (like AOR, ROR) to change the returned value.

- **Example**: `return x + y;` mutated to `return x - y;`.

- **Purpose**: Tests if the calling method correctly handles variations or errors in the returned value.

## 3. Object-Oriented (OO) Integration Mutation Operators

These operators specifically target features unique to OO languages like Java (inheritance, polymorphism, encapsulation). A tester selects a few relevant operators based on the feature being tested.

### I. Targeting Encapsulation (Information Hiding)

- **AMC (Access Modifier Change)**
  - **What it does**: Changes access modifiers (`public`, `private`, `protected`) of variables and methods.
  - **Example**: `public int x;` mutated to `private int x;`.
  - **Purpose**: Tests if accessibility rules are correctly enforced and relied upon.

### II. Targeting Inheritance

- **HVD (Hiding Variable Deletion)**
  - **What it does**: Deletes a variable in a *child* class that has the same name as one in the *parent*.
  - **Purpose**: Forces references to resolve to the parent's variable, testing if this causes unexpected behavior.

- **HVI (Hiding Variable Insertion)**
  - **What it does**: Adds a variable to a *child* class that hides an ancestor's variable.
  - **Purpose**: Tests if references correctly use the child's new variable instead of the ancestor's.

- **OMD (Overriding Method Deletion)**
  - **What it does**: Deletes a method in the *child* class that overrides a parent method.
  - **Purpose**: Forces calls to resolve to the parent's implementation, testing if the override was necessary or if the parent behavior is acceptable/incorrect.

- **OMM (Overriding Method Moving)**
  - **What it does**: Moves the `super.method()` call within an overriding child method (e.g., moves it up or down).
  - **Purpose**: Tests if the parent method needs to be called at a specific point, especially if the child needs to modify private parent state via the `super` call.
  - **Note**: Calls still initially resolve to the child method, but the *timing* of the parent's logic execution changes.

- **OMR (Overridden Method Rename)**
  - **What it does**: Renames the *parent's* version of a method that is overridden in the child.
  - **Purpose**: Tests for subtle errors where an *inherited* method in the child *still calls* the parent's original method name, but due to polymorphism, it now unexpectedly executes the child's *overriding* version.

- **SKD (Super Keyword Deletion)**
  - **What it does**: Deletes the `super.` qualifier when accessing a parent member.
  - **Example**: `super.x = 10;` mutated to `x = 10;`.
  - **Purpose**: Tests if there's ambiguity and if the code incorrectly accesses a local/child variable instead of the intended parent variable.

- **PCD (Parent Constructor Deletion)**
  - **What it does**: Deletes the explicit call to the parent constructor (e.g., `super();`) from a child constructor.
  - **Purpose**: Tests if the *default* parent constructor (if any) results in a different or incorrect object state.

### III. Targeting Polymorphism

- **ATC (Actual Type Change)**
  - **What it does**: Changes the type of object being instantiated.
  - **Example**: `Parent p = new Child();` mutated to `Parent p = new Sibling();` (where Sibling is another child of Parent).
  - **Purpose**: Tests if the code correctly handles different object types through the polymorphic reference.

- **DTC/PTC (Declared/Parameter Type Change)**
  - **What it does**: Changes the declared type of a variable or method parameter to an *ancestor* type (upcasting).
  - **Example**: `Child c;` mutated to `Parent c;`.
  - **Purpose**: Tests if the code relies on child-specific methods that are no longer accessible through the parent-type reference.

- **RTC (Reference Type Change)**
  - **What it does**: Changes the type of object being assigned to a reference, usually involving unrelated types.
  - **Example**: `Object o = myInteger;` mutated to `Object o = myString;`.
  - **Purpose**: Tests type compatibility and error handling during assignments.

### IV. Targeting Overloading

- **OMC (Overloading Method Change)**
  - **What it does**: Swaps the *bodies* of two overloaded methods (same name, different parameters).
  - **Purpose**: Tests if the correct overloaded version is being called based on the arguments.

- **OMD (Overloading Method Deletion)**
  - **What it does**: Deletes one of the overloaded methods.
  - **Purpose**: Tests if that specific overloaded version was actually necessary and correctly invoked, or if calls incorrectly resolve to another version after deletion.

- **AOC (Argument Order Change)**
  - **What it does**: Swaps the order of arguments in a method call to potentially match a *different* overloaded version.
  - **Purpose**: Tests sensitivity to argument order when overloading is involved.

- **ANC (Argument Number Change)**
  - **What it does**: Changes the number of arguments in a method call to potentially match a *different* overloaded version. (When adding arguments, default values are used.)
  - **Purpose**: Tests sensitivity to the number of arguments when overloading is involved, ensuring the correct version is called.

### V. Other Class Features

- **TKD (This Keyword Deletion)**
  - **What it does**: Deletes the `this.` qualifier when accessing an instance variable.
  - **Example**: `this.x = 10;` mutated to `x = 10;`.
  - **Purpose**: Tests if there's ambiguity with a local variable of the same name.

- **SMC (Static Modifier Change)**
  - **What it does**: Adds or removes the `static` modifier from methods or variables.
  - **Purpose**: Tests correct handling of class-level vs. instance-level state and behavior.

- **DCD (Default Constructor Deletion)**
  - **What it does**: Deletes the user-defined default (no-argument) constructor.
  - **Purpose**: Tests if other parts of the system rely on this specific constructor.

- **VID (Variable Initialization Deletion)**
  - **What it does**: Removes the initialization of member variables (both instance and static) at their declaration.
  - **Purpose**: Forces the variable to rely on its default value (e.g., 0, null) to test if the explicit initialization was actually necessary or if the default value causes errors.
