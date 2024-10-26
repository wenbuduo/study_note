## 时间戳
```
import time
ticks = time.time()求得当前时间与1970年1月1日午夜之间相差的秒数
```
时间戳单位最适于做日期运算。但是1970年之前的日期就无法以此表示了。太遥远的日期也不行，UNIX和Windows只支持到2038年。
## 时间元组
参考网站：
[Python 日期和时间 | 菜鸟教程 (runoob.com)](https://www.runoob.com/python/python-date-time.html)
![[Pasted image 20241111131649.png]]
```
localtime = time.localtime(time.time())
#返回
本地时间为 : time.struct_time(tm_year=2016, tm_mon=4, tm_mday=7, tm_hour=10, tm_min=3, tm_sec=27, tm_wday=3, tm_yday=98, tm_isdst=0)
```
如果想要获取某一个特定的属性就用.属性这个方法

返回可读时间函数：
```
localtime = time.asctime( time.localtime(time.time()))
#本地时间为 : Thu Apr  7 10:05:21 2016
```

格式化时间表示
```
# 格式化成2016-03-20 11:45:39形式
print time.strftime("%Y-%m-%d %H:%M:%S", time.localtime()) 
# 格式化成Sat Mar 28 22:24:24 2016形式 
print time.strftime("%a %b %d %H:%M:%S %Y", time.localtime()) 
# 将格式字符串转换为时间戳 
a = "Sat Mar 28 22:24:24 2016" 
print time.mktime(time.strptime(a,"%a %b %d %H:%M:%S %Y"))
######输出结果##########
2016-04-07 10:25:09
Thu Apr 07 10:25:09 2016
1459175064.0
```
##  时区转换

使用`pytz`和`datetime`模块进行时区转换：

```python
from datetime import datetime
import pytz

def convert_timezone(dt, from_tz, to_tz):
    # 将日期时间转换为from_tz时区
    from_zone = pytz.timezone(from_tz)
    to_zone = pytz.timezone(to_tz)
    
    # 附加上时区信息
    dt_from = from_zone.localize(dt)
    
    # 转换到目标时区
    dt_to = dt_from.astimezone(to_zone)
    
    return dt_to

# 示例
dt = datetime(2024, 10, 12, 12, 26, 23)
converted_dt = convert_timezone(dt, 'UTC', 'Asia/Shanghai')
print(converted_dt)
```
## 两个计算时间差函数的区别
未来时间-现在：两者一致  
过去-现在：  
total_seconds同未来时间-现在  （复数）
seconds = 24*60*60 - h*m（表示现在时间还有多久到明天的0点）
```
from datetime import datetime,timedelta  
schedule_time = datetime.strptime('2024-11-12 00:00:00','%Y-%m-%d %H:%M:%S')  
now = datetime.now()  
diff_time1 = (schedule_time - now).total_seconds()/60/60  
print(diff_time1)  
diff_time2 = (schedule_time - now).seconds/60/60  
print(diff_time2)  
diff_time11 = (now - schedule_time).total_seconds()/60/60  
print(diff_time11)  
diff_time22 = (now - schedule_time ).seconds/60/60  
print(diff_time22)
```
输出：
```
-14.617438771111113
9.3825
14.617438771111113
14.617222222222221
```
## 例题分析-小C的外卖判断超时
### 问题描述
小C点了一个外卖，并且急切地等待着骑手的送达。她想知道她的外卖是否超时了。已知小C在时刻 t1 点了外卖，外卖平台上显示的预计送达时间为 t2，而实际送达时间为 t3。需要判断外卖是否超时。如果外卖超时，则输出"Yes";否则输出"No"t3 在 t2 之后则认定为超时。实际送达时间与预计送达时间在 2小时之内。
### 题目答案
```
from datetime import datetime, timedelta

def solution(t1: str, t2: str, t3: str) -> str:

    # 将时间字符串转换为 datetime 对象

    t1_time = datetime.strptime(t1, "%H:%M")

    t2_time = datetime.strptime(t2, "%H:%M")

    t3_time = datetime.strptime(t3, "%H:%M")

    if t1_time > t2_time:

        t2_time += timedelta(days = 1)

    if t1_time > t3_time:

        t3_time += timedelta(days = 1)

    # 判断是否超时

    if t2_time < t3_time:

        return "Yes"

    else:

        return "No"

if __name__ == '__main__':

    print(solution("18:00", "19:05", "19:05") == 'No')

    print(solution("23:00", "00:21", "00:23") == 'Yes')

    print(solution("23:05", "00:05", "23:58") == 'No')
```