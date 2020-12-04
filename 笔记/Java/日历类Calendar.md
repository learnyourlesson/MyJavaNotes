**求上个月时间和Calendar转Date**

```java
Calendar newCalendar = Calendar.getInstance();
            newCalendar.add(Calendar.MONTH, -1);
            newCalendar.set(Calendar.DAY_OF_MONTH, 1);//每个月的第一天
            //转换
            Date lastMonth = newCalendar.getTime();
```

