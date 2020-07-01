# java日期

## 注意事项:

 `YYYY` 表示：当天所在的周属于的年份，一周从周日开始，周六结束，只要本周跨年，那么这周就算入下一年。 

 正确格式： **yyyy-MM-dd HH:mm:ss**



## jdk1.8之前



在JDK8之前，处理日期时间，我们主要使用3个类，`Date`、`SimpleDateFormat`和`Calendar`。

这3个类在使用时都或多或少的存在一些问题，比如`SimpleDateFormat`不是线程安全的，

比如`Date`和`Calendar`获取到的月份是0到11，而不是现实生活中的1到12，关于这一点，《阿里巴巴Java开发手册》中也有提及，因为很容易犯错：

![img](https://user-gold-cdn.xitu.io/2020/6/12/172a60e40fea7057?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

不过，JDK8推出了全新的日期时间处理类解决了这些问题，比如`Instant`、`LocalDate`、`LocalTime`、`LocalDateTime`、`DateTimeFormatter`，在《阿里巴巴Java开发手册》中也推荐使用`Instant`、

`LocalDateTime`、`DateTimeFormatter`：

![img](https://user-gold-cdn.xitu.io/2020/6/12/172a60e40ff532fc?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

但我发现好多项目中其实并没有使用这些类，使用的还是之前的`Date`、`SimpleDateFormat`和`Calendar`，所以本篇博客就讲解下JDK8新推出的日期时间类，主要是下面几个：

1. Instant
2. LocalDate
3. LocalTime
4. LocalDateTime
5. DateTimeFormatter

## jdk1.8之后

### Instant

#### 1.1 获取当前时间

~~~java
Instant instant = Instant.now();
System.out.println(instant);//2020-06-10T08:22:13.759Z
//时间比北京时间少了8个小时
System.out.println(instant.atZone(ZoneId.systemDefault()));
//2020-06-10T16:22:13.759+08:00[Asia/Shanghai]
~~~

#### 1.2 获取时间戳

~~~java
Instant instant = Instant.now();
~~~

~~~java
// 当前时间戳:单位为秒
System.out.println(instant.getEpochSecond());//1591777752
// 当前时间戳:单位为毫秒
System.out.println(instant.toEpochMilli());//1591777752613
~~~

#### 1.3 将long转换为Instant

 1)根据秒数时间戳转换： 

~~~java
Instant instant = Instant.now();
System.out.println(instant);//2020-06-10T08:40:54.046Z

long epochSecond = instant.getEpochSecond();
System.out.println(Instant.ofEpochSecond(epochSecond));//2020-06-10T08:40:54Z
System.out.println(Instant.ofEpochSecond(epochSecond, instant.getNano()));//2020-06-10T08:40:54.046Z
~~~

 2)根据毫秒数时间戳转换： 

~~~java
Instant instant = Instant.now();
System.out.println(instant);//2020-06-10T08:43:25.607Z
long epochMilli = instant.toEpochMilli();
System.out.println(Instant.ofEpochMilli(epochMilli));//2020-06-10T08:43:25.607Z
~~~

#### 1.4 将String转换为Instant

```java
String text = "2020-06-10T08:46:55.967Z";
Instant parseInstant = Instant.parse(text);
System.out.println("秒时间戳:" + parseInstant.getEpochSecond());//秒时间戳:1591778815
System.out.println("豪秒时间戳:" + parseInstant.toEpochMilli());
//豪秒时间戳:1591778815967
System.out.println("纳秒:" + parseInstant.getNano());//纳秒:967000000
```

 如果字符串格式不对，比如修改成`2020-06-10T08:46:55.967`，就会抛出`java.time.format.DateTimeParseException`异常，如下图所示： 

![img](https://user-gold-cdn.xitu.io/2020/6/12/172a60e40fbf6f9d?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)





### 2.LocalDate

### 2.1 获取当前日期

使用`LocalDate`获取当前日期非常简单，如下所示：

```java
LocalDate today = LocalDate.now();
System.out.println("today: " + today);//today: 2020-06-10
```

 不用任何格式化，输出结果就非常友好，如果使用`Date`，输出这样的格式，还得配合`SimpleDateFormat`指定`yyyy-MM-dd`进行格式化.

### 2.2 获取年月日

```java
LocalDate today = LocalDate.now();

int year = today.getYear();
int month = today.getMonthValue();
int day = today.getDayOfMonth();

System.out.println("year: " + year);//year: 2020
System.out.println("month: " + month);//month: 6
System.out.println("day: " + day);//day: 10
```

 获取月份终于返回1到12了，不像`java.util.Calendar`获取月份返回的是0到11，获取完还得加1。

 ![img](https://user-gold-cdn.xitu.io/2020/6/12/172a60e41083265d?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

### 2.3 指定日期

```java
LocalDate specifiedDate = LocalDate.of(2020, 6, 1);
System.out.println("specifiedDate: " + specifiedDate);//specifiedDate: 2020-06-01
```

如果确定月份，推荐使用另一个重载方法，使用枚举指定月份：

```java
LocalDate specifiedDate = LocalDate.of(2020, Month.JUNE, 1);
```

### 2.4 比较日期是否相等

```java
LocalDate localDate1 = LocalDate.now();
LocalDate localDate2 = LocalDate.of(2020, 6, 10);
if (localDate1.equals(localDate2)) {
    System.out.println("localDate1 equals localDate2");
}//localDate1 equals localDate2
```

### 2.5 获取日期是本周/本月/本年的第几天

~~~java
LocalDate today = LocalDate.now();

System.out.println("Today:" + today);//Today:2020-06-11
System.out.println("Today is:" + today.getDayOfWeek());//Today is:THURSDAY
System.out.println("今天是本周的第" + today.getDayOfWeek().getValue() + "天");//今天是本周的第4天
System.out.println("今天是本月的第" + today.getDayOfMonth() + "天");//今天是本月的第11天
System.out.println("今天是本年的第" + today.getDayOfYear() + "天");//今天是本年的第163天
~~~

### 2.6 判断是否为闰年

```java
LocalDate today = LocalDate.now();
System.out.println(today.getYear() + " is leap year:" + today.isLeapYear());//2020 is leap year:true
```



### 3.LocalTime

如果使用`java.util.Date`，那代码是下面这样的：

```java
Date date = new Date();

int hour = date.getHours();
int minute = date.getMinutes();
int second = date.getSeconds();

System.out.println("hour: " + hour);
System.out.println("minute: " + minute);
System.out.println("second: " + second);
```

![img](https://user-gold-cdn.xitu.io/2020/6/12/172a60e40fecf261?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

 **注意事项：这几个方法已经过期了，因此强烈不建议在项目中使用：** 

### ![img](https://user-gold-cdn.xitu.io/2020/6/12/172a60e47ce6b923?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)



如果使用`java.util.Calendar`，那代码是下面这样的：

```java
Calendar calendar = Calendar.getInstance();

// 12小时制
int hourOf12 = calendar.get(Calendar.HOUR);
// 24小时制
int hourOf24 = calendar.get(Calendar.HOUR_OF_DAY);
int minute = calendar.get(Calendar.MINUTE);
int second = calendar.get(Calendar.SECOND);
int milliSecond = calendar.get(Calendar.MILLISECOND);

System.out.println("hourOf12: " + hourOf12);
System.out.println("hourOf24: " + hourOf24);
System.out.println("minute: " + minute);
System.out.println("second: " + second);
System.out.println("milliSecond: " + milliSecond);
```

![img](https://user-gold-cdn.xitu.io/2020/6/12/172a60e4a1d164fc?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

**注意事项：**获取小时时，有2个选项，1个返回12小时制的小时数，1个返回24小时制的小时数，因为现在是晚上8点，所以`calendar.get(Calendar.HOUR)`返回8，而`calendar.get(Calendar.HOUR_OF_DAY)`返回20。

如果使用`java.time.LocalTime`，那代码是下面这样的：

```java
LocalTime localTime = LocalTime.now();
System.out.println("localTime:" + localTime);

int hour = localTime.getHour();
int minute = localTime.getMinute();
int second = localTime.getSecond();

System.out.println("hour: " + hour);
System.out.println("minute: " + minute);
System.out.println("second: " + second);
```

输出结果：



![img](https://user-gold-cdn.xitu.io/2020/6/12/172a60e4b9d828e9?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)



可以看出，LocalTime只有时间没有日期。



### 4.LocalDateTime

~~~java
LocalDateTime localDateTime = LocalDateTime.now();
System.out.println("localDateTime:" + localDateTime);//localDateTime: 2020-06-11T11:03:21.376
~~~

### 获取年月日时分秒

```java
LocalDateTime localDateTime = LocalDateTime.now();
System.out.println("localDateTime: " + localDateTime);

System.out.println("year: " + localDateTime.getYear());
System.out.println("month: " + localDateTime.getMonthValue());
System.out.println("day: " + localDateTime.getDayOfMonth());
System.out.println("hour: " + localDateTime.getHour());
System.out.println("minute: " + localDateTime.getMinute());
System.out.println("second: " + localDateTime.getSecond());

```

输出结果：



![img](https://user-gold-cdn.xitu.io/2020/6/12/172a60e4baa9c956?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)



### 4.3 增加天数/小时

```java
LocalDateTime localDateTime = LocalDateTime.now();
System.out.println("localDateTime: " + localDateTime);//ocalDateTime: 2020-06-11T11:13:44.979

LocalDateTime tomorrow = localDateTime.plusDays(1);
System.out.println("tomorrow: " + tomorrow);//tomorrow: 2020-06-12T11:13:44.979

LocalDateTime nextHour = localDateTime.plusHours(1);
System.out.println("nextHour: " + nextHour);//nextHour: 2020-06-11T12:13:44.979

```

`LocalDateTime`还提供了添加年、周、分钟、秒这些方法，这里就不一一列举了：



![img](https://user-gold-cdn.xitu.io/2020/6/12/172a60e4bb8ea5eb?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

### 4.4 减少天数/小时

```java
LocalDateTime localDateTime = LocalDateTime.now();
System.out.println("localDateTime: " + localDateTime);//localDateTime: 2020-06-11T11:20:38.896

LocalDateTime yesterday = localDateTime.minusDays(1);
System.out.println("yesterday: " + yesterday);//yesterday: 2020-06-10T11:20:38.896

LocalDateTime lastHour = localDateTime.minusHours(1);
System.out.println("lastHour: " + lastHour);//lastHour: 2020-06-11T10:20:38.896
```

类似的，`LocalDateTime`还提供了减少年、周、分钟、秒这些方法，这里就不一一列举了：



![img](https://user-gold-cdn.xitu.io/2020/6/12/172a60e4bbdea611?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

### 4.5 获取时间是本周/本年的第几天

```java
LocalDateTime localDateTime = LocalDateTime.now();
System.out.println("localDateTime: " + localDateTime);//localDateTime: 2020-06-11T11:32:31.731

System.out.println("DayOfWeek: " + localDateTime.getDayOfWeek().getValue());//DayOfWeek: 4
System.out.println("DayOfYear: " + localDateTime.getDayOfYear());//DayOfYear: 163
```



### 5.DateTimeFormatter

~~~java
LocalDate localDate = LocalDate.now();

System.out.println("ISO_DATE: " + localDate.format(DateTimeFormatter.ISO_DATE));
System.out.println("BASIC_ISO_DATE: " + localDate.format(DateTimeFormatter.BASIC_ISO_DATE));
System.out.println("ISO_WEEK_DATE: " + localDate.format(DateTimeFormatter.ISO_WEEK_DATE));
System.out.println("ISO_ORDINAL_DATE: " + localDate.format(DateTimeFormatter.ISO_ORDINAL_DATE));
~~~

![img](https://user-gold-cdn.xitu.io/2020/6/12/172a60e4bde14b13?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

自定义格式：

```java
LocalDate localDate = LocalDate.now();

System.out.println("yyyy/MM/dd: " + localDate.format(DateTimeFormatter.ofPattern("yyyy/MM/dd")));//yyyy/MM/dd: 2020/06/11
```

### 5.2 格式化LocalTime

```java
LocalTime localTime = LocalTime.now();
System.out.println(localTime);//14:28:35.230
System.out.println("ISO_TIME: " + localTime.format(DateTimeFormatter.ISO_TIME));//ISO_TIME: 14:28:35.23
System.out.println("HH:mm:ss: " + localTime.format(DateTimeFormatter.ofPattern("HH:mm:ss")));//HH:mm:ss: 14:28:35
```

### 5.3 格式化LocalDateTime

```java
LocalDateTime localDateTime = LocalDateTime.now();
System.out.println(localDateTime);//2020-06-11T14:33:18.303
System.out.println("ISO_DATE_TIME: " + localDateTime.format(DateTimeFormatter.ISO_DATE_TIME));//ISO_DATE_TIME: 2020-06-11T14:33:18.303
System.out.println("ISO_DATE: " + localDateTime.format(DateTimeFormatter.ISO_DATE));//ISO_DATE: 2020-06-11
```

## 6. 类型相互转换

### 6.1 Instant转Date

JDK8中，`Date`新增了`from()`方法，将`Instant`转换为`Date`，代码如下所示：

```java
Instant instant = Instant.now();
System.out.println(instant);//2020-06-11T06:39:34.979Z

Date dateFromInstant = Date.from(instant);
System.out.println(dateFromInstant);//Thu Jun 11 14:39:34 CST 2020
```

### 6.2 Date转Instant

JDK8中，`Date`新增了`toInstant`方法，将`Date`转换为`Instant`，代码如下所示：

```java
Date date = new Date();
Instant dateToInstant = date.toInstant();//Thu Jun 11 14:46:12 CST 2020
System.out.println(date);
System.out.println(dateToInstant);//2020-06-11T06:46:12.112Z
```

### 6.3 Date转LocalDateTime

```java
Date date = new Date();
Instant instant = date.toInstant();
LocalDateTime localDateTimeOfInstant = LocalDateTime.ofInstant(instant, ZoneId.systemDefault());
System.out.println(date);//Thu Jun 11 14:51:07 CST 2020
System.out.println(localDateTimeOfInstant);//2020-06-11T14:51:07.904
```

### 6.4 Date转LocalDate

```java
Date date = new Date();
Instant instant = date.toInstant();
LocalDateTime localDateTimeOfInstant = LocalDateTime.ofInstant(instant, ZoneId.systemDefault());
LocalDate localDate = localDateTimeOfInstant.toLocalDate();
System.out.println(date);//Thu Jun 11 14:59:38 CST 2020
System.out.println(localDate);//2020-06-11
```

 可以看出，`Date`是先转换为`Instant`，再转换为`LocalDateTime`，然后通过`LocalDateTime`获取`LocalDate`。 

### 6.5 Date转LocalTime

```java
Date date = new Date();
Instant instant = date.toInstant();
LocalDateTime localDateTimeOfInstant = LocalDateTime.ofInstant(instant, ZoneId.systemDefault());
LocalTime toLocalTime = localDateTimeOfInstant.toLocalTime();
System.out.println(date);//Thu Jun 11 15:06:14 CST 2020
System.out.println(toLocalTime);//15:06:14.531
```

 可以看出，`Date`是先转换为`Instant`，再转换为`LocalDateTime`，然后通过`LocalDateTime`获取`LocalTime` .

### 6.6 LocalDateTime转Date

```java
LocalDateTime localDateTime = LocalDateTime.now();

Instant toInstant = localDateTime.atZone(ZoneId.systemDefault()).toInstant();
Date dateFromInstant = Date.from(toInstant);
System.out.println(localDateTime);//2020-06-11T15:12:11.600
System.out.println(dateFromInstant);//Thu Jun 11 15:12:11 CST 2020
```

### 6.7 LocalDate转Date

```java
LocalDate today = LocalDate.now();

LocalDateTime localDateTime = localDate.atStartOfDay();
Instant toInstant = localDateTime.atZone(ZoneId.systemDefault()).toInstant();
Date dateFromLocalDate = Date.from(toInstant);
System.out.println(dateFromLocalDate);//Thu Jun 11 00:00:00 CST 2020
```

### 6.8 LocalTime转Date

```java
LocalDate localDate = LocalDate.now();
LocalTime localTime = LocalTime.now();

LocalDateTime localDateTime = LocalDateTime.of(localDate, localTime);
Instant instantFromLocalTime = localDateTime.atZone(ZoneId.systemDefault()).toInstant();
Date dateFromLocalTime = Date.from(instantFromLocalTime);

System.out.println(dateFromLocalTime);//Thu Jun 11 15:24:18 CST 2020
```