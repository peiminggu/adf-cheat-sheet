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

## View

### Refresh UI Component

```java
UIComponent xxxComponent = ...

AdfFacesContext adfFacesContext = AdfFacesContext.getCurrentInstance();
adfFacesContext.addPartialTarget(xxxComponent);
}
```

