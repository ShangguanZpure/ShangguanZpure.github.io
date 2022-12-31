---
title: json解析
date: 2020-04-09 22:01:01
tags:
- java
- 技术文档
categories: java
description: java的json用法解析
photos:
---

# json用法

使用阿里的fastjson包。

```java
//用到的两个类
import com.alibaba.fastjson.JSON; 
import com.alibaba.fastjson.JSONObject;

//javaObj 到 jsonSrt（两种方法）
String jsonStr = JSON.toJSONString(Obj);
String jsonStr = JSONObject.toJSONString(Obj); 

//jsonStr 到javaObj（两种方法）
	//(1) jsonStr -> JSONObject -> javaObj
JSONObject jsonObj = JSON.parseObject(jsonStr); //jsonStr -> JSONObject
Object javaObj = JSON.toJavaObject(jsonObj, Object.class); //JSONObject -> javaObj
	//(2)
Object obj = JSON.parseObject(jsonStr, Object.class);//jsonStr -> javaObj

//写jsonStr注意json串的引号需要转义
String jsonString = "{\"name\":38,\"name\":\"mkyong\"}";
```

对象中有空值的处理方法(手动处理):

```java
class MyObject{}
//对象转化为json对象obj -> jsonStr -> JSONObject;
JSONObject jsonObj = JSON.parseJson(JSON.toJsonString(MyObject));
//获取对象的成员变量
	//反射方法获取（todo）
//创建HashMap<String, Object> jsonMap,成员变量为key，jsonObj.get(key)为value
if(jsonObj.get(key) == null){
    jsonMap(key, "");
}else{
    jsonMap(key, jsonObj.get(key));
}
```

例子（测试包含json串嵌套的场景通过）

```java
import com.alibaba.fastjson.JSON;
import com.alibaba.fastjson.JSONObject;
import com.alibaba.fastjson.serializer.SerializerFeature;

/**
 * JSON 转换
 */
public final class JsonUtils {

    /**
     * 把Java对象转换成json字符串
     *
     * @param object 待转化为JSON字符串的Java对象
     * @return json 串 or null
     */
    public static <T> String parseObjToJson(T object) {
        String string = null;
        try {
            //string = JSON.toJSONString(object);
            string = JSONObject.toJSONString(object, SerializerFeature.PrettyFormat);
        } catch (Exception e) {
            System.out.println(e.getMessage());
        }
        return string;

    }

    /**
     * 将Json字符串信息转换成对应的Java对象
     *
     * @param json json字符串对象
     * @param c    对应的类型
     */
    public static <T> T parseJsonToObj(String json, Class<T> c) {
        try {
            //两个都是可行的，起码我测试的时候是没问题的。
            //JSONObject jsonObject = JSONObject.parseObject(json);
//            JSONObject jsonObject = JSON.parseObject(json);
//            return JSON.toJavaObject(jsonObject, c);
            return JSON.parseObject(json, c);
        } catch (Exception e) {
            System.out.println(e.getMessage());
        }
        return null;
    }

}
```

[1]: https://blog.csdn.net/qq_27093465/article/details/73277291	"fastjson-1.2.21 使用实例，复杂嵌套Java对象转json对象，复杂嵌套json对象转对应Java对象的代码实现"

