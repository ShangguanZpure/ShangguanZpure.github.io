---
title: java-funda-class
date: 2020-04-16 15:25:17
tags:
- java
categories: java
description: Java基本类用法汇总
photos:
---

# Java数字类

```java
//double 和 float不能表示精确的小数
//常规类型表示有范围限制
//引入BigInterger和BigDecimal，没有范围限制
//BigDecimal初始化尽量用字符串，使用double类型不准确
BigDecimal bigDecimal = new BigDecimal("10");
BigDecimal bigDecimal = new BigDecimal(2.3);//out:2.29365453658662...
	//BigDecimal做除法，记得限制输入位数
System.out.println(
    bigDecimal.divide(new BigDecimal("3"), 3, BigDecimal.ROUND_HALF_UP));

//随机数类
//使用Random类或者Math.random()
//具体的随机数范围查看源码注释确认
Random rd = new Random();
rd.nextInt();
rd.nextInt(10);//[0,10)间
rd.nextDouble();//[0.0,1.0)间
rd.ints(5,10,100);//返回10-100之间的5个随机数
Math.random();//[0.0,1.0)间
System.out.println(Math.round(Math.random()*10));

```

# 字符串相关类

字符串相关类基本都要背，因为太常用了。

String类是不可变对象，concat等改变的操作都会产生一个新的对象。

```java
String str = "123,456,789,/*/";
//增
System.out.println(str.concat("aaaa"));
//删、改
System.out.println(str.substring(2,4));
System.out.println(str.replace("123", "321"));
System.out.println(str.replaceAll("[*]", "??")); //第一个表达式是正则表达式
System.out.println(str.split(","));
System.out.println(str.trim());
//查
System.out.println(str.charAt(0));
System.out.println(str.concat("123"));
System.out.println(str.contains("123"));
System.out.println(str.indexOf("123"));
System.out.println(str.length());
System.out.println(str.isEmpty());
//空的判断与念出来的含义相符合：如果数组不为空
if(!str.isEmpty()){
    System.out.println("no empty");
}
//可变长字符对象有StringBuffer和StringBuilder
//StringBuffer是线程同步的
//StringBuilder线程不同步
//.append()功能速度上StringBuilder>StringBuffer>String
```

# 日期相关类

```java
//Calendar
import java.text.DateFormat;
import java.text.ParseException;
import java.text.SimpleDateFormat;
import java.util.Calendar;
import java.util.Date;
 
public class MyCalendar {
    //格式化声明
	private static SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd");
    //日期实例
	private static Calendar startDate = Calendar.getInstance();
	private static Calendar endDate = Calendar.getInstance();
    
	private static DateFormat df = DateFormat.getDateInstance();
	private static Date earlydate = new Date();
	private static Date latedate = new Date();
 
	/**
	 * 计算两个时间相差多少个年
	 * 
	 * @param early
	 * @param late
	 * @return
	 * @throws ParseException
	 */
	public static int yearsBetween(String start, String end) throws ParseException {
        //Str -> format -> set
		startDate.setTime(sdf.parse(start));
		endDate.setTime(sdf.parse(end));
		return (endDate.get(Calendar.YEAR) - startDate.get(Calendar.YEAR));
	}
 
	/**
	 * 计算两个时间相差多少个月
	 * 
	 * @param date1
	 *            <String>
	 * @param date2
	 *            <String>
	 * @return int
	 * @throws ParseException
	 */
	public static int monthsBetween(String start, String end) throws ParseException {
		startDate.setTime(sdf.parse(start));
		endDate.setTime(sdf.parse(end));
		int result = yearsBetween(start, end) * 12 + endDate.get(Calendar.MONTH) - startDate.get(Calendar.MONTH);
		return result == 0 ? 1 : Math.abs(result);
 
	}

————————————————
版权声明：本文为CSDN博主「浅沫微雨」的原创文章，遵循 CC 4.0 BY-SA 版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/L_hb123/java/article/details/59058209
```

