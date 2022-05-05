# Tips

### case-insensitive sort in adf table

* add a transient attribute, specify Value Type as expression
* Specify the following expression : XXX.toUpperCase()
* Update binding, add the new transient attribute to Disaply Attributes
* Update column sortProperty to the new attribute
