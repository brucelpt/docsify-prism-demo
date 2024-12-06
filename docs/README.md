# 4. test title
## 4.4. Section log

### 4.4.1. Introduction

During the development process, we often need to log interfaces to facilitate subsequent maintenance and problem location, but writing logging code for each interface is very redundant. Therefore, the springboot public package provides the functions of aspect interface logging and aspect business logging to reduce code redundancy.

- Aspect log depends on sys_log and sys_user tables

### 4.4.2. Example

Please refer to the interface codes of businessLogDemo(), businessLogAddDemo(), and autoLogDemo() in the supporting example com.landicorp.ieco.test.controller.TestController class

### 4.4.3. @AutoLog

Aspect interface log annotation. Just add this annotation to the interface to enable the corresponding interface log recording function. The interface name is filled in the brackets of @AutoLog()

For example

```java
@AutoLog("Interface log example")
@PostMapping("/autoLogDemo")
public BaseResult autoLogDemo(JSONObject json)throws Exception{
return BaseResult.getSuccResult();
}
```

### 4.4.4. @BusinessLog

Aspect business log annotation. Just add this annotation to the interface to enable the corresponding business log recording function.
For example

```java
/**
* Business log interface modification example
* The difference between @BusinessLog and @AutoLog is that @BusinessLog will determine if the interface name contains the characters "modify", "edit", or "state switch", and will
* start the data comparison function to compare the modification points of the user's new and old data and record them in the database. Before modifying, you need to use the LogObjectHolder.me().set()
* method to save the database data entity before modification.
* @param json
* @return
* @throws Exception
*/
@BusinessLog(value = "Modify test interface", key = "departId")
@PostMapping("/businessLogDemo")
public BaseResult businessLogDemo(JSONObject json)throws Exception
/**
* sysDepartEntity This should be the old data entity queried from the database before modification, and then set using the LogObjectHolder.me().set() method
* Used for data comparison in @BusinessLog aspect, entities of different interfaces should be changed according to actual conditions
*/
// LogObjectHolder.me().set(sysDepartEntity);
return BaseResult.getSuccResult();
}
// Business log interface example
@BusinessLog(value = "Business log test interface", key = "itemCode")
@PostMapping("/businessLogAddDemo")
public BaseResult businessLogAddDemo(JSONObject json)throws Exception{
return BaseResult.getSuccResult();
}
```

It should be noted that the difference between @BusinessLog and @AutoLog is that @BusinessLog will determine if the interface name contains the characters "modify", "edit", or "state switch", and will start the data comparison function to compare the modification points of the user's new and old data and record the database. Before the modification, the LogObjectHolder.me().set() method needs to be used to save the database data entity before the modification.
