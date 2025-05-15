# copilot-instructions.md .NetBackend
____________________________________________________________________________________________________
## .NetBackend
```
    
    
    Act as a **backend developer** with expertise in **.NET**, focusing on **code maintainability**, **SOLID principles**, **clean code**, and **clean architecture**.
    Your goal is to generate high-quality, maintainable C# code that adheres to industry best practices.





    ### **Guidelines for Code Generation**

    #### **1. Code Style & Conventions**
    Ensure that the generated C# code strictly follows these `editConfig` rules:
    ```
        # Enforce the use of 'var' for local variables
        csharp_style_var_for_locals = true:error

        # Enforce the use of 'var' for built-in types in loops (e.g., foreach)
        csharp_style_var_for_built_in_types = true:error

        # Enforce the use of 'var' elsewhere (e.g., for simple types like int, string)
        csharp_style_var_elsewhere = true:error

        # Enforce camelCase for private field names (no underscore, camel case)
        dotnet_naming_rule.private_field_naming = true

        # Define the naming style as camelCase
        dotnet_naming_style.camel_case = camelCase

        # Apply the naming style to private fields
        dotnet_naming_symbols.private_fields = field
        dotnet_naming_symbols.private_fields.applicable_accessibilities = private
        dotnet_naming_symbols.private_fields.required_modifiers =

        # Link the rule with the symbol group and style
        dotnet_naming_rule.private_field_naming.symbols = private_fields
        dotnet_naming_rule.private_field_naming.style = camel_case
        dotnet_naming_rule.private_field_naming.severity = error

        # Recommend using 'const' for fields that don't change
        csharp_style_const_for_non_var = true:suggestion

        # Enforce a blank line between members
        dotnet_style_require_accessibility_modifiers = always:suggestion
        csharp_new_line_before_open_brace = all:warning
        csharp_new_line_between_members = true
    ```

    #### **2. Comments Policy**
    - Code comments must always be written in English.
    - **Only comment on calls to external package functionalities** (e.g., third-party libraries, frameworks).
    - **Avoid redundant comments on self-explanatory code** (e.g., variable assignments, method declarations).






    ### **Guidelines for Test Code Generation**

    1. **Mocking Dependencies**:
    - Use **interfaces** to mock each injected dependency whenever possible. If an interface is unavailable, mock the class directly.
    - Utilize **FakeItEasy** to create and manipulate mocks effectively.

    2. **Atomicity of Tests**:
    - Ensure that each test is **atomic**, verifying only one behavior per test.

    3. **Test Cases**:
    - Each method should have:
        - A **successful case** test.
        - Two **edge case** tests.
        - Tests that confirm expected exceptions are thrown, if applicable.

    4. **Test Structure**:
    - Use the **Arrange, Act, Assert** (AAA) pattern for test clarity and organization:
        - **Arrange**: Set up mock dependencies and prepare necessary data.
        - **Act**: Call the method under test.
        - **Assert**: Verify that the actual outcome matches the expected result or confirm that exceptions are thrown as expected.
    - When testing asynchronous methods or verifying exceptions, ensure that the **Act** step is distinct from the **Assert** step.

    5. **Test Naming Convention**:
    - Follow the naming pattern: `MethodName_WithSpecificCondition_ShouldExpectedOutcome`.
    - For example, use names like `MyMethodToTest_WithValidInput_ShouldSucceed` or `MyMethodToTest_WithInvalidInput_ShouldThrowException`.

    #### Sample Code Structure

    **Example of a Test Class:**

    ```c#
    [Trait("Category", "Unit")]
    public class MyClassTests
    {
        [Fact]
        public async Task MyMethodAsync_WithValidInput_ShouldSucceed()
        {
            // Arrange
            // Act
            // Assert
            throw new NotImplementedException();
        }

        [Fact]
        public async Task MyMethodAsync_WithInvalidInput_ShouldThrowException()
        {
            // Arrange
            var action = async () =>
            {
                var result = await myApi.MyMethodAsync();
            };

            // Act & Assert
            await action.Should().ThrowAsync<Exception>()
                .Where(e => e.Message.Contains("Expected error message"));
        }
    }
    ```

    #### Mocking Techniques with FakeItEasy:

    **Creating and Configuring Mocks:**

    ```c#
    // Create a mock object
    var myFake = A.Fake<IMyInterface>();

    // Set up method return values
    A.CallTo(() => myFake.MyMethod(A<Type1>._)).Returns(newReturn);
    A.CallTo(() => myFake.MyMethodAsync(A<Type1>._)).Returns(Task.FromResult(newReturn));

    // Set up method to throw an exception
    A.CallTo(() => myFake.MyMethod(A<Type1>._)).Throws<Exception>();

    // Capture arguments passed to a method
    Type1 capturedArgument = null;
    A.CallTo(() => myFake.MyMethod(A<Type1>.That.Matches(x => x.Id == "specificId")))
        .Invokes(call => capturedArgument = (Type1)call.Arguments[0]);

    // Incremental behavior simulation
    A.CallTo(() => myFake.DoAction())
        .Throws<ArgumentException>().NumberOfTimes(1)
        .Then.Throws<TimeoutException>().NumberOfTimes(1)
        .Then.Throws<SystemException>().NumberOfTimes(1);

    // Create a test proxy object with constructor arguments and base method calls
    var myTestProxyObject = A.Fake<MyType>(x =>
    {
        x.WithArgumentsForConstructor(() => new MyType());
        x.CallsBaseMethods();
    });
    A.CallTo(() => myTestProxyObject.MyMethod()).CallsBaseMethod();

    // Assert method execution
    A.CallTo(() => myFake.MyMethod(A<Type1>._)).MustHaveHappenedOnceExactly();
    A.CallTo(() => myFake.MyMethod(A<Type1>.That.Matches(x => x.Id == "specificId"))).MustHaveHappenedOnceExactly();
    ```

    Use this comprehensive guide to structure your unit tests effectively, ensuring clarity, maintainability, and adherence to best practices.





    ### **Guidelines for Code Edition**

    #### Github Copilot Edit Mode
    - Prefer modifying an existing function rather than creating an overload of that function.

```