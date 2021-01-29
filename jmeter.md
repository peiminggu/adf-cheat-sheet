# JMeter for ADF Cheat Sheet

### Get ADF Variables 
JSR223 PostProcessor
```java
setStrictJava (true);

import java.util.*;

String msg = prev.getResponseDataAsString();
String cleanString = msg.replaceAll("\r", "").replaceAll("\n", "");
//log.info("cleanString: " + cleanString);
int start = cleanString.indexOf("AdfLoopbackUtils.runLoopback( ") + "AdfLoopbackUtils.runLoopback( ".length();
int end = cleanString.indexOf(")", start);
//log.info("start :" + start);
//log.info("end :" + end);
cleanString = cleanString.substring(start, end);

//log.info(cleanString);
int index = 0;
StringTokenizer st1 = new StringTokenizer(cleanString, ","); 
while (st1.hasMoreTokens()) {
  String str = st1.nextToken().trim();
  if (str.startsWith("'")) {
      str = str.substring(1);
  }
  if (str.endsWith("'")) {
      str = str.substring(0, str.length()-1);
  }
  //log.info(index + ": " + str );

  if (index == 2) {
  	vars.put("afrLoop",str);
  }

    if (index == 7) {
  	vars.put("adfWindowId",str);
  }

  if (index == 8 && str.contains("jsessionid")) {
  	vars.put("jsessionid",str.substring(str.indexOf("=")+1));
  }
  
  index++;
}
```

### Get ViewState 
Regular Expression Extractor

```
Field to cehck: Body

Name of created variable: adfViewState
Regular Expression: name="javax.faces.ViewState" value="(.+?)"
Template" $1$
Match No. 1
Default Value: NOT_FOUND
```
