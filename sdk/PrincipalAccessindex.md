# SDK:ReterivePrincipalAccess
Retrieve the access rights of the specified security principal (team or user) to the specified record.

```
IOrganizationService _service; //Populate the CRM service

public void ReterivePrincipalAccess(Guid userId,EntityReference entityRef)
{
	// Create the request object.
	RetrievePrincipalAccessRequest access = new RetrievePrincipalAccessRequest();

	// Set the properties of the request object.
	access.Principal = new EntityReference("systemuser",userId); // GetUser id
	access.Target = entityRef; //new EntityReference("account",new Guid("6db75ab3-5197-e911-80e7-c49ecd784764"));

	// Execute the request.
	RetrievePrincipalAccessResponse accessResponse = (RetrievePrincipalAccessResponse)_service.Execute(access);
	accessResponse.Dump();
}
```
