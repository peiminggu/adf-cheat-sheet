## Get Application Module
```java
BindingContext bindingContext = BindingContext.getCurrent(); 
DCDataControl dc = bindingContext.findDataControl("XXXAMDataControl");
```
## Get View Object
```java
DCBindingContainer dc = (DCBindingContainer) BindingContext.getCurrent().getCurrentBindingsEntry();
DCIteratorBinding iterBind = dc.findIteratorBinding("XXXVO1Iterator");
ViewObject xxxVo = iterBind.getViewObject();
```
## Get Binding Operation 
```java
BindingContext bindingContext = BindingContext.getCurrent(); 
OperationBinding operation = bindingContextgetCurrentBindingsEntry().getOperationBinding("xxxPperation");
operation.getParamsMap().put("parameterName", parameterValue);
operation.execute();
```
