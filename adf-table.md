# Oracle ADF Table Cheat Sheet

Oracle ADF code mostly used, easy to copy paste


## ADF Table

### Reset af:table pagination to first page
```java
  //add this at the end of the query method
  DCIteratorBinding dcIter = (DCIteratorBinding)(BindingContext.getCurrent().getCurrentBindingsEntry()).get("xxxIterator");
  int taskIndex = 1; 
  int range = dcIter.getRangeSize();
  int oldStart = dcIter.getRangeStart();
  int newStart = taskIndex-(taskIndex % range);

  //"tableBinding" table Binding in ADF page
  dcIter.getRowSetIterator().setRangeStart(newStart);
  dcIter.setRangeStart(newStart);
  RangeChangeEvent event = new RangeChangeEvent(tableBinding, oldStart, oldStart+range, newStart, newStart+range);
  tableBinding.broadcast(event);

  dcIter.getRowSetIterator().setCurrentRowAtRangeIndex(taskIndex % range);
  dcIter.setCurrentRowIndexInRange(taskIndex % range);
  AdfFacesContext.getCurrentInstance().addPartialTarget(tableBinding);
```
