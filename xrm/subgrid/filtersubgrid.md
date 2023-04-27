# XRM:FilterSubgrid

This is a snippet to filter subgrid
```js
function modifyRibbon() {
    //to store the original function ones
    var originalFunctionAddNewStandard = Mscrm.GridRibbonActions.addNewFromSubGridStandard;
    var originalFunctionAddExistingStandard = Mscrm.GridRibbonActions.addExistingFromSubGridStandard;
    var originalFunctionAssociated = Mscrm.GridRibbonActions.addExistingFromSubGridAssociated;
 
    //add new standard subgrid
    Mscrm.GridRibbonActions.addNewFromSubGridStandard = function (gridTypeCode, parentEntityTypeCode, parentEntityId, primaryControl, gridControl) {
        if (gridControl.get_id() != "subgrid_id") {
            originalFunctionAddNewStandard(gridTypeCode, parentEntityTypeCode, parentEntityId, primaryControl, gridControl);
        }
        else {
            callPopUp(gridControl);
        }
    }
 
    //add existing standard subgrid
    Mscrm.GridRibbonActions.addExistingFromSubGridStandard = function (gridTypeCode, gridControl) {
        if (gridControl.get_id() != "subgrid_id") {
            originalFunctionAddExistingStandard(gridTypeCode, gridControl);
        }
        else {
            callPopUp(gridControl);
        }
    }
 
    //add associate subgrid (N:N)
    Mscrm.GridRibbonActions.addExistingFromSubGridAssociated = function (gridTypeCode, gridControl) {
        if (gridControl.get_id() != "subgrid_id") {
            originalFunctionAssociated(gridTypeCode, gridControl);
        }
        else {
            callPopUp(gridControl);
        }
    }
 
}
 
function callPopUp(gridcontrol) {
    if (Xrm.Page.ui.getFormType() == 2) //update {
        //call the pop up
        var dialogoptions = new Xrm.DialogOptions;
        dialogoptions.width = 1080;
        dialogoptions.height = 750;
//You can call pop up or modal dialog or web resource or other script, running dialog, running workflow, or javascript to call custom action
Xrm.Internal.openDialog('http://mycrmserver?id=' + Xrm.Page.data.entity.getId(),
       dialogoptions, null, null, function (retval) { gridcontrol.Refresh() });
        gridcontrol.Refresh();
    }
}
```
