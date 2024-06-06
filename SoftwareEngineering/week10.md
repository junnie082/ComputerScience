# Week 10 Software Testing

#### 2024.06.06.(목)

### Topics covered

- Program Testing

- Development testing

- Test-driven development

- Release testing

- User testing

## Program testing

- Testing is intedned to show that a program does what it is intended to do and to discover program defects before it is put into use.

- When you test software, you execute a program using artificial data.

- You check the results of the test run for errors, anomalies or information about the program's non-functional attributes.

- Can reveal the presence of errors, NOT their absence.

### Program testing goals

- To demonstrate to the developer and the customer that the software meets its requirements.

  - For custom software, this means that there should be at least one test for every requirement in the requirements document.
  - For generic software products, it means that there should be tests for all of the system features, plus combinations of these features, that will be incorporated in the product release.

- To discover situations in which the behavior of the software is incorrect, undesirable of does not conform to its specification.

- Validation testing

  - To demonstrate to the developer and the system customer that the software meets its requirements.
  - A successful test shows that the system operates as intended.

- Defect testing
  - To discover faults or defects in the software where its behaviour is incorrect or not in conformance with its specification
  - A successful test is a test that makes the system perform incorrectly and so exposes a defect in the system.

### Verification vs Validation

- Verification

  - "Are we building the product right".
  - The software should conform to its specification.

- Validation
  - "Are we building the right product".
  - The software should do what the user really requires.

### Inspections and testing

- Software inspections: Concerned with analysis of the static system representation to discover problems (static verification)

* May be supplemented by tool-based document and code analysis.

- Software testing: Concerned with exercising and observing product behaviour (dynamic verification)

* The system is executed with test data and its operational behaviour is observed.

- Inspections and testing are complementary and not opposing verification techniques.

- Both should be used during the V & V process.

- Inspections can check conformance with specification but not conformance with the customer's real requirements.

- Inspections cannot check non-functional characteristics such as performance, usability, etc.

### Stages of testing

- Development testing, where the system is tested during development to discover bugs and defects.

- Release testing, where a separate testing team test a complete version of the system before it is released to users.

- User testing, where users or potential users of a system test the system in their own environment.

## Development testing

- Development testing includes all testing activities that are carried out by the team developing the system.

- Testing activities can be categorized based on the level of components being tested.

- Unit testing, where individual program units or object classes are tested.

  - Unit testing should focus on testing the functionality of objects or methods.

- Component testing, where several individual units are integrated to create composite components.

  - Component testing should focus on testing component intefaces.

- System testing, where some or all of the components in a system are integrated and the system is tested as a whole.
  - System testing should focus on testing component interactions.

### Unit testing

- Unit testing is the process of testing individual components in isolation.

- It is a defect testing process.

- Units may be:
  - Individual functions or methods within an object
  - Object classes with several attributes and methods
  - Composite components with defined interfaces used to access their functionality.

### Object class testing

- Complete test coverage of a class involves

* Testing all operations associated with an object
* Setting and interrogating all object attributes
* Exercising the object in all possible states.

- Inheritance makes it more difficult to design object class tests as the information to be tested is not localised.

### Automated testing

- Whenever possible, unit testing should be automated so that tests are run and checked without manual intervention.

- In automated unit testing, you make use of a test automation framework (such as JUnit) to write and run your program tests.

- Unit testing frameworks provide generic test classes that you extend to create specific test cases.

* They can then run all of the tests that you have implemented and report, often through some GUI, on the success of otherwise of the tests.

### Automated test components

- A `setup` part, where you initialize the system with the test case, namely the inputs and expected outputs.

- A `call` part, where you call the object or method to be tested.

- An `assertion` part where you compare the result of the call with the expected result.

  - If the assertion evaluates to true, the test has been successful / if false, then it has failed.

- A `tear-down` part, which restores system states back to the beginning, such as releasing resources or clean up temporary files.

- The first two parts have more attention in automated testing research.

### Types of Unit Test Cases

- The test cases should show that, when used as expected, the component that you are testing does what it is supposed to do.

- If there are defects in the component, these should be revealed by test cases.

- This leads to two types of unit test case:
  - The first of these should reflect normal operation of a program and should show that the component works as expected.
  - The other kind of test case should be based on testing experience of where common problems arise.
    - It should use abnormal inputs to check that these are properly processed and do not crash the component.

### Testing strategies

- `Partition testing`, where you identify groups of inputs that have common characteristics and should be processed in the same way.

  - You should choose tests from within each of these groups.

- `Guideline-based testing`, where you use testing guidelines to choose test cases.

* These guidelines reflect previous experience of the kinds of errors that programmers often make when developing components.

### Partition testing

- Input data and output results often fall into different classes where all members of a class are related.

- Each of these classes is an equivalence partition or domain where the program behaves in an equivalent way for each class member.

- Test cases should be chosen from each partition.

### Testing guidelines (collections)

- Test software with collections which have only a single value.

```Java
List list = new ArrayList();
list.add(0);
```

- Use collections of different sizes in different tests.

- Derive tests so that the first, middle and last elements of the collection are accessed.

- Test with collections of zero length.

### General testing guidelines

- Choose inputs that force the system to generate all error messages.

  - e.g.) Java try/catch. Consider all catch blocks.

- Design inputs that cause input buffers to overflow.

- Repeat the same input or series of inputs numerous times.

- Force invalid outputs to be generated.

- Force computation results to be too large or too small.

### Component testing

- Software components are often composite components that are made up of several interacting objects.

  - For example, a software may have separate parts to obtain or read user data and analyze the data, so that data obtained from various source can be analyzed by the same technique.

- You access the functionality of these objects through the defined component interface.

- Testing composite components should therefore focus on showing that the component interface behaves according to its specification.
  - You can assume that unit tests on the individual objects within the component have been completed.

### Interface testing

- Objectives are to detect faults due to interface errors or invalid assumptions about interfaces.

- Interface types
  - `Parameter interfaces`: Data passed from one method or procedure to another.
  - `Shared memory interfaces`: Block of memory is shared between procedures or functions.
  - `Procedural interfaces`: Sub-system encapsulates a set of procedures to be called by other sub-systems.
  - `Message passing interfaces`: Sub-systems request services from other sub-systems.

### Interface errors

- `Interface misuse`

  - A calling component calls another component and makes an error in its use of its interface
  - e.g.) parameters in the wrong order.

- `Interface misunderstanding`

  - A calling component embeds assumptions about the behaviour of the called component which are incorrect.
  - e.g.) returning an empty or default object instead of null.

- `Timing errors`
  - The called and the calling component operate at different speeds and out-of-date information is accessed.

### Interface testing guidelines

- Design tests so that parameters to a called procedure are at the extreme ends of their ranges.

* corner cases or boundary cases.

- Always test pointer parameters with null pointers.

- Design tests which cause the component to fail.

- Use stress testing in message passing systems.

- In shared memory systems, vary the order in which components are activated.

### System testing

- System testing during development involves integrating components to create a version of the system and then testing the integrated system.

- The focus in system testing is testing the interactions between components.

- System testing checks that components are compatible, interact correctly and transfer the right data at the right time across their interfaces.

- System testing tests the emergent behaviour of a system.

### System and component testing

- During system testing, reusable components that have been separately developed and off-the-shelf systems may be integrated with newly developed components

* The complete system is then tested.

- Components developed by different team members or sub-teams may be integrated at this stage.

* System testing is a collective rather than an individual process.
* In some companies, system testing may involve a separate testing team with no involvement from designers and programmers.

### Use-case testing

- The use-cases developed to identify system interactions can be used as a basis for system testing.

- Each use case usually involves several system components so testing the use case forces these interactions to occur.

- The sequence diagrams associated with the use case documents can describe the components and interactions that are being tested.

### Testing policies

- Exhaustive system testing is impossible so testing policies which define the required system test coverage may be developed.

- Examples of testing policies:
  - All system functions that are accessed through menus should be tested.
  - Combinations of functions (e.g. text formatting) that are accessed through the same menu must be tested.
  - Where user input is provided, all functions must be tested with both correct and incorrect inputs.

## Test-driven development

- `Test-driven development (TDD)` is an approach to program development in which you inter-leave testing and code development.

- Tests are written before code and 'passing' the tests is the critical driver of development.

- You develop code incrementally, along with a test for that increment.

  - You don't move on to the next increment until the code that you have developed passes its test.

- TDD was introduced as part of agile methods such as Extreme Programming.
  - However, it can also be used in plan-driven development processes.

### Test-driven development

Identify new functionality -> Write test -> Run test -> fail -> Implement functionality and refactor
-> Run test

### Benefits of test-driven development

- `Code coverage`

  - Every code segment that you write has at least one associated test so all code written has at least one test.

- `Regression testing`

  - A regression test suite is developed incrementally as a program is developed.

- `Simplified debugging`

  - When a test fails, it should be obvious where the problem lies. The newly written code needs to be checked and modified.

- `System documentation`
  - The tests themselves are a form of documentation that describe what the code should be doing.

### Regression testing

- Regression testing is testing the system to check that changes have not 'broken' previously working code.

- In a manual testing process, regression testing is expensive but, with automated testing, it is simple and straightforward.

  - All tests are rerun every time a change is made to the program.

- Tests must run 'successfully' before the change is committed.

- TDD is a good practice to secure regression tests.

### Code Coverage

- Statement Coverage (or Line Coverage)

  - Satisfied if a statement is executed.

- Branch Coverage (or Decision Coverage)

  - Satisfied if each branch is executed.

- Condition Coverage
  - Satisfied if each condition in a branch is tested.

### Increasing Code Coverage

- You can increase code coverage by designing setup and call parts of test cases carefully.

- In Java, private methods must be called by public methods.

  - If your tests call all public methods, and take all the branches in the methods,
  - then theoretically you can acieve 100% line/branch coverage.

- For this purpose, it is important to create appropriate object instances, which can be used to call methods.
  - Good modularization makes it easier to write test cases for each unit of a program.

### Limitation of Code Coverage

- Code coverage is used to assess your test suite's quality or performance.

  - How much portion of the code has been tested?

- Automated testing research often uses branch coverage.

  - Randoop / EvoSuite for JUnit test case generation.

- Recent studies showed that code coverage is not enough for test suite evaluation.

  - Does it really detect faults?

- Code coverage is used to assess your test suite's quality or performance.

  - How much portion of the code has been tested?

- Automated testing research often uses branch coverage.

  - Randoop / EvoSuite for JUnit test case generation.

- Recent studies showed that code coverage is not enough for test suite evaluation.

  - Does it really detect faults?

- **Mutating testing**: automatically mutate your code to plan faults, and see if your tests can detect the faults.

### TDD Example

`Java and JUnit`

- Class Under Test

  - SimpleMath

- Test Class

  - SimpleMathTest

- Implementing a simple math library.
  - a method computing Fibonacci numbers.

```Java
package math.lib;

public class SimpleMath {
    public static int fibonacci(int n) {
        return -1;
    }
}
```

```Java
package math.lib;

import static org.junit.jupiter.api.Assertions.fail;

class SimpleMathTest {

    @Test
    void testFibonacci() {
        fail("Not yet implemented");
    }
}
```

`Java and JUnit`

- Writing a unit test for the method.

  - Test various inputs for Fibonacci numbers.

- At the first run, it should fail.

- The goal is writing the method body which can pass the test.

```Java
@Test
void testFibonacci() {
    //1, 1, 2, 3, 5, 8, 13, ...
    assertEquals(5, SimpleMath.fibonacci(5));
    assertEquals(3, SimpleMath.fibonacci(4));
    assertEquals(1, SimpleMath.fibonacci(1));
}
```

```Java
package math.lib;

// fail
public class SimpleMath {
    public static int fibonacci(int n) {
        return -1;
    }
}
```

```Java
// pass
public static int fibonacci(int n) {
    return n == 1 || n == 2 ?
            1 : fibonacci(n-2) + fibonacci(n-1);
}
```

- We can add more requirements during development.

- Then we first need to modify the test for the requirement.

- Suppose we return 0 for any input <= 0.

- With the current method, the new test fails.

```Java
// fail
public static int fibonacci(int n) {
    return n == 1 || n == 2 ?
            1 : fibonacci(n-2) + fibonacci(n-1);
}
```

- Modify the code to pass the new test.

- With the additional if statement, it now passes the test.

- How about checking performance?
  - We also want to make the method efficient.

```Java
public static int fibonacci(int n) {
    if (n <= 0)
        return 0;
    else
        return n <= 2 ?
            1 : fibonacci(n-2) + fibonacci(n-1);
}
```

- We can write a new test.

- Note that the previous test still exists for verification.

- Test results show that the previous test passed, but the new test for performance checking failed.
  - We need to cut off about 21 seconds.

```Java
@Test
void testFibonacciTimeout() {
    assertTimeout(ofMillis(1), () -> {
        SimpleMath.fibonacci(30);
    });
}

public static int fibonacci(int n) {
    if (n <= 0)
        return 0;
    else
        return n <= 2 ?
            1 : fibonacci(n-2) + fibonacci(n-1);
}
```

- Modify the method.

- We introduce a new method to keep the interface same.

- Now it passes all the tests.

- In TDD, we repeat these steps continuously for software development.
  - Then we will have our software as well as a test suite for verification.

```Java
@Test
void testFibonacciTimeout() {
    assertTimeout(ofMillis(1), () -> {
        SimpleMath.fibonacci(30);
    });
}

public static int fibonacci(int n) {
    return fibonacci(n, 1, 1);
}

public static int fibonacci(int n, int pp, int p) {
    if (n <= 0)
        return 0;
    else
        return n <= 2 ?
                p : fibonacci(n-1, p, pp + p);
}
```

## Release testing

- Release testing is the process of testing a particular release of a system that is intended for use outside of the development team.

- The primary goal of the release testing process is to convince the supplier of the system that it is good enough for use.

  - Release testing, therefore, has to show that the system delivers its specified functionality, performance and dependability, and that it does not fail during normal use.

- Release testing is usually a black-box testing process where tests are only derived from the system specification.
  - vs. white-box testing

### Release testing and system testing

- Release testing is a form of system testing.

- Important differences:
  - A separate team that has not been involved in the system development, should be responsible for release testing.
  - System testing by the development team should focus on discovering bugs in the system (defect testing).
  - The objective of release testing is to check that the system meets its requirements and is good enough for external use (validation testing).

### Requirements based testing

- Requirements-based testing involves examining each requirement and developing a test of tests for it.

- We've already experienced requirements based testing, by verifying each requirement in the list for an intermediate release.

- A developed test is not necessarily an automated test.
  - You can simply define a scenario to use the system and verify the requirements are satisfied.

### Performance testing

- Part of release testing may involve testing the emergent properties of a system, such as performance and reliability.

- Tests should reflect the profile of use of the system.

- Performance tests usually involve planning a series of tests where the load is steadily increased until the system performance becomes unacceptable.

- Stress testing is a form of performance testing where the system is deliberately overloaded to test its failure behaviour.

## User testing

- User or customer testing is a stage in the testing process in which users or customers provide input and advice on system testing.

- User testing is essential, even when comprehensive system and release testing have been carried out.

  - The reason for this is that influences from the user's working environment have a major effect on the reliability, performance, usability and robustness of a system.

  - These cannot be replicated in a testing environment.

### Types of user testing

- `Alpha testing`

* Users of the software work with the development team to test the software at the developer's site.

- `Beta testing`

* A release of the softwrae is made available to users to allow them to experiment and to raise problems that they discover with the system developers.

- `Acceptance testing`

* Customers test a system to decide whether or not it is ready to be accepted from the system developers and deployed in the customer environment. Primarily for custom systems.

### User Testing for Different Types of Software

- Although a software has been developed enough for in user testing, it is still in its testing stage.

- Undiscovered errors might happen during this period.

- Hence it is necessary to design and conduct user testing differently for different types of software.
  - For instance, beta testing of online games can be more easily conducted, compare to autonomous driving software or financial software.

### Agile methods and acceptance testing

- In agile methods, the user/customer is part of the development team and is responsible for making decisions on the acceptability of the system.

- Tests are defined by the user/customer and are integrated with other tests in that they are run automatically when changes are made.

- There is no separate acceptance testing process.

- Main problem here is whether or not the embedded user is 'typical' and can represent the interests of all system stakeholders.
