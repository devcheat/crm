**XRM Script (MSCRM Client JS) `8.2`**
===

> # Tabel of Contents
> - [Add OnChange (5.0+)](#add-onchange-50)
> - [Fire OnChange (5.0+)](#fire-onchange-50)
> - [Get Attribute Name (5.0+)](#get-attribute-name-50)
> - [Get Attribute Type (5.0+)](#get-attribute-type-50)
> - [Get Format (5.0+)](#get-format-50)
> - [Get Initial Value (5.0+)](#get-initial-value-50)
> - [Get IsDirty (5.0+)](#get-isdirty-50)
> - [Get IsPartyList (6.0+)](#get-ispartylist-60)
> - [Get Max (5.0+)](#get-max-50)
> - [Get Max Length (5.0+)](#get-max-length-50)
> - [Get Min (5.0+)](#get-min-50)
> - [Get Option (5.0+)](#get-option-50)
> - [Get Options (5.0+)](#get-options-50)
> - [Get Parent (5.0+)](#get-parent-50)
> - [Get Precision (5.0+)](#get-precision-50)
> - [Get Required Level (5.0+)](#get-required-level-50)
> - [Get Selected Option (5.0+)](#get-selected-option-50)
> - [Get Submit Mode (5.0+)](#get-submit-mode-50)
> - [Get Text (5.0+)](#get-text-50)
> - [Get User Privilege (5.0+)](#get-user-privilege-50)
> - [Get Attribute Value (5.0+)](#get-attribute-value-50)
> - [Remove OnChange (5.0+)](#remove-onchange-50)
> - [Set Required Level (5.0+)](#set-required-level-50)
> - [Set Submit Mode (5.0+)](#set-submit-mode-50)
> - [Set Attribute Value (5.0+)](#set-attribute-value-50)
> - [Add OnStageChange (7.0+)](#add-onstagechange-70)
> - [Add OnStageSelected (7.0+)](#add-onstageselected-70)
> - [Get Active Path (7.0+)](#get-active-path-70)
> - [Get Active Process (7.0+)](#get-active-process-70)
> - [Get Active Stage (7.0+)](#get-active-stage-70)
> - [Get Display State (7.1+)](#get-display-state-71)
> - [Get Enabled Processes (7.0+)](#get-enabled-processes-70)
> - [Get Selected Stage (7.1+)](#get-selected-stage-71)
> - [Get Visible (7.1+)](#get-visible-71)
> - [Move Next (7.0+)](#move-next-70)
> - [Move Previous (7.0+)](#move-previous-70)
> - [Remove OnStageChange (7.0+)](#remove-onstagechange-70)
> - [Set Active Process (7.0+)](#set-active-process-70)
> - [Set Active Stage (7.0+)](#set-active-stage-70)
> - [Set Display State (7.0+)](#set-display-state-70)
> - [Set Visible (7.0+)](#set-visible-70)
> - [ForEach Attributes (5.0+)](#foreach-attributes-50)
> - [ForEach Controls (5.0+)](#foreach-controls-50)
> - [ForEach Form Selector (5.0+)](#foreach-form-selector-50)
> - [ForEach Navigation Item (5.0+)](#foreach-navigation-item-50)
> - [ForEach Sections (5.0+)](#foreach-sections-50)
> - [ForEach Section Controls (5.0+)](#foreach-section-controls-50)
> - [ForEach Tabs (5.0+)](#foreach-tabs-50)
> - [Get Attributes (5.0+)](#get-attributes-50)
> - [Get Attributes Count (5.0+)](#get-attributes-count-50)
> - [Get Controls (5.0+)](#get-controls-50)
> - [Get Controls Count (5.0+)](#get-controls-count-50)
> - [Get Form Selectors (5.0+)](#get-form-selectors-50)
> - [Get Form Selectors Count (5.0+)](#get-form-selectors-count-50)
> - [Get Navigation Items. (5.0+)](#get-navigation-items-50)
> - [Get Navigation Items Count (5.0+)](#get-navigation-items-count-50)
> - [Get Section Controls (5.0+)](#get-section-controls-50)
> - [Get Sections (5.0+)](#get-sections-50)
> - [Get Sections Count (5.0+)](#get-sections-count-50)
> - [Get Tabs (5.0+)](#get-tabs-50)
> - [Get Tabs Count (5.0+)](#get-tabs-count-50)
> - [Get Client (6.0+)](#get-client-60)
> - [Get Client State (6.0+)](#get-client-state-60)
> - [Get Client Url (5.0+)](#get-client-url-50)
> - [Get Current Theme (5.0+)](#get-current-theme-50)
> - [Get FormFactor (7.1+)](#get-formfactor-71)
> - [Get Is Auto Save Enabled (7.0+)](#get-is-auto-save-enabled-70)
> - [Get LCID (5.0+)](#get-lcid-50)
> - [Get Org Unique Name (5.0+)](#get-org-unique-name-50)
> - [Get Query String Parameters (5.0+)](#get-query-string-parameters-50)
> - [Get Time Zone Offset Minutes (7.1+)](#get-time-zone-offset-minutes-71)
> - [Get User Id (5.0+)](#get-user-id-50)
> - [Get User Lcid (5.0+)](#get-user-lcid-50)
> - [Get User Name (6.0+)](#get-user-name-60)
> - [Get User Roles (5.0+)](#get-user-roles-50)
> - [Is Outlook Client (5.0-6.1)](#is-outlook-client-50-61)
> - [Is Outlook Online (5.0-6.1)](#is-outlook-online-50-61)
> - [Prepend OrgName (5.0+)](#prepend-orgname-50)
> - [Add Custom Filter (6.0+)](#add-custom-filter-60)
> - [Add Custom View (5.0+)](#add-custom-view-50)
> - [Add OnKeyPress (8.0+)](#add-onkeypress-80)
> - [Add Option (5.0+)](#add-option-50)
> - [Add PreSearch (6.0+)](#add-presearch-60)
> - [Clear Notification (6.0+)](#clear-notification-60)
> - [Clear Options (5.0+)](#clear-options-50)
> - [Fire OnKeyPress (8.0+)](#fire-onkeypress-80)
> - [Get Attribute (5.0+)](#get-attribute-50)
> - [Control Type (5.0+)](#control-type-50)
> - [Get Data (5.0+)](#get-data-50)
> - [Get Default View (5.0+)](#get-default-view-50)
> - [Get Disabled (5.0+)](#get-disabled-50)
> - [Get Initial Url (5.0+)](#get-initial-url-50)
> - [Get Label (5.0+)](#get-label-50)
> - [Get Name (5.0+)](#get-name-50)
> - [Get Object (5.0+)](#get-object-50)
> - [Get Parent (5.0+)](#get-parent-50-1)
> - [Get Show Time (7.1+)](#get-show-time-71)
> - [Get Src (5.0+)](#get-src-50)
> - [Get Visible (5.0+)](#get-visible-50)
> - [Hide AutoComplete (8.0+)](#hide-autocomplete-80)
> - [Remove OnKeyPress (8.0+)](#remove-onkeypress-80)
> - [Remove Option (5.0+)](#remove-option-50)
> - [Remove PreSearch (6.0+)](#remove-presearch-60)
> - [Set Data (5.0+)](#set-data-50)
> - [Set Default View (5.0+)](#set-default-view-50)
> - [Set Disabled (5.0+)](#set-disabled-50)
> - [Set Focus (5.0+)](#set-focus-50)
> - [Set Notification (6.0+)](#set-notification-60)
> - [Set Show Time (6.0+)](#set-show-time-60)
> - [Set Src (5.0+)](#set-src-50)
> - [Set Visible (5.0+)](#set-visible-50)
> - [Show AutoComplete (8.0+)](#show-autocomplete-80)
> - [Add OnSave (5.0+)](#add-onsave-50)
> - [Get Data Xml (5.0+)](#get-data-xml-50)
> - [Get Entity Name (5.0+)](#get-entity-name-50)
> - [Get Id (5.0+)](#get-id-50)
> - [Get IsDirty (5.0+)](#get-isdirty-50-1)
> - [Get Primary Attribute Value (6.0+)](#get-primary-attribute-value-60)
> - [Remove OnSave (5.0+)](#remove-onsave-50)
> - [Save (5.0+)](#save-50)
> - [Get Form Id (5.0+)](#get-form-id-50)
> - [Get Form Label (5.0+)](#get-form-label-50)
> - [Navigate (5.0+)](#navigate-50)
> - [Add OnPostSearch (8.1+)](#add-onpostsearch-81)
> - [Add OnResultOpened (7.1+)](#add-onresultopened-71)
> - [Add OnSelection (7.1+)](#add-onselection-71)
> - [Get Search Query (7.1+)](#get-search-query-71)
> - [Get Selected Result (7.1+)](#get-selected-result-71)
> - [Get TotalResultCount (8.1+)](#get-totalresultcount-81)
> - [Open Search Result (8.1+)](#open-search-result-81)
> - [Remove OnPostSearch (8.1+)](#remove-onpostsearch-81)
> - [Remove OnResultOpened (7.1+)](#remove-onresultopened-71)
> - [Remove OnSelection (7.1+)](#remove-onselection-71)
> - [Set Search Query (7.1+)](#set-search-query-71)
> - [Get Navigation Id (5.0+)](#get-navigation-id-50)
> - [Get Navigation Label (5.0+)](#get-navigation-label-50)
> - [Get Navigation Visible (5.0+)](#get-navigation-visible-50)
> - [Set Navigation Focu (5.0+)](#set-navigation-focu-50)
> - [Set Navigation Label (5.0+)](#set-navigation-label-50)
> - [Set Navigation Visible (5.0+)](#set-navigation-visible-50)
> - [Get Process Id (7.0+)](#get-process-id-70)
> - [Get Process Name (7.0+)](#get-process-name-70)
> - [Get Sections (7.0+)](#get-sections-70)
> - [Is Rendered(7.0+)](#is-rendered70)
> - [Get Save Mode (5.0+)](#get-save-mode-50)
> - [Is Default Prevented (5.0+)](#is-default-prevented-50)
> - [Prevent Default (5.0+)](#prevent-default-50)
> - [Get Context (5.0+)](#get-context-50)
> - [Get Depth (5.0+)](#get-depth-50)
> - [Get Event Source (5.0+)](#get-event-source-50)
> - [Get Shared Variable (5.0+)](#get-shared-variable-50)
> - [Set Shared Variable (5.0+)](#set-shared-variable-50)
> - [Get Label (5.0+)](#get-label-50-1)
> - [Get Name (5.0+)](#get-name-50-1)
> - [Get Parent (5.0+)](#get-parent-50-2)
> - [Get Visible (5.0+)](#get-visible-50-1)
> - [Set Label (5.0+)](#set-label-50)
> - [Set Visible (5.0+)](#set-visible-50-1)
> - [Get Category (7.0+)](#get-category-70)
> - [Get Entity Name (7.0+)](#get-entity-name-70)
> - [Get Id (7.0+)](#get-id-70)
> - [Get Name (7.0+)](#get-name-70)
> - [Get Name (7.0+)](#get-name-70-1)
> - [Get Name (7.0+)](#get-name-70-2)
> - [Get Attribute (7.0+)](#get-attribute-70)
> - [Get Name (7.0+)](#get-name-70-3)
> - [IsRequired (7.0+)](#isrequired-70)
> - [Add Grid OnLoad (7.1+)](#add-grid-onload-71)
> - [Get Grid (7.1+)](#get-grid-71)
> - [Get Grid Control (7.1+)](#get-grid-control-71)
> - [Get Grid Current View (7.1+)](#get-grid-current-view-71)
> - [Get Grid Entity Name (7.1+)](#get-grid-entity-name-71)
> - [Get Row Data (7.1+)](#get-row-data-71)
> - [Get Row Entity (7.1+)](#get-row-entity-71)
> - [Get Row Entity Id (7.1+)](#get-row-entity-id-71)
> - [Get Row Entity Name (7.1+)](#get-row-entity-name-71)
> - [Get Row Entity Primary Attribute Value (7.1+)](#get-row-entity-primary-attribute-value-71)
> - [Get Row Entity Reference (7.1+)](#get-row-entity-reference-71)
> - [Get Rows (7.1+)](#get-rows-71)
> - [Get Selected Rows (7.1+)](#get-selected-rows-71)
> - [Get Total Record Count (7.1+)](#get-total-record-count-71)
> - [Get View Selector (7.1+)](#get-view-selector-71)
> - [Is View Selector Visible (7.1+)](#is-view-selector-visible-71)
> - [Refresh (5.0+)](#refresh-50)
> - [Remove Grid OnLoad (7.1+)](#remove-grid-onload-71)
> - [Set Grid Current View (7.1+)](#set-grid-current-view-71)
> - [Get Display State (5.0+)](#get-display-state-50)
> - [Get Tab Label (5.0+)](#get-tab-label-50)
> - [Get Tab Name (5.0+)](#get-tab-name-50)
> - [Get Tab Parent (5.0+)](#get-tab-parent-50)
> - [Get Tab Visible (5.0+)](#get-tab-visible-50)
> - [Set Tab Display State (5.0+)](#set-tab-display-state-50)
> - [Set Tab Focus (5.0+)](#set-tab-focus-50)
> - [Set Tab Label (5.0+)](#set-tab-label-50)
> - [Set Tab Visible (5.0+)](#set-tab-visible-50)
> - [Clear Form Notification (6.0+)](#clear-form-notification-60)
> - [Get Current Control (5.0+)](#get-current-control-50)
> - [Close (5.0+)](#close-50)
> - [Get Form Type (5.0+)](#get-form-type-50)
> - [Get ViewPort Width(5.0+)](#get-viewport-width50)
> - [Get ViewPort Height (5.0+)](#get-viewport-height-50)
> - [Refresh Ribbon (5.0+)](#refresh-ribbon-50)
> - [Set Form Notification (6.0+)](#set-form-notification-60)
> - [Alert Dialog (6.0+)](#alert-dialog-60)
> - [Confirm Dialog (6.0+)](#confirm-dialog-60)
> - [Is Activity Type (6.0+)](#is-activity-type-60)
> - [Open Entity Form (5.0+)](#open-entity-form-50)
> - [Open Quick Create (7.1+)](#open-quick-create-71)
> - [Open Web Resource (5.0+)](#open-web-resource-50)
> - [Is Offline Enabled (8.2+)](#is-offline-enabled-82)
> - [Create Offline Record (8.2+)](#create-offline-record-82)
> - [Retrieve Offline Record (8.2+)](#retrieve-offline-record-82)
> - [Retrieve Multiple Offline Records (8.2+)](#retrieve-multiple-offline-records-82)
> - [Update Offline Record (8.2+)](#update-offline-record-82)
> - [Delete Offline Record (8.2+)](#delete-offline-record-82)
> - [Add Cell Notification (8.2+)](#add-cell-notification-82)
> - [Get Barcode Value (8.2+)](#get-barcode-value-82)
> - [Get Current Position (8.2+)](#get-current-position-82)
> - [Load Panel (8.2+)](#load-panel-82)
> - [Get Form Context (8.2+)](#get-form-context-82)
> - [Get Process Instances (8.2+)](#get-process-instances-82)
> - [Set Active Process Instance (8.2+)](#set-active-process-instance-82)
> - [Add OnProcessStatusChange (8.2+)](#add-onprocessstatuschange-82)
> - [Remove OnProcessStatusChange (8.2+)](#remove-onprocessstatuschange-82)
> - [Get Instance Id (8.2+)](#get-instance-id-82)
> - [Get Instance Name (8.2+)](#get-instance-name-82)
> - [Get Status (8.2+)](#get-status-82)
> - [Set Status (8.2+)](#set-status-82)
> - [Get QuickForm Controls (8.1+)](#get-quickform-controls-81)
> - [ForEach QuickForm Controls (8.1+)](#foreach-quickform-controls-81)
> - [QuickForm Get Control (8.1+)](#quickform-get-control-81)
> - [QuickForm Get Control Type (8.1+)](#quickform-get-control-type-81)
> - [QuickForm Get Name (8.1+)](#quickform-get-name-81)
> - [QuickForm Get Parent (8.1+)](#quickform-get-parent-81)
> - [QuickForm Get Visible (8.1+)](#quickform-get-visible-81)
> - [QuickForm Get Label (8.1+)](#quickform-get-label-81)
> - [QuickForm Set Label (8.1+)](#quickform-set-label-81)
> - [QuickForm Is Loaded (8.1+)](#quickform-is-loaded-81)
> - [QuickForm Refresh (8.1+)](#quickform-refresh-81)

---

# Add OnChange (5.0+)
>  XrmAttributeAddOnChange
5.0+ Sets a function to be called when the attribute value is changed.
```js
Xrm.Page.getAttribute("${fieldname}").addOnChange(${functionName});
```
 
# Fire OnChange (5.0+)
>  XrmAttributeFireOnChange
5.0+ Causes the OnChange event to occur on the attribute so that any script associated to that event can execute.
```js
Xrm.Page.getAttribute("${fieldname}").fireOnChange();
```
 
# Get Attribute Name (5.0+)
>  XrmAttributeGetName
5.0+ Returns a string representing the logical name of the attribute.
```js
Xrm.Page.getAttribute("${fieldname}").getName();
```
 
# Get Attribute Type (5.0+)
>  XrmAttributeGetAttributeType
5.0+ Returns a string value that represents the type of attribute.
```js
Xrm.Page.getAttribute("${fieldname}").getAttributeType();
```
 
# Get Format (5.0+)
>  XrmAttributeGetFormat
5.0+ Returns a string value that represents formatting options for the attribute.
```js
Xrm.Page.getAttribute("${fieldname}").getFormat();
```
 
# Get Initial Value (5.0+)
>  XrmAttributeGetInitialValue
5.0+ Returns a value that represents the value set for an OptionSet or Boolean attribute when the form opened.
```js
Xrm.Page.getAttribute("${fieldname}").getInitialValue();
```
 
# Get IsDirty (5.0+)
>  XrmAttributeGetIsDirty
5.0+ Returns a Boolean value indicating if there are unsaved changes to the attribute value.
```js
Xrm.Page.getAttribute("${fieldname}").getIsDirty();
```
 
# Get IsPartyList (6.0+)
>  XrmAttributeGetIsPartyList
6.0+ Returns a Boolean value indicating whether the lookup represents a partylist lookup. Partylist lookups allow for multiple records to be set, such as the To: field for an email entity record.
```js
Xrm.Page.getAttribute("${fieldname}").getIsPartyList();
```
 
# Get Max (5.0+)
>  XrmAttributeGetMax
5.0+ Returns a number indicating the maximum allowed value for an attribute.
```js
Xrm.Page.getAttribute("${fieldname}").getMax();
```
 
# Get Max Length (5.0+)
>  XrmAttributeGetMaxLength
5.0+ Returns a number indicating the maximum length of a string or memo attribute.
```js
Xrm.Page.getAttribute("${fieldname}").getMaxLength();
```
 
# Get Min (5.0+)
>  XrmAttributeGetMin
5.0+ Returns a number indicating the minimum allowed value for an attribute.
```js
Xrm.Page.getAttribute("${fieldname}").getMin();
```
 
# Get Option (5.0+)
>  XrmAttributeGetOption
5.0+ Returns an option object with the value matching the argument passed to the method.
```js
Xrm.Page.getAttribute("${fieldname}").getOption(${value});
```
 
# Get Options (5.0+)
>  XrmAttributeGetOptions
5.0+ Returns an array of option objects representing the valid options for an optionset attribute.
```js
Xrm.Page.getAttribute("${fieldname}").getOptions();
```
 
# Get Parent (5.0+)
>  XrmAttributeGetParent
5.0+ Returns the Xrm.Page.data.entity object that is the parent to all attributes.
```js
Xrm.Page.getAttribute("${fieldname}").getParent();
```
 
# Get Precision (5.0+)
>  XrmAttributeGetPrecision
5.0+ Returns the number of digits allowed to the right of the decimal point.
```js
Xrm.Page.getAttribute("${fieldname}").getPrecision();
```
 
# Get Required Level (5.0+)
>  XrmAttributeGetRequiredLevel
5.0+ Returns a string value indicating whether a value for the attribute is required or recommended.
```js
Xrm.Page.getAttribute("${fieldname}").getRequiredLevel();
```
 
# Get Selected Option (5.0+)
>  XrmAttributeGetSelectedOption
5.0+ Returns the option object that is selected in an optionset attribute.
```js
Xrm.Page.getAttribute("${fieldname}").getSelectedOption();
```
 
# Get Submit Mode (5.0+)
>  XrmAttributeGetSubmitMode
5.0+ Returns a string indicating when data from the attribute will be submitted when the record is saved.
```js
Xrm.Page.getAttribute("${fieldname}").getSubmitMode();
```
 
# Get Text (5.0+)
>  XrmAttributeGetText
5.0+ Returns a string value of the text for the currently selected option for an optionset attribute.
```js
Xrm.Page.getAttribute("${fieldname}").getText();
```
 
# Get User Privilege (5.0+)
>  XrmAttributeGetUserPrivilege
5.0+ Returns an object with three Boolean properties corresponding to privileges indicating if the user can create, read or update data values for an attribute.
```js
Xrm.Page.getAttribute("${fieldname}").getUserPrivilege();
```
 
# Get Attribute Value (5.0+)
>  XrmAttributeGetValue
5.0+ Retrieves the data value for an attribute.
```js
Xrm.Page.getAttribute("${fieldname}").getValue();
```
 
# Remove OnChange (5.0+)
>  XrmAttributeRemoveOnChange
5.0+ Removes a function from the OnChange event hander for an attribute.
```js
Xrm.Page.getAttribute("${fieldname}").removeOnChange(${functionName});
```
 
# Set Required Level (5.0+)
>  XrmAttributeSetRequiredLevel
5.0+ Sets whether data is required or recommended for the attribute before the record can be saved.
```js
Xrm.Page.getAttribute("${fieldname}").setRequiredLevel("${none|required|recommended}");
```
 
# Set Submit Mode (5.0+)
>  XrmAttributeSetSubmitMode
5.0+ Sets whether data from the attribute will be submitted when the record is saved.
```js
Xrm.Page.getAttribute("${fieldname}").setSubmitMode("${always|never|dirty}");
```
 
# Set Attribute Value (5.0+)
>  XrmAttributeSetValue
5.0+ Sets the data value for an attribute.
```js
Xrm.Page.getAttribute("${fieldname}").setValue("${value}");
```
 
# Add OnStageChange (7.0+)
>  XrmProcessAddOnStageChange
7.0+ Use these methods to add or remove event handlers for the business process flow control.
```js
Xrm.Page.data.process.addOnStageChange(${functionName});
```
 
# Add OnStageSelected (7.0+)
>  XrmProcessAddOnStageSelected
7.0+ Use this to add a function as an event handler for the OnStageSelected event so that it will be called when a business process flow stage is selected.
```js
Xrm.Page.data.process.addOnStageSelected(${functionName});
```
 
# Get Active Path (7.0+)
>  XrmProcessGetActivePath
7.0+ Use this method to get a collection of stages currently in the active path with methods to interact with the stages displayed in the business process flow control.
```js
Xrm.Page.data.process.getActivePath();
```
 
# Get Active Process (7.0+)
>  XrmProcessGetActiveProcess
7.0+ Returns a Process object representing the active process.
```js
Xrm.Page.data.process.getActiveProcess();
```
 
# Get Active Stage (7.0+)
>  XrmProcessGetActiveStage
7.0+ Use getActiveStage to retrieve information about the active stage and setActiveStage to set a different stage as the active stage.
```js
Xrm.Page.data.process.getActiveStage();
```
 
# Get Display State (7.1+)
>  XrmProcessGetDisplayState
7.1+ Retrieve the display state for the business process control.
```js
Xrm.Page.ui.process.getDisplayState();
```
 
# Get Enabled Processes (7.0+)
>  XrmProcessGetEnabledProcesses
7.0+ Use this method to asynchronously retrieve the enabled business process flows that the user can switch to for an entity.
```js
Xrm.Page.data.process.getEnabledProcesses(${callbackfunctionName}(enabledProcesses));
```
 
# Get Selected Stage (7.1+)
>  XrmProcessGetSelectedStage
7.1+ Use this method to get the currently selected stage.
```js
Xrm.Page.data.process.getSelectedStage();
```
 
# Get Visible (7.1+)
>  XrmProcessGetVisible
7.1+ Retrieve whether the business process control is visible.
```js
Xrm.Page.ui.process.getVisible();
```
 
# Move Next (7.0+)
>  XrmProcessMoveNext
7.0+ Progresses to the next stage.
```js
Xrm.Page.data.process.moveNext(${callbackfunctionName});
```
 
# Move Previous (7.0+)
>  XrmProcessMovePrevious
7.0+ Moves to the previous stage. Use movePrevious to a previous stage in a different entity.
```js
Xrm.Page.data.process.movePrevious(${callbackfunctionName});
```
 
# Remove OnStageChange (7.0+)
>  XrmProcessRemoveOnStageChange
7.0+ Use this to remove a function as an event handler for the OnStageChange event.
```js
Xrm.Page.data.process.removeOnStageChange(${functionName});
```
 
##Remove OnStageSelected (7.0+)
>  XrmProcessRemoveOnStageSelected
7.0+ Use this to remove a function as an event handler for the OnStageSelected event.
```js
Xrm.Page.data.process.removeOnStageSelected(${functionName});
```
 
# Set Active Process (7.0+)
>  XrmProcessSetActiveProcess
7.0+ Set a Process as the active process.
```js
Xrm.Page.data.process.setActiveProcess("${processId}, ${callbackfunctionName});
```
 
# Set Active Stage (7.0+)
>  XrmProcessSetActiveStage
7.0+ Set a completed stage as the active stage.
```js
Xrm.Page.data.process.setActiveStage("${stageId}, ${callbackfunctionName});
```
 
# Set Display State (7.0+)
>  XrmProcessSetDisplayState
7.0+ Use this method to expand or collapse the business process flow control.
```js
Xrm.Page.ui.process.setDisplayState("${expanded|collapsed}");
```
 
# Set Visible (7.0+)
>  XrmProcessSetVisible
7.0+ Use this method to show or hide the business process flow control.
```js
Xrm.Page.ui.process.setVisible(${true|false});
```
 
# ForEach Attributes (5.0+)
>  XrmEntityAttributesForEach
5.0+ Apply an action in a delegate function to each object in the collection of attributes.
```js
Xrm.Page.data.entity.attributes.forEach(function (control, index) 
	{
		${functionbody}
	});
```
 
# ForEach Controls (5.0+)
>  XrmControlsForEach
5.0+ Apply an action in a delegate function to each object in the collection of controls.
```js
Xrm.Page.ui.controls.forEach(function (control, index) 
	{
		${functionbody}
	});
```
 
# ForEach Form Selector (5.0+)
>  XrmFormSelectorItemsForEach
5.0+ Apply an action in a delegate function to each object in the collection of forms.
```js
Xrm.Page.ui.formSelector.items.forEach(function (control, index) 
	{
        ${functionbody}
	});
```
 
# ForEach Navigation Item (5.0+)
>  XrmNavigationItemsForEach
5.0+ Apply an action in a delegate function to each object in the collection of navigation items.
```js
Xrm.Page.ui.navigation.items.forEach(function (control, index) 
	{
		${functionbody}
	});
```
 
# ForEach Sections (5.0+)
>  XrmTabsSectionsForEach
5.0+ Apply an action in a delegate function to each object in the collection of sections.
```js
Xrm.Page.ui.tabs.get(${null|"tabname"|tabindex}).sections.forEach(function (control, index) 
	{
		${functionbody}
	});
```
 
# ForEach Section Controls (5.0+)
>  XrmTabsSectionsControlsForEach
5.0+ Apply an action in a delegate function to each object in the collection of controls in a section.
```js
Xrm.Page.ui.tabs.get(${null|"tabname"|tabindex}).sections.get(${null|"sectionname"|sectionindex}).controls.forEach(function (control, index) 
	{
		${functionbody}
    });
```
 
# ForEach Tabs (5.0+)
>  XrmTabsForEach
5.0+ Apply an action in a delegate function to each object in the collection of tabs.
```js
Xrm.Page.ui.tabs.forEach(function (control, index) 
	{
		${functionbody}
	});
```
 
# Get Attributes (5.0+)
>  XrmEntityAttributes
5.0+ A collection of all the attributes on the page.
```js
Xrm.Page.data.entity.attributes.get();
```
 
# Get Attributes Count (5.0+)
>  XrmEntityAttributesGetLength
5.0+ Get the number of items in the collection of attributes.
```js
Xrm.Page.data.entity.attributes.getLength();
```
 
# Get Controls (5.0+)
>  XrmControls
5.0+ A collection of all the controls on the page.
```js
Xrm.Page.ui.controls.get();
```
 
# Get Controls Count (5.0+)
>  XrmControlsGetLength
5.0+ Get the number of items in the collection of controls.
```js
Xrm.Page.ui.controls.getLength();
```
 
# Get Form Selectors (5.0+)
>  XrmFormSelectorItems
5.0+ A collection of all the forms on the page.
```js
Xrm.Page.ui.formSelector.items.get();
```
 
# Get Form Selectors Count (5.0+)
>  XrmFormSelectorItemsGetLength
5.0+ Get the number of items in the collection of forms.
```js
Xrm.Page.ui.formSelector.items.getLength();
```
 
# Get Navigation Items. (5.0+)
>  XrmNavigationItems
5.0+ A collection of all the navigation items on the page.
```js
Xrm.Page.ui.navigation.items.get();
```
 
# Get Navigation Items Count (5.0+)
>  XrmNavigationItemsGetLength
5.0+ Get the number of items in the collection of navigation items.
```js
Xrm.Page.ui.navigation.items.getLength();
```
 
# Get Section Controls (5.0+)
>  XrmTabsSectionsControls
5.0+ A collection of all the controls in the section.
```js
Xrm.Page.ui.tabs.get(${null|"tabname"|tabindex}).sections.get(${null|"sectionname"|sectionindex}).controls.get();
```
 
# Get Sections (5.0+)
>  XrmTabsSections
5.0+ A collection of all the sections in a tab.
```js
Xrm.Page.ui.tabs.get(${null|"tabname"|tabindex}).sections.get();
```
 
# Get Sections Count (5.0+)
>  XrmTabsSectionsGetLength
5.0+ Get the number of items in the collection of sections.
```js
Xrm.Page.ui.tabs.get(${null|"tabname"|tabindex}).sections.getLength();
```
 
# Get Tabs (5.0+)
>  XrmTabs
5.0+ A collection of all the tabs on the page.
```js
Xrm.Page.ui.tabs.get(${null|"tabname"|tabindex});
```
 
# Get Tabs Count (5.0+)
>  XrmTabsGetLength
5.0+ Get the number of items in the collection of tabs.
```js
Xrm.Page.ui.tabs.getLength();
```
 
# Get Client (6.0+)
>  XrmContextClientGetClient
6.0+ Returns a value to indicate which client the script is executing in.
```js
Xrm.Page.context.client.getClient();
```
 
# Get Client State (6.0+)
>  XrmContextClientGetClientState
6.0+ Returns a value to indicate the state of the client.
```js
Xrm.Page.context.client.getClientState();
```
 
# Get Client Url (5.0+)
>  XrmContextGetClientUrl
5.0+ Returns the base URL that was used to access the application.
```js
Xrm.Page.context.getClientUrl();
```
 
# Get Current Theme (5.0+)
>  XrmContextGetCurrentTheme
5.0+ Returns a string representing the current Microsoft Office Outlook theme chosen by the user.
```js
Xrm.Page.context.getCurrentTheme();
```
 
# Get FormFactor (7.1+)
>  XrmContextClientGetFormFactor
7.1+ Use this method to get information about the kind of device the user is using.
```js
Xrm.Page.context.client.getFormFactor();
```
 
# Get Is Auto Save Enabled (7.0+)
>  XrmContextGetIsAutoSaveEnabled
7.0+ Returns whether Autosave is enabled for the organization.
```js
Xrm.Page.context.getIsAutoSaveEnabled();
```
 
# Get LCID (5.0+)
>  XrmContextGetOrgLcid
5.0+ Returns the LCID value that represents the language pack that is the base language for the organization.
```js
Xrm.Page.context.getOrgLcid();
```
 
# Get Org Unique Name (5.0+)
>  XrmContextGetOrgUniqueName
5.0+ Returns the unique text value of the organization’s name.
```js
Xrm.Page.context.getOrgUniqueName();
```
 
# Get Query String Parameters (5.0+)
>  XrmContextGetQueryStringParameters
5.0+ Returns a dictionary object of key value pairs that represent the query string arguments that were passed to the page.
```js
Xrm.Page.context.getQueryStringParameters();
```
 
# Get Time Zone Offset Minutes (7.1+)
>  XrmContextGetTimeZoneOffsetMinutes
7.1+ Returns the difference between the local time and Coordinated Universal Time (UTC).
```js
Xrm.Page.context.getTimeZoneOffsetMinutes();
```
 
# Get User Id (5.0+)
>  XrmContextGetUserId
5.0+ Returns the GUID of the SystemUser.Id value for the current user.
```js
Xrm.Page.context.getUserId();
```
 
# Get User Lcid (5.0+)
>  XrmContextGetUserLcid
5.0+ Returns the LCID value that represents the language pack that is the user selected as their preferred language.
```js
Xrm.Page.context.getUserLcid();
```
 
# Get User Name (6.0+)
>  XrmContextGetUserName
6.0+ Returns the name of the current user.
```js
Xrm.Page.context.getUserName();
```
 
# Get User Roles (5.0+)
>  XrmContextGetUserRoles
5.0+ Returns an array of strings that represent the GUID values of each of the security roles that the user is associated with and any teams that the user is associated with.
```js
Xrm.Page.context.getUserRoles();
```
 
# Is Outlook Client (5.0-6.1)
>  XrmContextIsOutlookClient
5.0-6.1 Returns a Boolean value indicating if the user is using CRM for Outlook.
```js
Xrm.Page.context.isOutlookClient();
```
 
# Is Outlook Online (5.0-6.1)
>  XrmContextIsOutlookOnline
5.0-6.1 Returns a Boolean value that indicates whether the user is connected to the server while using CRM for Outlook with Offline Access. When this function returns false, the user is working offline without a connection to the server.
```js
Xrm.Page.context.isOutlookOnline();
```
 
# Prepend OrgName (5.0+)
>  XrmContextPrependOrgName
5.0+ Prepends the organization name to the specified path.
```js
Xrm.Page.context.prependOrgName("${path}");
```
 
# Add Custom Filter (6.0+)
>  XrmControlsAddCustomFilter
6.0+ Use to add filters to the results displayed in the lookup. Each filter will be combined with any previously added filters as an 'AND' condition.
```js
var entityName = "account";
var filter = "<filter type='and'><condition attribute='name' operator='eq' value='test' /></filter>";
Xrm.Page.getControl("${fieldname}").addCustomFilter(filter, entityName);
```
 
# Add Custom View (5.0+)
>  XrmControlsAddCustomView
5.0+ Adds a new view for the lookup dialog box.
```js
var viewId = "F0EE06F5-BB78-465F-BADA-FC3F5CF05300";
var entityName = "account";
var viewDisplayName = "Custom View";
var fetchXml = "<fetch version='1.0' output-format='xml-platform' mapping='logical' distinct='false'>" +
				"<entity name='account'>" +
				"<attribute name='name' />" +
				"<attribute name='accountid' />" +
				"<filter type='and'>" +
				"<condition attribute='name' operator='eq' value='test' />" +
				"</filter>" +
				"</entity>" +
				"</fetch>";

var layoutXml = "<grid name='resultset' object='1' jump='accountid' select='1' icon='1' preview='1'>" +
				"<row name='result' id='accountid'>" +
				"<cell name='name' width='150' />" +
				"</row>" +
				"</grid>";

var isDefault = true;
Xrm.Page.getControl("${fieldname}").addCustomView(viewId, entityName, viewDisplayName, fetchXml, layoutXml, isDefault);
```
 
# Add OnKeyPress (8.0+)
>  XrmControlsAddOnKeyPress
8.0+ Use this to add a function as an event handler for the keypress event so that the function is called when you type a character in the specific text or number field.
```js
Xrm.Page.getControl("${controlname}").addOnKeyPress(${functionName});
```
 
# Add Option (5.0+)
>  XrmControlsAddOption
5.0+ Adds an option to an option set control.
```js
Xrm.Page.getControl("${fieldname}").addOption(${option}, ${[index]});
```
 
# Add PreSearch (6.0+)
>  XrmControlsAddPreSearch
6.0+ Use this method to apply changes to lookups based on values current just as the user is about to view results for the lookup.
```js
Xrm.Page.getControl("${fieldname}").addPreSearch(${functionName});
```
 
# Clear Notification (6.0+)
>  XrmControlsClearNotification
6.0+ Remove a message already displayed for a control.
```js
Xrm.Page.getControl("${controlname}").clearNotification("${uniqueId}");
```
 
# Clear Options (5.0+)
>  XrmControlsClearOptions
5.0+ Clears all options from an option set control.
```js
Xrm.Page.getControl("${fieldname}").clearOptions();
```
 
# Fire OnKeyPress (8.0+)
>  XrmControlsFireOnKeyPress
8.0+ Use this to manually fire an event handler that you created for a specific text or number field to be executed on the keypress event.
```js
Xrm.Page.getControl("${controlname}").fireOnKeyPress();
```
 
# Get Attribute (5.0+)
>  XrmControlsGetAttribute
5.0+ Returns the attribute that the control is bound to.
```js
Xrm.Page.getControl("${controlname}").getAttribute();
```
 
# Control Type (5.0+)
>  XrmControlsGetControlType
5.0+ Returns a value that categorizes controls.
```js
Xrm.Page.getControl("${controlname}").getControlType();
```
 
# Get Data (5.0+)
>  XrmControlsGetData
5.0+ Returns the value of the data query string parameter passed to a Silverlight web resource.
```js
Xrm.Page.getControl("${controlname}").getData();
```
 
# Get Default View (5.0+)
>  XrmControlsGetDefaultView
5.0+ Returns the Id value of the default lookup dialog view.
```js
Xrm.Page.getControl("${fieldname}").getDefaultView();
```
 
# Get Disabled (5.0+)
>  XrmControlsGetDisabled
5.0+ Returns whether the control is disabled.
```js
Xrm.Page.getControl("${controlname}").getDisabled();
```
 
# Get Initial Url (5.0+)
>  XrmControlsGetInitialUrl
5.0+ Returns the default URL that an IFRAME control is configured to display. This method is not available for web resources.
```js
Xrm.Page.getControl("${controlname}").getInitialUrl();
```
 
# Get Label (5.0+)
>  XrmControlsGetLabel
5.0+ Returns the label for the control.
```js
Xrm.Page.getControl("${controlname}").getLabel();
```
 
# Get Name (5.0+)
>  XrmControlsGetName
5.0+ Returns the name assigned to the control.
```js
Xrm.Page.getControl("${controlname}").getName();
```
 
# Get Object (5.0+)
>  XrmControlsGetObject
5.0+ Returns the object in the form that represents an IFRAME or web resource.
```js
Xrm.Page.getControl("${controlname}").getObject();
```
 
# Get Parent (5.0+)
>  XrmControlsGetParent
5.0+ Returns a reference to the section object that contains the control.
```js
Xrm.Page.getControl("${controlname}").getParent();
```
 
# Get Show Time (7.1+)
>  XrmControlsGetShowTime
7.1+ Get whether a date control shows the time portion of the date.
```js
Xrm.Page.getControl("${fieldname}").getShowTime();
```
 
# Get Src (5.0+)
>  XrmControlsGetSrc
5.0+ Returns the current URL being displayed in an IFRAME or web resource.
```js
Xrm.Page.getControl("${controlname}").getSrc();
```
 
# Get Visible (5.0+)
>  XrmControlsGetVisible
5.0+ Returns a value that indicates whether the control is currently visible.
```js
Xrm.Page.getControl("${controlname}").getVisible();
```
 
# Hide AutoComplete (8.0+)
>  XrmControlsHideAutoComplete
8.0+ Use this function to hide the auto-completion drop-down list you configured for a specific text field.
```js
Xrm.Page.getControl("${controlname}").hideAutoComplete();
```
 
# Remove OnKeyPress (8.0+)
>  XrmControlsRemoveOnKeyPress
8.0+ Use this to remove an event handler for a text or number field that you added using addOnKeyPress.
```js
Xrm.Page.getControl("${controlname}").removeOnKeyPress(${functionName});
```
 
# Remove Option (5.0+)
>  XrmControlsRemoveOption
5.0+ Removes an option from an option set control.
```js
Xrm.Page.getControl("${fieldname}").removeOption(${number});
```
 
# Remove PreSearch (6.0+)
>  XrmControlsRemovePreSearch
6.0+ Use this method to remove event handler functions that have previously been set for the PreSearch event.
```js
Xrm.Page.getControl("${fieldname}").removePreSearch(${functionName});
```
 
# Set Data (5.0+)
>  XrmControlsSetData
5.0+ Sets the value of the data query string parameter passed to a Silverlight web resource.
```js
Xrm.Page.getControl("${controlname}").setData(value);
```
 
# Set Default View (5.0+)
>  XrmControlsSetDefaultView
5.0+ Sets the default view for the lookup control dialog.
```js
Xrm.Page.getControl("${fieldname}").setDefaultView("${viewId}");
```
 
# Set Disabled (5.0+)
>  XrmControlsSetDisabled
5.0+ Sets whether the control is disabled.
```js
Xrm.Page.getControl("${controlname}").setDisabled(${true|false});
```
 
# Set Focus (5.0+)
>  XrmControlsSetFocus
5.0+ Sets the focus on the control.
```js
Xrm.Page.getControl("${controlname}").setFocus();
```
 
##Set Label (5.0+)
>  XrmControlsSetLabel
5.0+ Sets the label for the control.
```js
Xrm.Page.getControl("${fieldname}").setLabel("${value}");
```
 
# Set Notification (6.0+)
>  XrmControlsSetNotification
6.0+ Display a message near the control to indicate that data isn’t valid.
```js
Xrm.Page.getControl("${controlname}").setNotification("${message}, "${uniqueId}");
```
 
# Set Show Time (6.0+)
>  XrmControlsSetShowTime
6.0+ Specify whether a date control should show the time portion of the date.
```js
Xrm.Page.getControl("${fieldname}").setShowTime(${true|false});
```
 
# Set Src (5.0+)
>  XrmControlsSetSrc
5.0+ Sets the URL to be displayed in an IFRAME or web resource.
```js
Xrm.Page.getControl("${controlname}").setSrc("${value}");
```
 
# Set Visible (5.0+)
>  XrmControlsSetVisible
5.0+ Sets a value that indicates whether the control is visible.
```js
Xrm.Page.getControl("${controlname}").setVisible(${true|false});
```
 
# Show AutoComplete (8.0+)
>  XrmControlsShowAutoComplete
8.0+ Use this to show up to 10 matching strings in a drop-down list as users press keys to type character in a specific text field. You can also add a custom command with an icon at the bottom of the drop-down list.
```js
Xrm.Page.getControl("${controlname}").showAutoComplete(${object});
```
 
# Add OnSave (5.0+)
>  XrmEntityAddOnSave
5.0+ Sets a function to be called when the record is saved.
```js
Xrm.Page.data.entity.addOnSave(${functionName});
```
 
# Get Data Xml (5.0+)
>  XrmEntityGetDataXml
5.0+ Returns a string representing the XML that will be sent to the server when the record is saved. Only data in fields that have changed are set to the server.
```js
Xrm.Page.data.entity.getDataXml();
```
 
# Get Entity Name (5.0+)
>  XrmEntityGetEntityName
5.0+ Returns a string representing the logical name of the entity for the record.
```js
Xrm.Page.data.entity.getEntityName();
```
 
# Get Id (5.0+)
>  XrmEntityGetId
5.0+ Returns a string representing the GUID id value for the record.
```js
Xrm.Page.data.entity.getId();
```
 
# Get IsDirty (5.0+)
>  XrmEntityGetIsDirty
5.0+ Returns a Boolean value that indicates if any fields in the form have been modified.
```js
Xrm.Page.data.entity.getIsDirty();
```
 
# Get Primary Attribute Value (6.0+)
>  XrmEntityGetPrimaryAttributeValue
6.0+ Gets a string for the value of the primary attribute of the entity.
```js
Xrm.Page.data.entity.getPrimaryAttributeValue();
```
 
# Remove OnSave (5.0+)
>  XrmEntityRemoveOnSave
5.0+ Removes a function to be called when the record is saved.
```js
Xrm.Page.data.entity.removeOnSave(${functionName});
```
 
# Save (5.0+)
>  XrmEntitySave
5.0+ Saves the record synchronously with the options to close the form or open a new form after the save is completed.
```js
Xrm.Page.data.entity.save(${null|"saveandclose"|"saveandnew"});
```
 
# Get Form Id (5.0+)
>  XrmFormSelectorItemsGetId
5.0+ Returns the GUID ID of the form.
```js
Xrm.Page.ui.formSelector.items.get("${formindex}").getId();
```
 
# Get Form Label (5.0+)
>  XrmFormSelectorItemsGetLabel
5.0+ Returns the label of the form.
```js
Xrm.Page.ui.formSelector.items.get("${formindex}").getLabel();
```
 
# Navigate (5.0+)
>  XrmFormSelectorItemsNavigate
5.0+ Opens the specified form.
```js
Xrm.Page.ui.formSelector.items.get("${formindex}").navigate();
```
 
# Add OnPostSearch (8.1+)
>  XrmControlsAddOnPostSearch
8.1+ Use this method to add an event handler to the PostSearch event.
```js
Xrm.Page.getControl("${controlname}").addOnPostSearch(${functionName});
```
 
# Add OnResultOpened (7.1+)
>  XrmControlsAddOnResultOpened
7.1+ Use this method to add an event handler to the OnResultOpened event.
```js
Xrm.Page.getControl("${controlname}").addOnResultOpened(${functionName});
```
 
# Add OnSelection (7.1+)
>  XrmControlsAddOnSelection
7.1+ Use this method to add an event handler to the OnSelection event.
```js
Xrm.Page.getControl("${controlname}").addOnSelection(${functionName});
```
 
# Get Search Query (7.1+)
>  XrmControlsGetSearchQuery
7.1+ Use this method to get the text used as the search criteria for the knowledge base management control.
```js
Xrm.Page.getControl("${controlname}").getSearchQuery();
```
 
# Get Selected Result (7.1+)
>  XrmControlsGetSelectedResult
7.1+ Use this method to get the currently selected result of the search control. The currently selected result also represents the result that is currently open.
```js
Xrm.Page.getControl("${controlname}").getSelectedResult();
```
 
# Get TotalResultCount (8.1+)
>  XrmControlsGetTotalResultCount
8.1+ Gets the count of results found in the search control.
```js
Xrm.Page.getControl("${controlname}").getTotalResultCount();
```
 
# Open Search Result (8.1+)
>  XrmControlsOpenSearchResult
8.1+ Opens a search result in the search control by specifying the result number.
```js
Xrm.Page.getControl("${controlname}").openSearchResult(${index}, "${Inline|Popout}");
```
 
# Remove OnPostSearch (8.1+)
>  XrmControlsRemoveOnPostSearch
8.1+ Use this method to remove an event handler from the PostSearch event.
```js
Xrm.Page.getControl("${controlname}").removeOnPostSearch(${functionName});
```
 
# Remove OnResultOpened (7.1+)
>  XrmControlsRemoveOnResultOpened
7.1+ Use this method to remove an event handler from the OnResultOpened event.
```js
Xrm.Page.getControl("${controlname}").removeOnResultOpened(${functionName});
```
 
# Remove OnSelection (7.1+)
>  XrmControlsRemoveOnSelection
7.1+ Use this method to remove an event handler from the OnSelection event.
```js
Xrm.Page.getControl("${controlname}").removeOnSelection(${functionName});
```
 
# Set Search Query (7.1+)
>  XrmControlsSetSearchQuery
7.1+ Use this method to set the text used as the search criteria for the knowledge base management control.
```js
Xrm.Page.getControl("${controlname}").setSearchQuery("${value}");
```
 
# Get Navigation Id (5.0+)
>  XrmNavigationItemsGetId
5.0+ Returns the name of the item.
```js
Xrm.Page.ui.navigation.items.get(${navigationindex|"navigationname"}).getId();
```
 
# Get Navigation Label (5.0+)
>  XrmNavigationItemsGetLabel
5.0+ Returns the label for the item.
```js
Xrm.Page.ui.navigation.items.get(${navigationindex|"navigationname"}).getLabel();
```
 
# Get Navigation Visible (5.0+)
>  XrmNavigationItemsGetVisible
5.0+ Returns a value that indicates whether the item is currently visible.
```js
Xrm.Page.ui.navigation.items.get(${navigationindex|"navigationname"}).getVisible();
```
 
# Set Navigation Focu (5.0+)
>  XrmNavigationItemsSetFocus
5.0+ Sets the focus on the item.
```js
Xrm.Page.ui.navigation.items.get(${navigationindex|"navigationname"}).setFocus();
```
 
# Set Navigation Label (5.0+)
>  XrmNavigationItemsSetLabel
5.0+ Sets the label for the item.
```js
Xrm.Page.ui.navigation.items.get(${navigationindex|"navigationname"}).setLabel("${value}");
```
 
# Set Navigation Visible (5.0+)
>  XrmNavigationItemsSetVisible
5.0+ Sets a value that indicates whether the item is visible.
```js
Xrm.Page.ui.navigation.items.get(${navigationindex|"navigationname"}).setVisible(${true|false});
```
 
# Get Process Id (7.0+)
>  XrmProcessGetId
7.0+ Returns the unique identifier of the process.
```js
Xrm.Page.data.process.getActiveProcess().getId();
```
 
# Get Process Name (7.0+)
>  XrmProcessGetName
7.0+ Returns the name of the process.
```js
Xrm.Page.data.process.getActiveProcess().getName();
```
 
# Get Sections (7.0+)
>  XrmProcessGetStages
7.0+ Returns an collection of stages in the process.
```js
Xrm.Page.data.process.getActiveProcess().getStages();
```
 
# Is Rendered(7.0+)
>  XrmProcessIsRendered
7.0+ Returns true if the process is rendered, false if not.
```js
Xrm.Page.data.process.getActiveProcess().isRendered();
```
 
# Get Save Mode (5.0+)
>  XrmExecObjGetSaveMode
5.0+ Returns a value indicating how the save event was initiated by the user.
```js
	execObj.getEventArgs().getSaveMode();
```
 
# Is Default Prevented (5.0+)
>  XrmExecObjIsDefaultPrevented
5.0+ Returns a value indicating whether the save event has been canceled because the preventDefault method was used in this event handler or a previous event handler.
```js
    execObj.getEventArgs().isDefaultPrevented();
```
 
# Prevent Default (5.0+)
>  XrmExecObjPreventDefault
5.0+ Cancels the save operation, but all remaining handlers for the event will still be executed.
```js
    execObj.getEventArgs().preventDefault();
```
 
# Get Context (5.0+)
>  XrmExecObjGetContext
5.0+ Returns the Xrm.Page.context object.
```js
	execObj.getContext();
```
 
# Get Depth (5.0+)
>  XrmExecObjGetDepth
5.0+ Returns a value that indicates the order in which this handler is executed.
```js
	execObj.getDepth();
```
 
# Get Event Source (5.0+)
>  XrmExecObjGetEventSource
5.0+ Returns a reference to the object that the event occurred on.
```js
	execObj.getEventSource();
```
 
# Get Shared Variable (5.0+)
>  XrmExecObjGetSharedVariable
5.0+ Retrieves a variable set using setSharedVariable.
```js
	execObj.getSharedVariable("${key}");
```
 
# Set Shared Variable (5.0+)
>  XrmExecObjSetSharedVariable
5.0+ Sets the value of a variable to be used by a handler after the current handler completes.
```js
	execObj.setSharedVariable("${key}\ ${value});
```
 
# Get Label (5.0+)
>  XrmTabsSectionsGetLabel
5.0+ Returns the label for the section.
```js
Xrm.Page.ui.tabs.get(${null|"tabname"|tabindex}).sections.get(${null|"sectionname"|sectionindex}).getLabel();
```
 
# Get Name (5.0+)
>  XrmTabsSectionsGetName
5.0+ Method to return the name of the section.
```js
Xrm.Page.ui.tabs.get(${null|"tabname"|tabindex}).sections.get(${null|"sectionname"|sectionindex}).getName();
```
 
# Get Parent (5.0+)
>  XrmTabsSectionsGetParent
5.0+ Method to return the tab containing the section.
```js
Xrm.Page.ui.tabs.get(${null|"tabname"|tabindex}).sections.get(${null|"sectionname"|sectionindex}).getParent();
```
 
# Get Visible (5.0+)
>  XrmTabsSectionsGetVisible
5.0+ Returns true if the section is visible, otherwise returns false.
```js
Xrm.Page.ui.tabs.get(${null|"tabname"|tabindex}).sections.get(${null|"sectionname"|sectionindex}).getVisible();
```
 
# Set Label (5.0+)
>  XrmTabsSectionsSetLabel
5.0+ Sets the label for the section.
```js
Xrm.Page.ui.tabs.get(${null|"tabname"|tabindex}).sections.get(${null|"sectionname"|sectionindex}).setLabel("${value}");
```
 
# Set Visible (5.0+)
>  XrmTabsSectionsSetVisible
5.0+ Sets a value to show or hide the section.
```js
Xrm.Page.ui.tabs.get(${null|"tabname"|tabindex}).sections.get(${null|"sectionname"|sectionindex}).setVisible(${true|false});
```
 
# Get Category (7.0+)
>  XrmProcessGetStagesGetCategoryGetValue
7.0+ Returns an object with a getValue method which will return the integer value of the business process flow category.
```js
Xrm.Page.data.process.getActiveProcess().getStages().get(${stageindex}).getCategory().getValue();
```
 
# Get Entity Name (7.0+)
>  XrmProcessGetStagesGetCategoryGetValueGetEntityName
7.0+ Returns the logical name of the entity associated with the stage.
```js
Xrm.Page.data.process.getActiveProcess().getStages().get(${stageindex}).getEntityName();
```
 
# Get Id (7.0+)
>  XrmProcessGetStagesGetId
7.0+ Returns the unique identifier of the stage.
```js
Xrm.Page.data.process.getActiveProcess().getStages().get(${stageindex}).getId();
```
 
# Get Name (7.0+)
>  XrmProcessGetStagesGetName
7.0+ Returns the name of the stage.
```js
Xrm.Page.data.process.getActiveProcess().getStages().get(${stageindex}).getName();
```
 
# Get Name (7.0+)
>  XrmProcessGetStagesGetStatus
7.0+ Returns the status of the stage.
```js
Xrm.Page.data.process.getActiveProcess().getStages().get(${stageindex}).getStatus();
```
 
# Get Name (7.0+)
>  XrmProcessGetStagesGetSteps
7.0+ Returns a collection of steps in the stage.
```js
Xrm.Page.data.process.getActiveProcess().getStages().get(${stageindex}).getSteps();
```
 
# Get Attribute (7.0+)
>  XrmProcessGetStagesGetStepsGetAttribute
7.0+ Returns the logical name of the attribute associated to the step.
```js
Xrm.Page.data.process.getActiveProcess().getStages().get(${stageindex}).getSteps().get(${stepindex}).getAttribute();
```
 
# Get Name (7.0+)
>  XrmProcessGetStagesGetStepsGetAttributeGetName
7.0+ Returns the name of the step.
```js
Xrm.Page.data.process.getActiveProcess().getStages().get(${stageindex}).getSteps().get(${stepindex}).getName();
```
 
# IsRequired (7.0+)
>  XrmProcessGetStagesGetStepsGetAttributeIsRequired
7.0+ Returns whether the step is required in the business process flow.
```js
Xrm.Page.data.process.getActiveProcess().getStages().get(${stageindex}).getSteps().get(${stepindex}).isRequired();
```
 
# Add Grid OnLoad (7.1+)
>  XrmControlsAddOnLoad
7.1+ Use this method to add event handlers to the GridControlOnLoad event.
```js
Xrm.Page.getControl("${controlname}").addOnLoad(${functionName});
```
 
# Get Grid (7.1+)
>  XrmControlsGetGrid
7.1+ Use this method to get access to the Grid available in the GridControl.
```js
Xrm.Page.getControl("${controlname}").getGrid();
```
 
# Get Grid Control (7.1+)
>  XrmControlsGetControl
7.1+ Returns the control.
```js
Xrm.Page.getControl("${controlname}");
```
 
# Get Grid Current View (7.1+)
>  XrmControlsGetViewSelectorGetCurrentView
7.1+ Use this method to get a reference to the current view.
```js
Xrm.Page.getControl("${controlname}").getViewSelector().getCurrentView();
```
 
# Get Grid Entity Name (7.1+)
>  XrmControlsGetEntityName
7.1+ Use this method to get the logical name of the entity data displayed in the grid.
```js
Xrm.Page.getControl("${controlname}").getEntityName();
```
 
# Get Row Data (7.1+)
>  XrmGridGetRowsGetData
7.1+ Returns the GridRowData for the GridRow.
```js
Xrm.Page.getControl("${controlname}").getGrid().getRows().get(${rowindex}).getData();
```
 
# Get Row Entity (7.1+)
>  XrmGridGetRowsGetEntity
7.1+ Returns the GridEntity for the GridRowData.
```js
Xrm.Page.getControl("${controlname}").getGrid().getRows().get(${rowindex}).getData().getEntity();
```
 
# Get Row Entity Id (7.1+)
>  XrmGridGetRowsGetEntityGetEntityGetId
7.1+ Returns the id for the record in the row.
```js
Xrm.Page.getControl("${controlname}").getGrid().getRows().get(${rowindex}).getData().getEntity().getId();
```
 
# Get Row Entity Name (7.1+)
>  XrmGridGetRowsGetEntityGetEntityName
7.1+ Returns the logical name for the record in the row.
```js
Xrm.Page.getControl("${controlname}").getGrid().getRows().get(${rowindex}).getData().getEntity().getEntityName();
```
 
# Get Row Entity Primary Attribute Value (7.1+)
>  XrmGridGetRowsGetEntityGetPrimaryAttributeValue
7.1+ Returns the primary attribute value for the record in the row.
```js
Xrm.Page.getControl("${controlname}").getGrid().getRows().get(${rowindex}).getData().getEntity().getPrimaryAttributeValue();
```
 
# Get Row Entity Reference (7.1+)
>  XrmGridGetRowsGetEntityGetEntityReference
7.1+ Returns the entity reference for the record in the row.
```js
Xrm.Page.getControl("${controlname}").getGrid().getRows().get(${rowindex}).getData().getEntity().getEntityReference();
```
 
# Get Rows (7.1+)
>  XrmGridGetRows
7.1+ Returns a collection of every GridRow in the Grid.
```js
Xrm.Page.getControl("${controlname}").getGrid().getRows();
```
 
# Get Selected Rows (7.1+)
>  XrmGridGetSelectedRows
7.1+ Returns a collection of every selected GridRow in the Grid.
```js
Xrm.Page.getControl("${controlname}").getGrid().getSelectedRows();
```
 
# Get Total Record Count (7.1+)
>  XrmGridGetTotalRecordCount
7.1+ Returns the total number of records that match the filter criteria of the view, not limited by the number visible in a single page.
```js
Xrm.Page.getControl("${controlname}").getGrid().getTotalRecordCount();
```
 
# Get View Selector (7.1+)
>  XrmControlsGetViewSelector
7.1+ Use this method to get access to the ViewSelector available for the GridControl.
```js
Xrm.Page.getControl("${controlname}").getViewSelector();
```
 
# Is View Selector Visible (7.1+)
>  XrmControlsGetViewSelectorIsVisible
7.1+ Use this method to determine whether the view selector is visible.
```js
Xrm.Page.getControl("${controlname}").getViewSelector().isVisible();
```
 
# Refresh (5.0+)
>  XrmControlsRefresh
5.0+ Refreshes the data displayed in a subgrid.
```js
Xrm.Page.getControl("${controlname}").refresh();
```
 
# Remove Grid OnLoad (7.1+)
>  XrmControlsRemoveOnLoad
7.1+ Use this method to remove event handlers from the GridControlOnLoad event.
```js
Xrm.Page.getControl("${controlname}").removeOnLoad(${functionName});
```
 
# Set Grid Current View (7.1+)
>  XrmControlsGetViewSelectorSetCurrentView
7.1+ Use this method to set the current view.
```js
"var contactSavedQuery = {
	entityType: 1039, // SavedQuery
	id: "3A282DA1-5D90-E011-95AE-00155D9CFA02"
	name: "Contacts Saved Query"
	}
	
Xrm.Page.getControl("${controlname}").getViewSelector().setCurrentView(contactSavedQuery);
```
 
# Get Display State (5.0+)
>  XrmTabsGetDisplayState
5.0+ Returns a value that indicates whether the tab is collapsed or expanded.
```js
Xrm.Page.ui.tabs.get(${null|"tabname"|tabindex}).getDisplayState();
```
 
# Get Tab Label (5.0+)
>  XrmTabsGetLabel
5.0+ Returns the tab label.
```js
Xrm.Page.ui.tabs.get(${null|"tabname"|tabindex}).getLabel();
```
 
# Get Tab Name (5.0+)
>  XrmTabsGetName
5.0+ Returns the name of the tab.
```js
Xrm.Page.ui.tabs.get(${null|"tabname"|tabindex}).getName();
```
 
# Get Tab Parent (5.0+)
>  XrmTabsGetParent
5.0+ Returns the Xrm.Page.ui (client-side reference) object.
```js
Xrm.Page.ui.tabs.get(${null|"tabname"|tabindex}).getParent();
```
 
# Get Tab Visible (5.0+)
>  XrmTabsGetVisible
5.0+ Returns a value that indicates whether the tab is visible.
```js
Xrm.Page.ui.tabs.get(${null|"tabname"|tabindex}).getVisible();
```
 
# Set Tab Display State (5.0+)
>  XrmTabsSetDisplayState
5.0+ Sets the tab to be collapsed or expanded.
```js
Xrm.Page.ui.tabs.get(${null|"tabname"|tabindex}).setDisplayState("${expanded|collapsed}");
```
 
# Set Tab Focus (5.0+)
>  XrmTabsSetFocus
5.0+ Sets the focus on the tab.
```js
Xrm.Page.ui.tabs.get(${null|"tabname"|tabindex}).setFocus();
```
 
# Set Tab Label (5.0+)
>  XrmTabsSetLabel
5.0+ Sets the label for the tab.
```js
Xrm.Page.ui.tabs.get(${null|"tabname"|tabindex}).setLabel("${value}");
```
 
# Set Tab Visible (5.0+)
>  XrmTabsSetVisible
5.0+ Sets a value that indicates whether the control is visible.
```js
Xrm.Page.ui.tabs.get(${null|"tabname"|tabindex}).setVisible(${true|false});
```
 
# Clear Form Notification (6.0+)
>  XrmUiClearFormNotification
6.0+ Use this method to remove form level notifications.
```js
Xrm.Page.ui.clearFormNotification("${uniqueId}");
```
 
# Get Current Control (5.0+)
>  XrmUiGetCurrentControl
5.0+ Method to get the control object that currently has focus on the form. Web Resource and IFRAME controls are not returned by this method.
```js
Xrm.Page.ui.getCurrentControl();
```
 
# Close (5.0+)
>  XrmUiClose
5.0+ Method to close the form.
```js
Xrm.Page.ui.close();
```
 
# Get Form Type (5.0+)
>  XrmUiGetFormType
5.0+ Method to get the form context for the record.
```js
Xrm.Page.ui.getFormType();
```
 
# Get ViewPort Width(5.0+)
>  XrmUiGetViewPortWidth
5.0+ Method to get the width of the viewport in pixels.
```js
Xrm.Page.ui.getViewPortWidth();
```
 
# Get ViewPort Height (5.0+)
>  XrmUiGetViewPortHeight
5.0+ Method to get the height of the viewport in pixels.
```js
Xrm.Page.ui.getViewPortHeight();
```
 
# Refresh Ribbon (5.0+)
>  XrmUiRefreshRibbon
5.0+ Method to cause the ribbon to re-evaluate data that controls what is displayed in it.
```js
Xrm.Page.ui.refreshRibbon();
```
 
# Set Form Notification (6.0+)
>  XrmUiSetFormNotification
6.0+ Use this method to display form level notifications.
```js
Xrm.Page.ui.setFormNotification("${message}, "${ERROR|WARNING|INFO}, "${uniqueId}");
```
 
# Alert Dialog (6.0+)
>  XrmUtilityAlertDialog
6.0+ Displays a dialog box containing an application-defined message.
```js
Xrm.Utility.alertDialog("${message}\ ${functionName});
```
 
# Confirm Dialog (6.0+)
>  XrmUtilityConfirmDialog
6.0+ Displays a confirmation dialog box that contains an optional message as well as OK and Cancel buttons.
```js
Xrm.Utility.confirmDialog("${message}, ${[yesfunctionName]}, ${[nofunctionName]});
```
 
# Is Activity Type (6.0+)
>  XrmUtilityIsActivityType
6.0+ Determine if an entity is an activity entity.
```js
Xrm.Utility.isActivityType("${entityName}");
```
 
# Open Entity Form (5.0+)
>  XrmUtilityOpenEntityForm
5.0+ Opens an entity form.
```js
var name = "account";
var id = "5D24B2A7-957A-4D08-8723-2CEF5219FFA0";
var parameters = {};
parameters["formid"] = "b053a39a-041a-4356-acef-ddf00182762b";
parameters["name"] = "Test";

var windowOptions = {
		openInNewWindow: true
	};

Xrm.Utility.openEntityForm(name, id, parameters, windowOptions);
```
 
# Open Quick Create (7.1+)
>  XrmUtilityOpenQuickCreate
7.1+ Opens a new quick create form.
```js
var entityName = "account";
var createFromEntity = { entityType: "account\ id: Xrm.Page.data.entity.getId() };
var parameters = {};
parameters["name"] = "Test";

Xrm.Utility.openQuickCreate(entityName, createFromEntity, parameters).then(${[successFunction]}, ${[errorFunction]});
```
 
# Open Web Resource (5.0+)
>  XrmUtilityOpenWebResource
5.0+ Opens an HTML web resource.
```js
var webResourceName = "new_webResource.htm";
var webResourceData = encodeURIComponent("first=First Value&second=Second Value");
var parameters = {};
var width = 300;
var height = 300;

Xrm.Utility.openWebResource(webResourceName, webResourceData, width, height);
```
 
# Is Offline Enabled (8.2+)
>  XrmMobileIsOfflineEnabled
8.2+ Returns whether an entity is offline enabled.
```js
Xrm.Mobile.offline.isOfflineEnabled("${entityName}");
```
 
# Create Offline Record (8.2+)
>  XrmMobileCreateOfflineRecord
8.2+ Creates an entity record in Dynamics 365 mobile clients while working in the offline mode.
```js
	var entity = {};
	entity.name = "test";
                
Xrm.Mobile.offline.createRecord("${entityName}\ entity).then(${[successFunction]}, ${[errorFunction]});
```
 
# Retrieve Offline Record (8.2+)
>  XrmMobileRetrieveOfflineRecord
8.2+ Retrieves an entity record in Dynamics 365 mobile clients while working in the offline mode.
```js
	var options = "?$select=name&$expand=primarycontactid($select=contactid,fullname);
 
Xrm.Mobile.offline.retrieveRecord("${entityName}\ "${id}\ options).then(${[successFunction]}, ${[errorFunction]});
```
 
# Retrieve Multiple Offline Records (8.2+)
>  XrmMobileRetrieveMultipleOfflineRecords
8.2+ Retrieves a collection of entity records in Dynamics 365 mobile clients while working in the offline mode.
```js
	var options = "?$select=name&$expand=primarycontactid($select=contactid,fullname);
	var maxPageSize = 100;
                
Xrm.Mobile.offline.retrieveMultipleRecords("${entityName}\ options, maxPageSize).then(${[successFunction]}, ${[errorFunction]});
```
 
# Update Offline Record (8.2+)
>  XrmMobileUpdateOfflineRecord
8.2+ Updates an entity record in Dynamics 365 mobile clients while working in the offline mode.
```js
	var entity = {};
	entity.name = "test";
                
Xrm.Mobile.offline.updateRecord("${entityName}\ "${id}\ entity).then(${[successFunction]}, ${[errorFunction]});
```
 
# Delete Offline Record (8.2+)
>  XrmMobileDeleteOfflineRecord
8.2+ Deletes an entity record in Dynamics 365 mobile clients while working in the offline mode.
```js
Xrm.Mobile.offline.deleteRecord("${entityName}\ "${id}").then(${[successFunction]}, ${[errorFunction]});
```
 
# Add Cell Notification (8.2+)
>  XrmControlsAddNotification
8.2+ Displays an error or recommendation notification for a control, and lets you specify actions to execute based on the notification.
```js
var actionCollection = {
		message: "${message},
		actions: null
	};

actionCollection.actions = [function () {
	${functionbody}
}];

Xrm.Page.getControl("${fieldname}").addNotification({
		messages: ["${message}"],
		notificationLevel: "${RECOMMENDATION|ERROR}",
		uniqueId: "${uniqueId}",
		actions: [actionCollection]
	});
```
 
# Get Barcode Value (8.2+)
>  XrmUtilityGetBarcodeValue
8.2+ Returns the barcode information, such as a product number, scanned using the device camera.
```js
Xrm.Utility.getBarcodeValue().then(${[successFunction]}, ${[errorFunction]});
```
 
# Get Current Position (8.2+)
>  XrmUtilityGetCurrentPosition
8.2+ Returns the current location using the device geolocation capability.
```js
Xrm.Utility.getCurrentPosition().then(${[successFunction]}, ${[errorFunction]});
```
 
# Load Panel (8.2+)
>  XrmPanelLoadPanel
8.2+ Displays the web page represented by a URL in the static area in the side pane, which appears on all pages in the Dynamics 365 web client.
```js
Xrm.Panel.LoadPanel("${url}\ "${title}");
```
 
# Get Form Context (8.2+)
>  XrmExecObjGetFormContext
8.2+ Method that returns a reference to either the form (Xrm.Page) or editable grid depending on where the method was called.
```js
    execObj.getFormContext();
```
 
# Get Process Instances (8.2+)
>  XrmProcessGetProcessInstances
8.2+ Returns all the process instances for the entity record that the calling user has access to.
```js
Xrm.Page.data.process.getProcessInstances(${callbackfunctionName}(${object}));
```
 
# Set Active Process Instance (8.2+)
>  XrmProcessSetActiveProcessInstance
8.2+ Sets a process instance as the active instance.
```js
Xrm.Page.data.process.setActiveProcessInstance("${processInstanceId}\ ${callbackfunctionName});
```
 
# Add OnProcessStatusChange (8.2+)
>  XrmProcessAddOnProcessStatusChange
8.2+ Use this to add a function as an event handler for the OnProcessStatusChange event event so that it will be called when the business process flow status changes.
```js
Xrm.Page.data.process.addOnProcessStatusChange(${functionName});
```
 
# Remove OnProcessStatusChange (8.2+)
>  XrmProcessRemoveOnProcessStatusChange
8.2+ Use this to remove a function as an event handler for the OnProcessStatusChange event event.
```js
Xrm.Page.data.process.removeOnProcessStatusChange(${functionName});
```
 
# Get Instance Id (8.2+)
>  XrmProcessGetInstanceId
8.2+ Returns the unique identifier of the process instance.
```js
Xrm.Page.data.process.getInstanceId();
```
 
# Get Instance Name (8.2+)
>  XrmProcessGetInstanceName
8.2+ Returns the name of the process instance.
```js
Xrm.Page.data.process.getInstanceName();
```
 
# Get Status (8.2+)
>  XrmProcessGetStatus
8.2+ Returns the current status of the process instance.
```js
Xrm.Page.data.process.getStatus();
```
 
# Set Status (8.2+)
>  XrmProcessSetStatus
8.2+ Sets the current status of the active process instance.
```js
Xrm.Page.data.process.setStatus("${status}\ ${callbackFunction});
```
 
# Get QuickForm Controls (8.1+)
>  XrmUiQuickFormControls
8.1+ A collection of all quick view controls on a form.
```js
Xrm.Page.ui.quickForms"
```
 
# ForEach QuickForm Controls (8.1+)
>  XrmUiQuickFormControlsForEach
8.1+ Apply an action in a delegate function to each object in the collection of quick view controls on a form.
```js
Xrm.Page.ui.quickForms.forEach(function (qvcontrol, index) {
	${functionbody}
});
```
 
# QuickForm Get Control (8.1+)
>  XrmUiQuickFormGetControl
8.1+ Gets the constituent controls in a quick view control.
```js
Xrm.Page.ui.quickForms.get("${qvcontrolname}").getControl("${controlname}");
```
 
# QuickForm Get Control Type (8.1+)
>  XrmUiQuickFormGetControlType
8.1+ Returns a string value that categorizes quick view controls.
```js
Xrm.Page.ui.quickForms.get("${qvcontrolname}").getControlType();
```
 
# QuickForm Get Name (8.1+)
>  XrmUiQuickFormGetName
8.1+ Returns the name assigned to the quick view control.
```js
Xrm.Page.ui.quickForms.get("${qvcontrolname}").getName();
```
 
# QuickForm Get Parent (8.1+)
>  XrmUiQuickFormGetParent
8.1+ Returns a reference to the section object that contains the control.
```js
Xrm.Page.ui.quickForms.get("${qvcontrolname}").getParent();
```
 
# QuickForm Get Visible (8.1+)
>  XrmUiQuickFormGetVisible
8.1+ Returns a value that indicates whether the quick view control is currently visible.
```js
Xrm.Page.ui.quickForms.get("${qvcontrolname}").getVisible();
```
 
# QuickForm Get Label (8.1+)
>  XrmUiQuickFormGetLabel
8.1+ Returns the label for the quick view control.
```js
Xrm.Page.ui.quickForms.get("${qvcontrolname}").getLabel();
```
 
# QuickForm Set Label (8.1+)
>  XrmUiQuickFormSetLabel
8.1+ Sets the label for the quick view control.
```js
Xrm.Page.ui.quickForms.get("${qvcontrolname}").setLabel("${value}");
```
 
# QuickForm Is Loaded (8.1+)
>  XrmUiQuickFormIsLoaded
8.1+ Returns whether the data binding for the constituent controls in a quick view control is complete.
```js
Xrm.Page.ui.quickForms.get("${qvcontrolname}").isLoaded();
```
 
# QuickForm Refresh (8.1+)
>  XrmUiQuickFormRefresh
8.1+ Refreshes the data displayed in a quick view control.
```js
Xrm.Page.ui.quickForms.get("${qvcontrolname}").refresh();
```
