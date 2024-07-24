# **Unit testing custom plug-ins**
---
 In this post I will show you how to unit test a Dynamics CRM plug-in. For this example we'll be using the sample FollowupPlugin class in the CRM 2011 SDK, but the principles I will demonstrate are applicable to any plug-in.

## Setting up the projects

First, you need to set up a project for the plug-in. In Visual Studio, create a new class library project called DemoCrmPlugins and add references to the following assemblies:
```
System.ServiceModel
System.Runtime.Serialization
Microsoft.Xrm.Sdk (browse to the .dll in the CRM SDK)
```
Once that's done, you need to add a new unit test project to the solution. Go to the add project menu, and select the test->unit test project type. Following the <PROJECT-BEING-TESTED>.Test naming convention, the test project should be called DemoCrmPlugins.Test. Once that project is created, add references to the following assemblies:

1. System.Runtime.Serialization
2. Moq (browse to the Moq.dll you downloaded for the earlier examples)
3. Microsoft.Xrm.Sdk (browse to the .dll in the CRM SDK)

Also, make sure to add a reference to the DemoCrmPlugins project. Otherwise you won't be able to call the methods you want to test in your test methods.

## The method to test

As I mentioned at the beginning of this post, we'll be using the sample FollowupPlugin class that is included in the CRM SDK. That plug-in is designed to create a follow-up task linked to a newly created account, so our test will need to verify that the task is created with the attributes as specified in the body of the plug-in as well as the parent account passed in at run time.

Here's the complete plug-in Execute method from the SDK:
```cs
public void Execute(IServiceProvider serviceProvider)
{
	//Extract the tracing service for use in debugging sandboxed plug-ins.
	ITracingService tracingService =
		(ITracingService)serviceProvider.GetService(typeof(ITracingService));
	// Obtain the execution context from the service provider.
	IPluginExecutionContext context = (IPluginExecutionContext)
		serviceProvider.GetService(typeof(IPluginExecutionContext));
	// The InputParameters collection contains all the data passed in the message request.
	if (context.InputParameters.Contains("Target") &&
		context.InputParameters["Target"] is Entity)
	{
		// Obtain the target entity from the input parameters.
		Entity entity = (Entity)context.InputParameters["Target"];
 
		// Verify that the target entity represents an account.
		// If not, this plug-in was not registered correctly.
		if (entity.LogicalName != "account")
			return;
		try
		{
			// Create a task activity to follow up with the account customer in 7 days. 
			Entity followup = new Entity("task");
			followup["subject"] = "Send e-mail to the new customer.";
			followup["description"] =
				"Follow up with the customer. Check if there are any new issues that need resolution.";
			followup["scheduledstart"] = DateTime.Now.AddDays(7);
			followup["scheduledend"] = DateTime.Now.AddDays(7);
			followup["category"] = context.PrimaryEntityName;
			// Refer to the account in the task activity.
			if (context.OutputParameters.Contains("id"))
			{
				Guid regardingobjectid = new Guid(context.OutputParameters["id"].ToString());
				string regardingobjectidType = "account";
				followup["regardingobjectid"] =
				new EntityReference(regardingobjectidType, regardingobjectid);
			}
			// Obtain the organization service reference.
			IOrganizationServiceFactory serviceFactory = (IOrganizationServiceFactory)serviceProvider.GetService(typeof(IOrganizationServiceFactory));
			IOrganizationService service = serviceFactory.CreateOrganizationService(context.UserId);
			// Create the task in Microsoft Dynamics CRM.
			tracingService.Trace("FollowupPlugin: Creating the task activity.");
			service.Create(followup);
		}
		catch (FaultException<OrganizationServiceFault> ex)
		{
			throw new InvalidPluginExecutionException("An error occurred in the FollupupPlugin plug-in.", ex);
		}
		catch (Exception ex)
		{
			tracingService.Trace("FollowupPlugin: {0}", ex.ToString());
			throw;
		}
	}
}
```

## The test method

Unlike when testing custom workflow activities, we can execute a CRM plug-in's Execute method directly in a test method, but we still have to pass it the correct input parameters. A plug-in's Execute method expects an IServiceProvider parameter that serves as a container for references to the CRM tracing service, a CRM organization service factory, the CRM notification service and the plug-in execution context. We can mock the IServiceProvider "container" as well as all of the references it contains using basic Moq functionality we've used previously. Once that's done, the Execute method is called, and we verify the expected results using Assert statements.

Here is the complete test method in ACT-ARRANGE-ASSERT format:
```cs
[TestMethod]
public void Execute_Test()
{
	//ARRANGE
	var serviceMock = new Mock<IOrganizationService>();
	var factoryMock = new Mock<IOrganizationServiceFactory>();
	var tracingServiceMock = new Mock<ITracingService>();
	var notificationServiceMock = new Mock<IServiceEndpointNotificationService>();
	var pluginContextMock = new Mock<IPluginExecutionContext>();
	var serviceProviderMock = new Mock<IServiceProvider>();
 
	//create a guid that we want our mock CRM organization service Create method to return when called
	Guid idToReturn = Guid.NewGuid();
 
	//next - create an entity object that will allow us to capture the entity record that is passed to the Create method
	Entity actualEntity = new Entity(); 
 
	//setup the CRM service mock
	serviceMock.Setup(t =>
		t.Create(It.IsAny<Entity>())) 
 
//when Create is called with any entity as an invocation parameter
		.Returns(idToReturn) //return the idToReturn guid
		.Callback<Entity>(s => actualEntity = s); //store the Create method invocation parameter for inspection later
	IOrganizationService service = serviceMock.Object;
 
	//set up a mock servicefactory using the CRM service mock
	factoryMock.Setup(t => t.CreateOrganizationService(It.IsAny<Guid>())).Returns(service);
	var factory = factoryMock.Object;
 
	//set up a mock tracingservice - will write output to console
	tracingServiceMock.Setup(t => t.Trace(It.IsAny<string>(), It.IsAny<object[]>())).Callback<string, object[]>((t1, t2) => Console.WriteLine(t1, t2));
	var tracingService = tracingServiceMock.Object;
 
	//set up mock notificationservice - not going to do anything with this
	var notificationService = notificationServiceMock.Object;
 
	//set up mock plugincontext with input/output parameters, etc.
	Entity targetEntity = new Entity("account");
	targetEntity.LogicalName = "account";
 
	//userid to be used inside the plug-in
	Guid userId = Guid.NewGuid();
 
	//"generated" account id
	Guid accountId = Guid.NewGuid();
 
	//get parameter collections ready
	ParameterCollection inputParameters = new ParameterCollection();
	inputParameters.Add("Target", targetEntity);
	ParameterCollection outputParameters = new ParameterCollection();
	outputParameters.Add("id", accountId);
 
	//finish preparing the plugincontextmock
	pluginContextMock.Setup(t => t.InputParameters).Returns(inputParameters);
	pluginContextMock.Setup(t => t.OutputParameters).Returns(outputParameters);
	pluginContextMock.Setup(t => t.UserId).Returns(userId);
	pluginContextMock.Setup(t => t.PrimaryEntityName).Returns("account");
	var pluginContext = pluginContextMock.Object;
 
	//set up a serviceprovidermock
	serviceProviderMock.Setup(t => t.GetService(It.Is<Type>(i => i == typeof(IServiceEndpointNotificationService)))).Returns(notificationService);
	serviceProviderMock.Setup(t => t.GetService(It.Is<Type>(i => i == typeof(ITracingService)))).Returns(tracingService);
	serviceProviderMock.Setup(t => t.GetService(It.Is<Type>(i => i == typeof(IOrganizationServiceFactory)))).Returns(factory);
	serviceProviderMock.Setup(t => t.GetService(It.Is<Type>(i => i == typeof(IPluginExecutionContext)))).Returns(pluginContext);
	var serviceProvider = serviceProviderMock.Object;
 
	//ACT
	//instantiate the plugin object and execute it with the testserviceprovider
	FollowupPlugin followupPlugin = new FollowupPlugin();
	followupPlugin.Execute(serviceProvider);
 
	//ASSERT
	//verify the entity created inside the plugin the values we expect
	Assert.AreEqual("Send e-mail to the new customer.", actualEntity["subject"]);
	Assert.AreEqual("Follow up with the customer. Check if there are any new issues that need resolution.", actualEntity["description"]);
	Assert.AreEqual(DateTime.Now.AddDays(7).ToLongDateString(), ((DateTime)actualEntity["scheduledstart"]).ToLongDateString()); //lazy way to get around milliseconds being different
	Assert.AreEqual(DateTime.Now.AddDays(7).ToLongDateString(), ((DateTime)actualEntity["scheduledend"]).ToLongDateString()); //lazy way to get around milliseconds being different
	Assert.AreEqual("account", actualEntity["category"]);
	Assert.AreEqual(accountId, ((EntityReference)actualEntity["regardingobjectid"]).Id);
}
```
As you can see, the code is relatively lengthy for what is being tested, but it's not actually that complex. In my next post I'll show how to mock HTTP requests for testing RESTful interface calls from Dynamics CRM custom code.
