# SolarDate_convert_LunarDate
Solar Date convert to Lunar Date/Lunar Date convert to Solar Date

# 农历日期与公历日期互转

​		农历日期没有固定的数学计算规则，每年的每个月的天数均不固定。本项目的数据源来自于香港天文台。

使用如下的内表存农历年数据，字段spring_solar_date存农历春节对应的公历日期，字段lunar存农历的二进制信息。

例如：1900年二进制信息：

二进制位编号：1------------------------------24

农历年二进制：0000-0100--1011- 1101--0000-1000

1-4bit位没有使用；

5-16bit位表示农历年的1-12月每个月的天数，值为0表示29天，值为1表示30天；

17-19bit位没有使用；

20bit位表示闰月的天数，值为0表示29天，值为1表示30天；

21-24位的值表示闰月的月份数值；

```TYPES:ABAP
  BEGIN OF ys_lunar_year,
   spring_solar_date TYPE d,
   lunar       TYPE x LENGTH 3,
  END OF ys_lunar_year .
 TYPES:
  yt_lunar_year TYPE TABLE OF ys_lunar_year .

 CLASS-DATA lunar_year TYPE yt_lunar_year .
```

 项目含有农历年1900-2100总共201个农历年的信息。数据在类构造器中进行初始化。

## 农历日期转公历日期

方法：GET_LUNAR根据输入的公历日期推算得出对应的农历日期，如果是闰月设置参数IS_LEAP_MONTH的值为X，否者为空。

## 公历日期转农历日期

方法：GET_SOLAR根据输入的农历日期推算得出对应的公历日期，如果指定的是闰月需要设置输入参数IS_LEAP_MONTH的值为X，否者为空。

## 备注
  项目背景是HR在HCM中计算13薪要依据农历春节所在的公历月进行。不行格式化显示农历日期，所以不实现格式化打印方法。农历日期采用8为数字文本类表示。两个方法分别检查公历日期或者农历日期要在农历年信息允许的范围内。
