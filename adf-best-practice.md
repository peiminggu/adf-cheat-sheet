# Oracle ADF Best Practices

## Skin

### Testing Changes for Skin

* Configure context initialization parameters in the web.xml file:
View changes in an ADF skin without having to restart the application
Set the value of the following context initialization parameter to **true**:\
`org.apache.myfaces.trinidad.CHECK_FILE_MODIFICATION`

* Display the full uncompressed CSS style class name at runtime
Set the value of the following context initialization parameter to **true**:\
`org.apache.myfaces.trinidad.DISABLE_CONTENT_COMPRESSION`

**web.xml**
```
<context-param>
  <param-name>org.apache.myfaces.trinidad.CHECK_FILE_MODIFICATION</param-name>
  <param-value>true</param-value>
</context-param>
<context-param>
  <param-name>org.apache.myfaces.trinidad.DISABLE_CONTENT_COMPRESSION</param-name>
  <param-value>true</param-value>
</context-param>
```

## Reference

[ADF Coding Standard and Guidelines](https://adfblog.wordpress.com/2014/01/09/adf-coding-standard-and-guidelines/)  
[An Introduction to ADF Best Practices - Oracle](https://www.oracle.com/technetwork/cn/java/introduction-best-practices-131743.pdf)
