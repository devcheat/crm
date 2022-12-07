Remove Priviledge from Role
====

Sample code as to how to remove a particular privilidge from an exisitng role and modify the role using SDK.

The following code will remove the `Print` & `Export` rights from the role for a list of priviledges look for the documantation here [**Security role UI to privilege mapping**](https://learn.microsoft.com/en-us/previous-versions/dynamicscrm-2016/developers-guide/hh547441(v=crm.8)?redirectedfrom=MSDN)

```cs
public void Execute(string roleName)
{
	var fetchXml = 
	$@"<fetch version='1.0' mapping='logical' distinct='false'>
      <entity name='roleprivileges'>                                      
         <attribute name='privilegeid'/>
         <attribute name='roleid'/>
         <link-entity name='role' alias='roles' to='roleid' from='roleid' link-type='inner'>
            <attribute name='name'/>
           <filter type='and'>
            <condition attribute='name' operator='neq' value='System Administrator' />
            <condition attribute='name' operator='eq' value='{roleName}' /> 
           </filter>
         </link-entity>
         <link-entity name='privilege' alias='privileges' to='privilegeid' from='privilegeid' link-type='inner'>
            <attribute name='name'/>
           <filter type='or'>
             <condition attribute='name' operator='eq' value='prvPrint' />
            <condition attribute='name' operator='eq' value='prvExportToExcel' />
          </filter>  
         </link-entity>
      </entity>
   </fetch>";

       var allRecordss = orgsvc.RetrieveMultiple(new FetchExpression(fetchXml));
       foreach (var record in allRecordss.Entities)
       {
         RemovePrivilegeRoleRequest removePrivilageRequest = new RemovePrivilegeRoleRequest()
         {
               PrivilegeId = new Guid(record["privilegeid"].ToString()),
               RoleId = new Guid(record["roleid"].ToString())
         };

           orgsvc.Execute(removePrivilageRequest);
           Console.WriteLine($"Removed '{((AliasedValue)record["privileges.name"]).Value.ToString()}' privilege from {((AliasedValue)record["roles.name"]).Value.ToString()} role.");			
       }
}
```
