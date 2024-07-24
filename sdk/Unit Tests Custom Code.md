# **Unit testing custom Microsoft Dynamics CRM code**
---


1. Create a project in Visual Studio to hold methods to test
2. Create a project in Visual Studio to handle the testing
3. Create a method to test
4. Create a test
5. Run the test and view the results

In a test-driven development environment, you would actually invert steps 3 and 4 so that you first write a test, and then you write code to satisfy the test. But for the purposes of this overview, I think it's better to write the method we want to test first.

## Setting up the projects

After you've downloaded Moq, open Visual Studio and create a new class library project called DemoCrm and add references to the following assemblies:

- `System.Runtime.Serialization` (.Net)
- `Microsoft.Xrm.Sdk` (browse to the .dll in the CRM SDK)

Once that's done, you need to add a new unit test project to the solution. Go to the add project menu, and select the test->unit test project type. You can call it whatever you like, but I generally prefer to use `<PROJECT-BEING-TESTED>.Test`, so mine is called DemoCRM.Test. Once that project is created, add references to the following assemblies:

- `System.Runtime.Serialization` (.Net)
- `Moq` (browse to the `Moq.dll` you downloaded earlier)
- `Microsoft.Xrm.Sdk` (browse to the .dll in the CRM SDK)

Also, make sure to add a reference to the DemoCrm project. Otherwise you won't be able to call the methods you want to test in your test methods.

## The method to test

In this example, we will have a method called CreateCrmAccount that creates a new account record in Dynamics CRM. This method does the following:

1. Accepts the name of the account to create and a reference to an instantiated CRM organization service object as invocation parameters
2. Creates the account record in Dynamics CRM
3. Returns the id of the newly created record

Here's the code:
```csharp
/// <summary>
/// Creates a new account with a given name using the supplied organization service
/// </summary>
/// <param name="accountName">account name</param>
/// <param name="service">organization service</param>
/// <returns>id of the new account</returns>
public static Guid CreateCrmAccount(string accountName, IOrganizationService service) {
    Entity account = new Entity("account");
    account["name"] = accountName;
    Guid newId = service.Create(account);
    return newId;
}
```

## The test method

Here's a sample test method. I'll explain how it behaves after I show the code.

```csharp
/// <summary>
/// Tests the CreateCrmAccount method
/// </summary>
[TestMethod]
public void CreateCrmAccount_Test() {
        //ARRANGE - set up everything our test needs
        //first - set up a mock service to act like the CRM organization service
        var serviceMock = new Mock <IOrganizationService> ();
        IOrganizationService service = serviceMock.Object;

        //next - set a name for our fake account record to create
        string accountName = "Lucas Demo Company";

        //next - create a guid that we want our mock service Create method to return when called
        Guid idToReturn = Guid.NewGuid();

        //next - create an entity object that will allow us to capture the entity record that is passed to the Create method
        Entity actualEntity = new Entity();

        //finally - tell our mock service what to do when the Create method is called
        serviceMock.Setup(t =>
                t.Create(It.IsAny < Entity > ())) //when Create is called with any entity as an invocation parameter
            .Returns(idToReturn) //return the idToReturn guid
            .Callback < Entity > (s => actualEntity = s); 
            //store the Create method invocation parameter for inspection later
        //ACT - do the thing(s) we want to test
        //call the CreateCrmAccount method like usual, but supply the mock service as an invocation parameter
        Guid actualGuid = MockDemo.CreateCrmAccount(accountName, service);

        //ASSERT - verify the results are correct
        //verify the entity created inside the CreateCrmAccount method has the name we supplied
        Assert.AreEqual(accountName, actualEntity["name"]);

        //verify the guid returned by the CreateCrmAccount is the same guid the Create method returns
        Assert.AreEqual(idToReturn, actualGuid);
}
```

This method uses the ARRANGE-ACT-ASSERT pattern in which we first set up everything we need for the test (arrange), then run the code to be tested (act) and finally validate the behavior of the code (assert).

## Arrange

In the arrange stage we:

1. Create a mock object as a stand-in for the actual CRM organization service.
2. Set up some values for the account name and the Id we want the mock service to return when the Create method is called.
3. Tell the mock service what value to return when the Create method is called and how to capture a copy of the entity that is passed to the Create method.

Because the Setup call uses "It.IsAny<Entity>()" our mock CRM service Create method would handle all Create calls the same. You can set up different behaviors for different situations if you wanted to test the creation of multiple entities. It is also useful to be able to return different responses for different queries against the mock CRM service.

## Act

In the act stage, we execute the `CreateCrmAccount` method.

## Assert

In the assert stage, we first compare the values of the accountName we passed into the CreateCrmAccount method with the `name` attribute value of the entity it passed to the CRM service Create method. If they are the same, the first Assert.AreEqual call will pass. If not, it will fail.

Next we compare the values of the Id that we told our mock service Create method to return with the value the CreateCrmAccount method returns. As before, if they match, this passes. Otherwise it fails. If either of our test assertions fail, the entire test will fail. If both assertions pass, the entire test passes.

The code samples for this example can be downloaded here, and the testing code can be downloaded here.

## Running the test

Once you've created your method to test and your method to run the test, you can execute the test directly in Visual Studio. First, build your solution, then go to the main menu and select **Test->Windows->Test Explorer**. Next, right-click the name of the test you want to execute and select **run selected tests.** If all went according you plan, you should see success icons.
