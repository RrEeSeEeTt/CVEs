### Software:

https://gitee.com/erzhongxmu/JEEWMS



### Description:

The reportType parameter of the JeeWMS logController.do interface has an XSS vulnerability。



### Proof of Concept:

```
GET /jeewms/logController.do?getBroswerBar&reportType=%3Cscript%3Ealert%281%29%3C%2fscript%3E HTTP/1.1
```

![alt text](img/25624-1.png)



### Code Analysis:


Start at line 185 of src/main/java/org/jeecgframework/web/system/controller/core/LogController.java:
![alt text](img/25624-2.png)


`reportType` is set to the `type` property of the `Highchart` object. If the front-end JS code is directly concatenated to HTML/JS, without any escaping or whitelist checking, it will cause xss. Navigate to the front-end js code, src/main/webapp/webpage/system/log/userBroswer.jsp:
![alt text](img/25624-3.png)


`${reportType}` is inserted directly into JS with no other processing.

 