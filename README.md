# Oracle ADF Cheat Sheet

Oracle ADF code mostly used, easy to copy paste

## Bindings

### Get Application Module
```java
BindingContext bindingContext = BindingContext.getCurrent(); 
DCDataControl dc = bindingContext.findDataControl("XXXAMDataControl");
```
### Get View Object
```java
DCBindingContainer dc = (DCBindingContainer) BindingContext.getCurrent().getCurrentBindingsEntry();
DCIteratorBinding iterBind = dc.findIteratorBinding("XXXVO1Iterator");
ViewObject xxxVo = iterBind.getViewObject();
```
### Get/Execute Method by Binding  
```java
BindingContext bctx = BindingContext.getCurrent();
BindingContainer bindings = bctx.getCurrentBindingsEntry();
OperationBinding operationBinding = 
     bindings.getOperationBinding("xxxMethodName");
//set parameter (optional)
operationBinding.getParamsMap().put("xxxParamName1",value1);
operationBinding.getParamsMap().put("xxxParamName2",value2);
//invoke method
operationBinding.execute();
if (!operationBinding.getErrors().isEmpty()) {
   //check errors
   List errors = operationBinding.getErrors();
   ...
}
//return value (optional)
Object methodReturnValue = operationBinding.getResult();
```

### Get Attribute Value by Binding  
```java
BindingContext bctx = BindingContext.getCurrent();
BindingContainer bindings = bctx.getCurrentBindingsEntry();

AttributeBinding attr = (AttributeBinding)bindings.getControlBinding("xxxValue");
String xxxValue =(String)attr.getInputValue();

```

### Redirect to another view or page
```java
public static void redirectToView(String viewId) {
   FacesContext fCtx = FacesContext.getCurrentInstance();
   ExternalContext eCtx = fCtx.getExternalContext();

   String activityUrl = ControllerContext.getInstance().getGlobalViewActivityURL(viewId);
   try {
       eCtx.redirect(activityUrl);
   } catch (IOException e) {
       e.printStackTrace();
       JSFUtils.addFacesErrorMessage("Exception when redirect to " + viewId);
   }
}
```


## View

### Refresh UI Component in Backing Bean

```java
UIComponent xxxComponent = ...

AdfFacesContext adfFacesContext = AdfFacesContext.getCurrentInstance();
adfFacesContext.addPartialTarget(xxxComponent);
```
## Groovy

## Javascript

Component ID Lookup using 
``` javascript
AdfPage.PAGE.findComponentByAbsoluteId()
```
