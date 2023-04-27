# Dynamic CRM Workflow

Workflows automate business processes without a user interface. People usually use workflow processes to initiate automation that doesn't require any user interaction.

Each workflow process is associated with a single entity. When configuring workflows you have four major areas to consider:
- When to start them?
- Should they run as a real-time workflow or a background workflow?
- What actions should they perform?
- Under what conditions should actions be performed?

> You can extend the options available within the workflow designer used in Microsoft Dataverse. These extensions are added by adding an assembly that contains a class that extends the CodeActivity class. These extensions are commonly called workflow assemblies or workflow activities.



A great example of when to use a workflow extension is when a business process requires the ability to query for an entity’s child records. With a workflow extension, you can program the retrieval of a parent’s related children records to either ensure that all records get an updated value, aggregate totals, or applying validation rules.

Another reason to use a workflow extension is maintenance. If a business process is identified as having complex steps or multiple varying conditions, it will be easier to implement this business logic in code. If there are changes in how this business process occurs, changes to code will be easier to apply than changes to a workflow using the Workflow Designer.

Another advantage of using a workflow extension is that, like workflows, they allow the ability to be restarted if an error occurs during execution. Every time a workflow extension executes, CRM creates an entry that allows you to inspect its execution result. You have the ability to restart the workflow without affecting the user or having the user update any information to a particular record that the workflow extension ran against.

```cs
using System.Activities;
using Microsoft.Xrm.Sdk;
using Microsoft.Xrm.Sdk.Workflow;

public class IncrementByTen : CodeActivity
{
    [RequiredArgument]
    [Input("Decimal input")]
    public InArgument<decimal> DecInput { get; set; }

    [Output("Decimal output")]
    public OutArgument<decimal> DecOutput { get; set; }

    protected override void Execute(CodeActivityContext context)
    {
        decimal input = DecInput.Get(context);
        DecOutput.Set(context, input + 10);
    }
}


```


## Create a Workflow Extension
The easiest way to create a workflow extension is to use Visual Studio.  The requirements for creating a workflow extension are as follows:

- Create a Class Library Project.
- Extend your class from CodeActivity located in the `System.Activites` namespace.
- Include a reference to `Microsoft.Xrm.Workflow` and `Microsoft.Xrm.Sdk` assemblies located in the CRM SDK.
- Sign the assembly by creating a SNK file.

The following code block is a basic shell that can be used to quickly create a workflow extension.  The entry point that CRM will utilize is the override void Execute method.  When a workflow is executed in CRM and the workflow extension is subsequently executed, CRM will pass in the information into the Execute method which allows for the TracingService, WorkflowContext, and OrganizationService to be utilized.

```csharp
using System.Activities;
using Microsoft.Xrm.Sdk;
using Microsoft.Xrm.Sdk.Workflow;

namespace CustomWF
{
	public class TestWorkflow : CodeActivity
	{
		protected override void Execute(CodeActivityContext ActivityContext)
        {
			//Tracing service
            ITracingService tracing = context.GetExtension<ITracingService>();
			
			//Get the context
			IWorkflowContext WorkflowContext = ActivityContext.GetExtension<IWorkflowContext>();
			IOrganizationServiceFactory serviceFactory = ActivityContext.GetExtension<IOrganizationServiceFactory>();
			IOrganizationService service = serviceFactory.CreateOrganizationService(WorkflowContext.UserId);
			Entity targetEntityImage = (Entity)WorkflowContext.InputParameters["Target"];
			var input = Test.Get<string>(ActivityContext);
		}
		
		[Input("Test")]
		public InArgument<string> Test { get;set; }
	}	
}

```

- The **TracingService** provides a way to log steps as the workflow extension is being executed.  This helps in identifying potential issues as code is being executed.  If an exception occurs, the information logged using the TracingService will be written to the entry that CRM creates when a workflow executes.  You can open this record and inspect the exception information and determine where to correct logic or an error in the code.

- The **WorkflowContext** provides information passed in by CRM about the entity record being executed and data such as the OrganizationId, BusinessId and UserId.  You can utilize this information and save time by not having to retrieve it from CRM.

- The **OrganizationService** is what is used to communicate with CRM to perform CRUD and CRM business processes.

- Input and output parameters are optional, but they provide a great way to save additional time in retrieve information.  This also allows for information to be passed as the workflow is being executed to other steps.  To retrieve a value from an input parameter, use the `Get<T>()` method, and to set a value to an output parameter, use the `Set<T>()` method.

## Creating custom workflow in CRM 2015

This sample shows how to write a custom workflow activity that can create an account and a task for the account. This sample uses early binding.

``` csharp
using System;
using System.Activities;
using System.Collections.ObjectModel;

using Microsoft.Crm.Sdk.Messages;

using Microsoft.Xrm.Sdk;
using Microsoft.Xrm.Sdk.Messages;
using Microsoft.Xrm.Sdk.Query;
using Microsoft.Xrm.Sdk.Workflow;

namespace Microsoft.Crm.Sdk.Samples
{
	public sealed class SimpleSdkActivity : CodeActivity
	{
		protected override void Execute(CodeActivityContext executionContext)
		{
			//Create the tracing service
			ITracingService tracingService = executionContext.GetExtension<ITracingService>();

			//Create the context
			IWorkflowContext context = executionContext.GetExtension<IWorkflowContext>();
			IOrganizationServiceFactory serviceFactory = executionContext.GetExtension<IOrganizationServiceFactory>();
			IOrganizationService service = serviceFactory.CreateOrganizationService(context.UserId);

			tracingService.Trace("Creating Account");

			Account entity = new Account();
			entity.Name = AccountName.Get<string>(executionContext);
			Guid entityId = service.Create(entity);

			tracingService.Trace("Account created with Id {0}", entityId.ToString());

			tracingService.Trace("Create a task for the account");
			Task newTask = new Task();
			newTask.Subject = TaskSubject.Get<string>(executionContext);
			newTask.RegardingObjectId = new EntityReference(Account.EntityLogicalName, entityId);

			Guid taskId = service.Create(newTask);

			tracingService.Trace("Task has been created");

			tracingService.Trace("Retrieve the task using QueryByAttribute");
			QueryByAttribute query = new QueryByAttribute();
			query.Attributes.AddRange(new string[] { "regardingobjectid" });
			query.ColumnSet = new ColumnSet(new string[] { "subject" });
			query.EntityName = Task.EntityLogicalName;
			query.Values.AddRange(new object[] { entityId });

			tracingService.Trace("Executing the Query for entity {0}", query.EntityName);

			//Execute using a request to test the OOB (XRM) message contracts
			RetrieveMultipleRequest request = new RetrieveMultipleRequest();
			request.Query = query;
			Collection<Entity> entityList = ((RetrieveMultipleResponse)service.Execute(request)).EntityCollection.Entities;

			//Execute a request from the CRM message assembly
			tracingService.Trace("Executing a WhoAmIRequest");
			service.Execute(new WhoAmIRequest());

			if (1 != entityList.Count)
			{
				tracingService.Trace("The entity list was too long");
				throw new InvalidPluginExecutionException("Query did not execute correctly");
			}
			else
			{
				tracingService.Trace("Casting the Task from RetrieveMultiple to strong type");
				Task retrievedTask = (Task)entityList[0];

				if (retrievedTask.ActivityId != taskId)
				{
					throw new InvalidPluginExecutionException("Incorrect task was retrieved");
				}

				tracingService.Trace("Retrieving the entity from IOrganizationService");

				//Retrieve the task using Retrieve
				retrievedTask = (Task)service.Retrieve(Task.EntityLogicalName, retrievedTask.Id, new ColumnSet("subject"));
				if (!string.Equals(newTask.Subject, retrievedTask.Subject, StringComparison.Ordinal))
				{
					throw new InvalidPluginExecutionException("Task's subject did not get retrieved correctly");
				}

				//Update the task
				retrievedTask.Subject = UpdatedTaskSubject.Get<string>(executionContext);
				service.Update(retrievedTask);
			}
		}

		[Input("Account Name")]
		[Default("Test Account: {575A8B41-F8D7-4DCE-B2EA-3FFDE936AB1B}")]
		public InArgument<string> AccountName { get; set; }

		[Input("Task Subject")]
		[Default("Task related to account {575A8B41-F8D7-4DCE-B2EA-3FFDE936AB1B}")]
		public InArgument<string> TaskSubject { get; set; }

		[Input("Updated Task Subject")]
		[Default("UPDATED: Task related to account {575A8B41-F8D7-4DCE-B2EA-3FFDE936AB1B}")]
		public InArgument<string> UpdatedTaskSubject { get; set; }
	}

	public sealed class SdkWithLooselyTypesActivity : CodeActivity
	{
		protected override void Execute(CodeActivityContext executionContext)
		{
			//Create the tracing service
			ITracingService tracingService = executionContext.GetExtension<ITracingService>();

			//Create the context
			IWorkflowContext context = executionContext.GetExtension<IWorkflowContext>();
			IOrganizationServiceFactory serviceFactory = executionContext.GetExtension<IOrganizationServiceFactory>();
			IOrganizationService service = serviceFactory.CreateOrganizationService(context.UserId);

			tracingService.Trace("Creating Account");

			tracingService.Trace("Creating Account");

			Entity entity = new Entity("account");
			entity.Attributes["name"] = AccountName.Get<string>(executionContext);
			Guid entityId = service.Create(entity);

			tracingService.Trace("Account created with Id {0}", entityId.ToString());

			tracingService.Trace("Create a task for the account");
			Entity newTask = new Entity("task");
			newTask["subject"] = TaskSubject.Get<string>(executionContext);
			newTask["regardingobjectid"] = new EntityReference("account", entityId);

			Guid taskId = service.Create(newTask);

			tracingService.Trace("Task has been created");

			Entity updateTask = new Entity("task");
			updateTask.Id = taskId;
			updateTask["subject"] = UpdatedTaskSubject.Get<string>(executionContext);
			service.Update(updateTask);

			tracingService.Trace("Task has been updated");
		}

		[Input("Account Name")]
		[Default("Test Account: {575A8B41-F8D7-4DCE-B2EA-3FFDE936AB1B}")]
		public InArgument<string> AccountName { get; set; }

		[Input("Task Subject")]
		[Default("Task related to account {575A8B41-F8D7-4DCE-B2EA-3FFDE936AB1B}")]
		public InArgument<string> TaskSubject { get; set; }

		[Input("Updated Task Subject")]
		[Default("UPDATED: Task related to account {575A8B41-F8D7-4DCE-B2EA-3FFDE936AB1B}")]
		public InArgument<string> UpdatedTaskSubject { get; set; }
	}

	public sealed class SimpleSdkWithRelatedEntitiesActivity : CodeActivity
	{
		protected override void Execute(CodeActivityContext executionContext)
		{
			//Create the tracing service
			ITracingService tracingService = executionContext.GetExtension<ITracingService>();

			//Create the context
			IWorkflowContext context = executionContext.GetExtension<IWorkflowContext>();
			IOrganizationServiceFactory serviceFactory = executionContext.GetExtension<IOrganizationServiceFactory>();
			IOrganizationService service = serviceFactory.CreateOrganizationService(context.UserId);

			Relationship primaryContactRelationship = new Relationship("account_primary_contact");

			Guid primaryContactId = service.Create(new Contact() { LastName = RootContactLastName.Get<string>(executionContext) });

			//tracingService.Trace("Create an Account with an existing primary contact");
			Account rootAccount = new Account() { Name = RelatedAccountName.Get<string>(executionContext) };
			Contact primaryContact = new Contact()
			{
				EntityState = EntityState.Changed,
				Id = primaryContactId,
				FirstName = RootContactFirstName.Get<string>(executionContext)
			};

			rootAccount.RelatedEntities[primaryContactRelationship] =
				new EntityCollection(new Entity[] { primaryContact }) { EntityName = Contact.EntityLogicalName };

			tracingService.Trace("Execute the Create/Update");
			rootAccount.Id = service.Create(rootAccount);

			//Create the related entities query
			RelationshipQueryCollection retrieveRelatedEntities = new RelationshipQueryCollection();
			retrieveRelatedEntities[primaryContactRelationship] = new QueryExpression(Account.EntityLogicalName)
			{
				ColumnSet = new ColumnSet("name"),
				Criteria = new FilterExpression()
			};

			//Create the request
			RetrieveResponse response = (RetrieveResponse)service.Execute(new RetrieveRequest()
			{
				ColumnSet = new ColumnSet("firstname"),
				RelatedEntitiesQuery = retrieveRelatedEntities,
				Target = new EntityReference(primaryContact.LogicalName, primaryContact.Id)
			});

			tracingService.Trace("Retrieve the record with its related entities");
			Contact retrievedContact = (Contact)response.Entity;
			Account retrievedAccount = (Account)retrievedContact.RelatedEntities[primaryContactRelationship][0];

			tracingService.Trace("Ensure the first name was updated");
			if (retrievedContact.FirstName == primaryContact.FirstName)
			{
				tracingService.Trace("Primary Contact name is correct");
			}
			else
			{
				tracingService.Trace("First Name is \"{0}\", expected \"{1}\".", retrievedContact.FirstName, primaryContact.FirstName);
				throw new InvalidPluginExecutionException("The first name was not changed");
			}
		}

		[Input("Related Account Name")]
		[Default("Related Account")]
		public InArgument<string> RelatedAccountName { get; set; }

		[Input("Root Contact First Name")]
		[Default("New FirstName")]
		public InArgument<string> RootContactFirstName { get; set; }

		[Input("Root Contact Last Name")]
		[Default("Root LastName")]
		public InArgument<string> RootContactLastName { get; set; }
	}
}
```
