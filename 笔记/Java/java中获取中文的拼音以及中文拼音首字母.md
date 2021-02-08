首先在pom文件中引入依赖 pinyin4j

```xml
    <dependencies>
        <dependency>
            <groupId>com.belerweb</groupId>
            <artifactId>pinyin4j</artifactId>
            <version>2.5.0</version>
        </dependency>
    </dependencies>
```

```
//获取中文的拼音
    @Test
    public void testPinyin() throws BadHanyuPinyinOutputFormatCombination {
        String name = "x互xx";
        char[] charArray = name.toCharArray();
        StringBuilder pinyin = new StringBuilder();
        HanyuPinyinOutputFormat defaultFormat = new HanyuPinyinOutputFormat();
        //设置大小写格式
        defaultFormat.setCaseType(HanyuPinyinCaseType.UPPERCASE);
        //设置声调格式：
        defaultFormat.setToneType(HanyuPinyinToneType.WITHOUT_TONE);
        for (int i = 0; i < charArray.length; i++) {
            //匹配中文,非中文转换会转换成null
            if (Character.toString(charArray[i]).matches("[\\u4E00-\\u9FA5]+")) {
                 String[] hanyuPinyinStringArray = PinyinHelper.toHanyuPinyinStringArray(charArray[i],defaultFormat);
                String string =hanyuPinyinStringArray[0];
                pinyin.append(string);
            } else {
                pinyin.append(charArray[i]);
            }
        }
        System.err.println(pinyin);
    }
```

```
//获取中文的首字母
    @Test
    public void testPinyin() throws BadHanyuPinyinOutputFormatCombination {
        String name = "x互xx";
        char[] charArray = name.toCharArray();
        StringBuilder pinyin = new StringBuilder();
        HanyuPinyinOutputFormat defaultFormat = new HanyuPinyinOutputFormat();
        // 设置大小写格式
        defaultFormat.setCaseType(HanyuPinyinCaseType.UPPERCASE);
        // 设置声调格式：
        defaultFormat.setToneType(HanyuPinyinToneType.WITHOUT_TONE);
        for (int i = 0; i < charArray.length; i++) {
            //匹配中文,非中文转换会转换成null
            if (Character.toString(charArray[i]).matches("[\\u4E00-\\u9FA5]+")) {
                String[] hanyuPinyinStringArray = PinyinHelper.toHanyuPinyinStringArray(charArray[i], defaultFormat);
                if (hanyuPinyinStringArray != null) {
                    pinyin.append(hanyuPinyinStringArray[0].charAt(0));
                }
            }
        }
        System.err.println(pinyin);
    }
```

#### 设置声调格式：

```
outputFormat.setToneType(HanyuPinyinToneType);1
```

方法参数HanyuPinyinToneType有以下常量对象：

###### HanyuPinyinToneType.WITH_TONE_NUMBER 用数字表示声调，例如：liu2

###### HanyuPinyinToneType.WITHOUT_TONE 无声调表示，例如：liu

###### HanyuPinyinToneType.WITH_TONE_MARK 用声调符号表示，例如：liú

设置特殊拼音ü的显示格式：

```
outputFormat.setVCharType(HanyuPinyinVCharType);1
```

方法参数HanyuPinyinVCharType有以下常量对象：

###### HanyuPinyinVCharType.WITH_U_AND_COLON 以U和一个冒号表示该拼音，例如：lu:

###### HanyuPinyinVCharType.WITH_V 以V表示该字符，例如：lv

###### HanyuPinyinVCharType.WITH_U_UNICODE 以ü表示

#### 设置大小写格式

```
outputFormat.setCaseType(HanyuPinyinCaseType);1
```

###### HanyuPinyinCaseType.LOWERCASE 转换后以全小写方式输出

###### HanyuPinyinCaseType.UPPERCASE 转换后以全大写方式输出



参考链接：https://blog.csdn.net/weixin_39297312/article/details/80944298