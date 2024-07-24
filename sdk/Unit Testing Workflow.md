**Unit testing custom workflow activities**
===

 In this post, I will show how to unit test custom workflow activities that are executed by Dynamics CRM processes. I will be using the regular expression validation custom workflow activity I showed in my "Using regular expressions in Dynamics CRM 2011 processes" post.


## Setting up the projects

First, you need to set up a project for the custom workflow activity. In Visual Studio create a new class library project called DemoCrmWorkflowActivities and add references to the following assemblies:

```
1. System.Activities
2. System.ServiceModel
3. System.Runtime.Serialization
4. System.Text.RegularExpressions
5. Microsoft.Xrm.Sdk (browse to the .dll in the CRM SDK)
6. Microsoft.Xrm.Sdk.Workflow (browse to the .dll in the CRM SDK)
```

Once that's done, you need to add a new unit test project to the solution. Go to the add project menu, and select the test->unit test project type. Following the `<PROJECT-BEING-TESTED>.Test` naming convention, the test project should be called DemoCrmWorkflowActivities.Test. Once that project is created, add references to the following assemblies:

1. `System.Runtime.Serialization`
2. `Moq` (browse to the `Moq.dll` you downloaded for the earlier examples)
3. `Microsoft.Xrm.Sdk` (browse to the .dll in the CRM SDK)
4. `Microsoft.Xrm.Sdk.Workflow` (browse to the .dll in the CRM SDK)

Also, make sure to add a reference to the DemoCrmWorkflowActivities project. Otherwise you won't be able to call the methods you want to test in your test methods.

## The method to test

Because I am using a custom workflow activity that I had discussed in my "Using regular expressions in Dynamics CRM 2011 processes" post, I'm not going to go into the code in a lot of detail here. Basically, the custom workflow activity does the following:

1. Accept a string to validate and a regular expression match pattern via input arguments.

2. Evaluate the regular expression.
3. Return the result of the match via an output argument.

## The test method

In my test project, I have a method to test a valid phone number and another method to test an invalid phone number. Let's take a look at the code that makes up test for the valid phone number:

First, we can't run the custom workflow activity's Execute method directly, so we need to use a WorkflowInvoker object. We can set up the invoker like this:

```cs
//get new validateregex object
ValidateRegex valRegex = new ValidateRegex();
//instantiate the workflowinvoker
var invoker = new WorkflowInvoker(valRegex);
```
Because the custom workflow activity executes inside Dynamics CRM, the WorkflowInvoker object expects several extensions that we need to mock using Moq. They are:

1. CRM tracing service
2. CRM workflow context
3. CRM organization service factory

Here's how we mock each of those:
```cs
//create our mocks
var serviceMock = new Mock<IOrganizationService>();
var factoryMock = new Mock<IOrganizationServiceFactory>();
var tracingServiceMock = new Mock<ITracingService>();
var workflowContextMock = new Mock<IWorkflowContext>();
//set up a mock service to act like the CRM organization service
IOrganizationService service = serviceMock.Object;
//set up a mock workflowcontext
var workflowUserId = Guid.NewGuid();
var workflowCorrelationId = Guid.NewGuid();
var workflowInitiatingUserId = Guid.NewGuid();
workflowContextMock.Setup(t => t.InitiatingUserId).Returns(workflowInitiatingUserId);
workflowContextMock.Setup(t => t.CorrelationId).Returns(workflowCorrelationId);
workflowContextMock.Setup(t => t.UserId).Returns(workflowUserId);
var workflowContext = workflowContextMock.Object;
//set up a mock tracingservice - will write output to console
tracingServiceMock.Setup(t => t.Trace(It.IsAny<string>(), It.IsAny<object[]>())).Callback<string, object[]>((t1, t2) => Console.WriteLine(t1, t2));
var tracingService = tracingServiceMock.Object;
//set up a mock servicefactory
factoryMock.Setup(t => t.CreateOrganizationService(It.IsAny<Guid>())).Returns(service);
var factory = factoryMock.Object;

```

We then add them to the WorkflowInvoker Extensions collection:

```cs
invoker.Extensions.Add<ITracingService>(() => tracingService);
invoker.Extensions.Add<IWorkflowContext>(() => workflowContext);
invoker.Extensions.Add<IOrganizationServiceFactory>(() => factory);
```

Once all the setup work is done, we can pass our input parameters and call the custom activity:
```cs
//set matchpattern to nanp format of xxx-xxx-xxxx
string matchPattern = @"^[2-9]\d{2}-\d{3}-\d{4}$";
//set string to validate to a valid phone number
string stringToValidate = "334-867-5309";
var inputs = new Dictionary<string, object> 
{
	{ "MatchPattern", matchPattern},
	{ "StringToValidate", stringToValidate }
};
var outputs = invoker.Invoke(inputs);
int valid = Convert.ToInt16(outputs["Valid"]);
```

Finally, we use a simple assertion just like in the previous examples to check the output is as we expect:
```cs
//0 = invalid, 1 = valid
Assert.AreEqual(1, valid);
```

---
