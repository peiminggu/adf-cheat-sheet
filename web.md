# Web, Servlet and JSP

## Read Environment Entries from web.xml

### Read property
```java
Context ctx = new InitialContext();
String value = (String) ctx.lookup("java:comp/env/VALUE_KEY");
```

### Read all properties

```java
Context ctx = new InitialContext();

for (Enumeration<Binding> e = ctx.listBindings("java:comp/env"); e.hasMoreElements();) {
  Binding bind = e.nextElement();
  String key = bind.getName();
  String value = (String)bind.getObject());
}
```
