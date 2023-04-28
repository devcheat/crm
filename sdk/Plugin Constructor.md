# Plug-in Constructor

The Microsoft Dynamics CRM platform has special support for a plug-in constructor that accepts two string parameters. If you write a constructor for your plug-in that accepts two string parameters, you can pass any two strings of information to the plug-in at run time.

The following code shows these two parameters.

```cs
using System;
using Microsoft.Crm.Sdk;
using Microsoft.Crm.SdkTypeProxy;

namespace MyPlugins
{
   public class AccountCreateHandler: IPlugin
   {
      public AccountCreateHandler(string unsecure, string secure)
      {
         // Do something with the parameter strings.
      }

      public void Execute(IPluginExecutionContext context)
      {
         // Do something here.
      }
   }
}
```

The first string parameter of the constructor contains public `unsecure` information. The second string parameter contains non-public `secure` information. 
> However, the secure string is not passed to a plug-in that executes while offline.


The information that is passed to the plug-in constructor in these two strings is specified when the plug-in is registered with Microsoft Dynamics CRM. 

When you use the PluginRegistration tool to register a plug-in, you can enter the secure and unsecure information in the Secure Configuration and Unsecure Configuration fields provided in the Register New Step form. 

The PluginDeveloper tool only supports the unsecure string through its CustomConfiguration attribute of the Step tag in the register.xml input file.
