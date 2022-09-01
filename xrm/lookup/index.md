
# Filter Lookup

Often when working with CRM Projects we can see requirements where we require a selection of other CRM records filtered to only records that are associated with an earlier user selection.

  A common example would be selecting a key contact or finance contact for an Opportunity or Order, but other examples of this could often be seen across different project requirements – say for selecting a Property from within a particular Portfolio, or selecting a particular Order to Invoice that related to a specific Customer Account.

`Xrm` methods provide us powerful methods for manipulating Lookup Controls to provide an additional method of implementing a filtered lookup.

Name | Description
--|--
addCustomView	|Adds an entirely new custom view to the Lookup Control.
setDefaultView	|Sets the initial default view when a user clicks on the Lookup Control.
getDefaultView	|Returns the GUID for the initial view when a user clicks on the Lookup Control.


**Xrm Code**
```js
var defaultViewId;
 
// FUNCTION: formOnLoad
function formOnLoad(accountFieldName, lookupFieldName) {
 
  defaultViewId = Xrm.Page.getControl(lookupFieldName).getDefaultView();
  setLookup(accountFieldName, lookupFieldName, false);
 
}
 
// FUNCTION: setLookup
function setLookup(accountFieldName, lookupFieldName, resetSelection) {
 
    // Get the selected Account Id in the [accountFieldName] indicated control
    var account = Xrm.Page.getAttribute(accountFieldName).getValue();
 
    if ( account != null )
    {
      var accountid = account[0].id;
      var accountname = account[0].name;
 
      if ( resetSelection == true )
      {
        // reset old selection for Contact
        Xrm.Page.getAttribute(lookupFieldName).setValue(null);
      }
 
      // use randomly generated GUID Id for our new view
      var viewId = "{1DFB2B35-B07C-44D1-868D-258DEEAB88E2}";
      var entityName = "contact";
 
      // give the custom view a name
      var viewDisplayName = "Active Contacts for " + accountname + "";
 
      // find all contacts where [Parent Customer] = [Account indicated by AccountId]
      // AND where [Statecode] = Active
      var fetchXml = "<fetch version='1.0' output-format='xml-platform' mapping='logical' distinct='false'>" +
                     "<entity name='contact'>" +
                     "<attribute name='firstname' />" +
                     "<attribute name='lastname' />" +
                     "<attribute name='contactid' />" +
                     "<order attribute='lastname' descending='false' />" +
                     "<filter type='and'>" +
                     "<condition attribute='parentcustomerid' operator='eq' value='" + accountid + "' />" +
                     "<condition attribute='statecode' operator='eq' value='0' />" +
                     "</filter>" +
                     "</entity>" +
                     "</fetch>";
 
      // build Grid Layout
      var layoutXml = "<grid name='resultset' " +
                               "object='1' " +
                               "jump='contactid' " +
                               "select='1' " +
                               "icon='1' " +
                               "preview='1'>" +
                           "<row name='result' " +
                                "id='contactid'>" +
                             "<cell name='firstname' " +
                                   "width='200' />" +
                             "<cell name='lastname' " +
                                   "width='250' />" +
                             "<cell name='parentcustomerid' " +
                                   "width='250' />" +
                           "</row>" +
                         "</grid>";
 
      // add the Custom View to the indicated [lookupFieldName] Control
      Xrm.Page.getControl(lookupFieldName).addCustomView(viewId, entityName, viewDisplayName, fetchXml, layoutXml, true);
    }
    else
    {
      // no Account selected, reset Contact Lookup View to the default view such that all Contacts are displayed for selection
      Xrm.Page.getControl(lookupFieldName).setDefaultView(defaultViewId);
    }
}
```
