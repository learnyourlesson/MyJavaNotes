### YYYY格式化时间

y表示year，而Y表示Week Year！

#### ISO 8601

因为不同人对于日期和时间的表示方法有不同的理解，于是，大家就共同制定了了一个国际规范：ISO 8601 。

> 国际标准化组织的国际标准ISO 8601是日期和时间的表示方法，全称为《数据存储和交换形式·信息交换·日期和时间的表示方法》。

在 ISO 8601中。对于一年的第一个日历星期有以下四种等效说法： 1，本年度第一个星期四所在的星期； 2，1月4日所在的星期； 3，本年度第一个至少有4天在同一星期内的星期； 4，星期一在去年12月29日至今年1月4日以内的星期；

根据这个标准，我们可以推算出：
**2020年第一周：2019.12.29-2020.1.4**
所以，根据ISO 8601标准，2019年12月29日、2019年12月30日、2019年12月31日这两天，其实不属于2019年的最后一周，而是属于2020年的第一周。
**JDK针对ISO 8601提供的支持**
根据ISO 8601中关于日历星期和日表示法的定义，2019.12.29-2020.1.4是2020年的第一周。

我们希望输入一个日期，然后程序告诉我们，根据ISO 8601中关于日历日期的定义，这个日期到底属于哪一年。

比如我输入2019-12-20，他告诉我是2019；而我输入2019-12-30的时候，他告诉我是2020。

为了提供这样的数据，Java 7引入了「YYYY」作为一个新的日期模式来作为标识。使用「YYYY」作为标识，再通过SimpleDateFormat就可以得到一个日期所属的周属于哪一年了。

### DD格式化时间

```java
private static void tryit(int Y, int M, int D, String pat) {
        DateTimeFormatter fmt = DateTimeFormatter.ofPattern(pat);
        LocalDate         dat = LocalDate.of(Y,M,D);
        String            str = fmt.format(dat);
        System.out.printf("Y=%04d M=%02d D=%02d " +
            "formatted with " +
            "\"%s\" -> %s\n",Y,M,D,pat,str);
    }
    public static void main(String[] args){
        tryit(2020,01,20,"MM/DD/YYYY");
        tryit(2020,01,21,"DD/MM/YYYY");
        tryit(2020,01,22,"YYYY-MM-DD");
        tryit(2020,03,17,"MM/DD/YYYY");
        tryit(2020,03,18,"DD/MM/YYYY");
        tryit(2020,03,19,"YYYY-MM-DD");
    }
```

```java
Y=2020 M=01 D=20 formatted with "MM/DD/YYYY" -> 01/20/2020
Y=2020 M=01 D=21 formatted with "DD/MM/YYYY" -> 21/01/2020
Y=2020 M=01 D=22 formatted with "YYYY-MM-DD" -> 2020-01-22
Y=2020 M=03 D=17 formatted with "MM/DD/YYYY" -> 03/77/2020
Y=2020 M=03 D=18 formatted with "DD/MM/YYYY" -> 78/03/2020
Y=2020 M=03 D=19 formatted with "YYYY-MM-DD" -> 2020-03-79
```

最后三个都错了，大写的DD代表的是处于这一年中那一天，不是处于这个月的那一天

### HH格式化时间

表示24小时制，hh表示12小时制