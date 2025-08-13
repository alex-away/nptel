## motivation for software testing

- software is ubiquitous, from consumer apps to safety-critical systems like aviation control and medical devices. we expect it to be reliable and error-free.
- inadequate testing can lead to catastrophic and expensive failures.
    - **ariane 5 rocket**: exploded 36 seconds after launch due to a data conversion error (64-bit float to 16-bit integer).
    - **therac-25**: a radiation therapy machine delivered fatal radiation overdoses due to a software **race condition**.
    - **intel pentium bug**: a flaw in floating-point division forced intel to recall millions of chips, costing ~$475 million.
- the cost of fixing a bug increases exponentially the later it is found in the development lifecycle. a bug found after deployment can be over 40 times more expensive to fix than one found during the requirements phase.

***

## core terminology

- **fault, error, and failure**: this is the life cycle of a bug.
    - **fault (or bug)**: a static defect in the code (e.g., a wrong algorithm).
    - **error**: an incorrect internal state that occurs when the faulty code is executed.
    - **failure**: the observable, incorrect output or behavior of the software as a result of the error.
- **verification vs. validation**:
    - **verification**: asks "are we building the product right?". it's the process of checking that software artifacts (design, code) meet their specified requirements during development.
    - **validation**: asks "are we building the right product?". it's the process of evaluating the final software to ensure it meets the customer's actual needs.
- **test case**: the fundamental unit of a test. it must contain two things:
    - a set of **inputs** to the software.
    - the **expected output** for those inputs.

***

## levels and types of testing

- **testing levels**: testing is done at different stages, from small parts to the whole system.
    - **unit testing**: testing individual components or methods in isolation, typically done by developers.
    - **integration testing**: testing the interfaces and interactions between combined modules. for example, testing the call from method `m1` to `m2`, and from `m2` to `m3`.
    - **system testing**: testing the entire, integrated system to evaluate its end-to-end functionality.
    - **acceptance testing**: final testing conducted by the end-user or client to ensure the system meets their business requirements.
- **other important test types**:
    - **functional testing**: ensures the software meets its specified functional requirements.
    - **performance testing**: measures the speed, responsiveness, and stability of the software.
    - **stress testing**: evaluates how the system behaves under extreme loads, beyond its normal operating capacity.
    - **usability testing**: evaluates the user interface (ui) and user experience (ux) to ensure they are accessible, easy to use, and conform to the **design specifications** in the requirements.
    - **regression testing**: performed after making any change to the software (like a bug fix or new feature) to ensure that the change has not broken any existing functionality.

***

## testing methods and strategy

- **black-box testing**: testing based on requirements without any knowledge of the internal code or structure. focuses on inputs and outputs.
- **white-box testing**: testing based on the internal structure of the software. test cases are designed to cover specific paths, branches, or statements in the code. **unit testing** and **integration testing** are typically white-box.
- **the ripr model**: for a fault to cause a failure, four conditions must be met.
    - **reachability**: the test must execute the location of the fault.
    - **infection**: the fault must cause an incorrect internal state (an error).
    - **propagation**: the error must spread and affect the final output.
    - **reveal**: the tester must be able to observe the incorrect output.
- **testability**: how easy it is to test a piece of software.
    - **controllability**: how easily we can provide inputs to "control" the software and reach a desired state.
    - **observability**: how easily we can see the outputs and internal states to check for correctness.
- **testing maturity levels**: a scale for how an organization views testing.
    - **level 0**: no difference between testing and debugging.
    - **level 1**: testing is done to show the software works (prove correctness), which is impossible.
    - **level 2**: testing's purpose is to find failures. this can create an adversarial developer-tester relationship.
    - **level 3**: testing is done to **identify failures and reduce risk**, with testers and developers collaborating.
    - **level 4**: testing is a mental discipline focused on quality improvement.

***

## criteria-based test design

- **model-based testing**: creating a simplified model of the software (like a graph or logical formula) and deriving tests from it.
- **coverage criterion (c)**: a rule or set of rules that impose requirements on a test set.
- **test requirement (tr)**: a specific element to be satisfied by a test (e.g., "execute line 55").
- **satisfying criteria**: a set of test cases `t` satisfies test requirements `tr` if **for every test requirement `tr` in `tr`, there is at least one test case `t` in `t` that satisfies `tr`**.
- **infeasible test requirements**: a test requirement that is impossible to satisfy (e.g., a line of code inside an `if (x > 0 and x < 0)` block).
- **criteria subsumption**: a way to compare the strength of criteria.
    - criterion `c1` **subsumes** criterion `c2` if and only if **every** set of test cases that satisfies `c1` also satisfies `c2`.
    - this means `c1` is more thorough or stringent than `c2`.
    - the statement "c1 subsumes c2 if there is at least one test case that satisfies c1 which also satisfies c2" is **false**.

***

## test automation with junit

- **automation**: the process of creating executable scripts to run test cases.
- **prefix values**: inputs/actions needed to set up the state *before* a test runs.
- **postfix values**: inputs/actions needed to observe the result or clean up *after* a test runs.
- **junit**: a popular open-source java framework for writing and running automated tests.
- **assertions**: methods that check for a boolean condition and cause a test to fail if the condition is false.
    - example: `assertfalse(val1 > val2)` checks if the statement `val1 > val2` is false. if `val1` is less than `val2`, the statement is false, so the assertion returns **true** (the test passes).
- **test fixtures**: code to manage the test environment.
    - **@before**: a method that runs before each test (for prefix values).
    - **@after**: a method that runs after each test (for postfix values).
- **data-driven testing**: running the same test logic with multiple different sets of input data automatically.