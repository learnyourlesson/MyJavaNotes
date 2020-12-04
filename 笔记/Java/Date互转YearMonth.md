**Date转YearMonth**

转instant 转 ZonedDateTime，转 localDate再转yearmonth

```java
Date date = new Date();
LocalDate localDate = date.toInstant().atZone(ZoneId.systemDefault()).toLocalDate();
YearMonth yearMonth = YearMonth.from(localDate);
System.out.println("YearMonth: " + yearMonth);
```

或

```java
Date date = new Date(time);
LocalDateTime localDateTime = LocalDateTime.ofInstant(date.toInstant(), ZONE_ID);
```

**YearMonth转Date**

先转localDate，转在转instant，转 ZonedDateTime，最后转成Date

```java
LocalDate localDate = YearMonth.now().atEndOfMonth();
ZoneId zone = ZoneId.systemDefault();
Instant instant = localDate.atStartOfDay(zone).toInstant();
java.util.Date date = Date.from(instant);
```

