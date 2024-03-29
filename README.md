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

## Redirect

### Redirect to another view
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

### Redirect to another view by task flow navigation
```java
public static void navigateTo(String navigationOutcome) {
     FacesContext context = FacesContext.getCurrentInstance();
     Application application = context.getApplication();
     application.getNavigationHandler().handleNavigation(context, null, navigationOutcome);
}
```

### Redirect to another page by FacesContext
```java
FacesContext ctx = FacesContext.getCurrentInstance();
try {
     ctx.getExternalContext().redirect("xxx.jspx");
} catch (IOException e) {
     
}
```

## View

### Refresh UI Component in Backing Bean

```java
UIComponent xxxComponent = ...

AdfFacesContext adfFacesContext = AdfFacesContext.getCurrentInstance();
adfFacesContext.addPartialTarget(xxxComponent);
```

### Reset UI Component cached value in Backing Bean

Set value to null doesn't work for String type attribute
```java
JSFUtils.setExpressionValue("#{bindings.xxxValue.inputValue}", "");

//or (codes below haven't been verified)
RichInputText input = (RichInputText)JSFUtils.findComponentInRoot(id); 
input.setSubmittedValue(null); 
input.resetValue(); 
AdfFacesContext.getCurrentInstance().addPartialTarget(input);

```

### Find Component from Root
```java
public static UIComponent findComponentFromRoot(UIComponent root, String targetComId) {
   if (targetComId.equals(root.getId()))
       return root;

   UIComponent child = null;
   UIComponent targetCom = null;
   Iterator children = root.getFacetsAndChildren();
   while (children.hasNext() && (targetCom == null)) {
       child = (UIComponent)children.next();
       if (targetComId.equals(child.getId())) {
           targetCom = child;
           break;
       }
       targetCom = findComponentFromRoot(child, targetComId);
       if (targetCom != null) {
           break;
       }
   }
   return targetCom;
}

```

## Groovy

### Get Database Sequence using Groovy
``` groovy
(new oracle.jbo.server.SequenceImpl("<SEQUENCE NAME>",adf.object.getDBTransaction())).getSequenceNumber()
```

## Javascript

### Find Component ID 
``` javascript
AdfPage.PAGE.findComponentByAbsoluteId()
```

### Trigger buttom event by javascript
``` javascript
function customHandler(targetBtnClientId){
   var targetCmd = AdfPage.PAGE.findComponentByAbsoluteId(targetBtnClientId);
   var actionEvent = new AdfActionEvent(targetCmd);
   actionEvent.forceFullSubmit();
   actionEvent.noResponseExpected();
   actionEvent.queue();
}
```

### Call Javascript from bean
```java
import org.apache.myfaces.trinidad.render.ExtendedRenderKitService;
import org.apache.myfaces.trinidad.util.Service;

public static void executeClientJavascript(String script) {
     FacesContext facesContext = FacesContext.getCurrentInstance();
     ExtendedRenderKitService service = Service.getRenderKitService(facesContext, ExtendedRenderKitService.class);
     service.addScript(facesContext, script);
}
```
### Call action on page load

In jspx page, add in af:source
```javascript
function invokeLoadEvnt() {
     if (window.AdfPage && window.AdfPage.PAGE && window.AdfPage.PAGE.isSynchronizedWithServer()) {
          var button = AdfPage.PAGE.findComponentByAbsoluteId('xxx');
          AdfActionEvent.queue(button,true);
     }
     else {
          //wait 100 ms
          window.setTimeout(invokeLoadEvnt, 100);
     }
}
```
In the jspx page under the af:document call the client listener
```html
<af:clientListener type="load" method="invokeLoadEvnt"/>
```

## Security

### Logout Programmatically
```java
import weblogic.servlet.security.ServletAuthentication;


HttpServletRequest request = (HttpServletRequest)ectx.getRequest();  
ServletAuthentication.logout(request);  
ServletAuthentication.invalidateAll(request);   
ServletAuthentication.killCookie(request);  
fctx.responseComplete();  

//clean session
ExternalContext ctx = FacesContext.getCurrentInstance().getExternalContext();
if (ctx.getSession(false) != null) {
  ((HttpSession)ctx.getSession(false)).invalidate();
}
```
