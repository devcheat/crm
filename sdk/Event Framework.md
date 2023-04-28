# Event framework

The Event Framework provides the capability to register custom code to be run in response to specific events.

All capabilities to extend the default behavior of the platform depend on the event framework. When you configure a workflow to respond to an event using the workflow designer without writing code, that event is provided by the event framework.

You will use the Plug-in Registration tool to configure plug-ins, Azure integrations, virtual table data providers, and Webhooks to respond to events that are provided by the event framework. When events occur and an extension is registered to respond to them, contextual information about the data involved in the operation is passed to the extension. Depending on how the registration for the event is configured, the extension can modify the data passed into it, initiate some automated process to be applied immediately, or define that an action is added to a queue to be performed later.

There are Create, Retrieve, RetrieveMultiple, Update, Delete, Associate and Disassociate messages that cover the basic data operations that happen with tables. There are also specialized messages for more complex operations, and custom actions add new messages.

When you use the Plug-in Registration tool to associate an extension with a particular message, you will register it as a step. A step provides the information about which message the extensions should respond to as well as a number of other configuration choices. Use the Message field to choose the message your extension will respond to.

Generally, you can expect to find a message for most of the *Request classes in the Microsoft.Crm.Sdk.Messages or Microsoft.Xrm.Sdk.Messages namespaces, but you will also find messages for any custom actions that have been created in the organization. Any operations involving table definitions are not available.

Data about messages is stored in the SdkMessage and SdkMessageFilter tables. The Plug-in registration tool will filter this information to only show valid messages.

## Event execution pipeline
When you register a step using the Plug-in Registration tool you must also choose the Event Pipeline Stage of Execution. Each message is processed in a series of 4 stages as described in the following table:
- The stage you should choose depends on the purpose of the extension. 
- You don't need to apply all your business logic within a single step. 
- You can apply multiple steps so that your logic about whether to allow a operation to proceed can be in the PreValidation stage and your logic to make modifications to the message properties can occur in the PostOperation stage.
- Multiple extensions can be registered for the same message and stage. Within the step registration the Execution Order value determines the order in which multiple extensions should be processed for a given stage.

### `PreValidation`
- For the initial operation, this stage will occur before the main system operation. 
- This provides an opportunity to include logic to cancel the operation before the database transaction.Subsequent operations triggered by extensions registered in other stages will pass through this stage as well but will be included within the transaction of the calling extensions. 
- This stage occurs before any security checks are preformed to verify that the calling or logged-on user has the correct permission to perform the intended operation.

### `PreOperation`
- Occurs before the main system operation and within the database transaction.
- If you want to change any values for an entity included in the message, you should do it here.
- Avoid cancelling an operation here. Canceling will trigger a rollback of the transaction and have significant performance impact.

### `MainOperation`
- For internal use only except for Custom API and Custom virtual table data providers.

### `PostOperation`
- Occurs after the main system operation and within the database transaction.
- Use this stage to modify any properties of the message before it is returned to the caller.
- Avoid applying changes to an entity included in the message because this will trigger a new Update event.
- Within the PostOperation stage you can register steps to use the asynchronous execution mode. These steps will run outside of the database transaction using the asynchronous service.
More information Asynchronous service.





