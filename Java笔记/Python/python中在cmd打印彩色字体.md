python中在cmd打印彩色字体

1. 使用最常用的方法

print("\033[0;30;40m\tHello World\033[0m")

\033[显示方式；前景色；后景色  后面的\033[... 指的是接下来输入的颜色

**显示方式:** 0（默认值）、1（高亮，即加粗）、4（下划线）、5（闪烁）7（反白显）、8（不可见）、22（非高亮显示）、24（去下划线）、25（去闪烁）、27（非反白显示）、28（可见）...

**前景色:** 30（黑色）、31（红色）、32（绿色）、 33（黄色）、34（蓝色）、35（梅色）、36（青色）、37（白色）

**背景色:** 40（黑色）、41（红色）、42（绿色）、 43（黄色）、44（蓝色）、45（梅色）、46（青色）、47（白色）

![image-20200310001121995](../img/python%E4%B8%AD%E5%9C%A8cmd%E6%89%93%E5%8D%B0%E5%BD%A9%E8%89%B2%E5%AD%97%E4%BD%93/python%E4%B8%AD%E5%9C%A8cmd%E6%89%93%E5%8D%B0%E5%BD%A9%E8%89%B2%E5%AD%97%E4%BD%931.png)

注意：需要导入 colorama 中的init

```python
from colorama import init 
init(autoreset=True)
```

1. 方法二，使用Fore设置颜色

```python
from colorama import init,Fore
init(autoreset=True)
print (Fore.YELLOW + "welcome to python !!")
print ("automatically back to default color again") 
print(Fore.RED+'SCORE: ' + str(100))
```

![img](../img/python%E4%B8%AD%E5%9C%A8cmd%E6%89%93%E5%8D%B0%E5%BD%A9%E8%89%B2%E5%AD%97%E4%BD%93/python%E4%B8%AD%E5%9C%A8cmd%E6%89%93%E5%8D%B0%E5%BD%A9%E8%89%B2%E5%AD%97%E4%BD%932.png)

1. 方法三，使用init_pair，color_pair设置颜色

```python
import curses

stdscr=curses.initscr()
 
def display_info1(str,x,y,colorpair=1):#x,y是横纵坐标
    #使用指定的colorpair显示文字
    stdscr.addstr(y,x,str,curses.color_pair(colorpair))
    stdscr.refresh()    
    
def display_info2(str,x,y,colorpair=2):#x,y是横纵坐标
    #使用指定的colorpair显示文字
    stdscr.addstr(y,x,str,curses.color_pair(colorpair))
    stdscr.refresh()
    
def get_ch_and_continue():
    #演示press any key to continue
    #设置nodelay为0时变成阻塞式等待
    stdscr.nodelay(0)
    #输入一个字符
    ch=stdscr.getch()
    #重置nodelay,使得控制台可以以非阻塞的方式接受控制台输入，超时1秒
    stdscr.nodelay(1)
    return True
 
def set_win():
    #控制台设置
    global stdscr
    #使用颜色首先需要调用这个方法
    curses.start_color()
    #文字和背景色设置，设置了两个color pair,分别为1和2
    curses.init_pair(1,curses.COLOR_GREEN,curses.COLOR_BLACK)
    curses.init_pair(2,curses.COLOR_RED,curses.COLOR_BLACK)
    #关闭屏幕回显
    curses.noecho()
    #输入时不需要回车确认
    curses.cbreak()
    #设置nodelay,使得控制台可以以非阻塞的方式接受控制台输入，超时1秒
    stdscr.nodelay(1)
    
def unset_win():
    #恢复控制台默认设置（若不恢复，会导致即使程序结束退出了，控制台仍然是没有回显的）
    curses.nocbreak()
    stdscr.keypad(0)
    curses.echo()
    #结束窗口
    curses.endwin()
    
if __name__=='__main__':
    try:
        set_win()
        display_info1('Hello,curses!',5,5)
        display_info2('Press any key to continue...',0,10)
        get_ch_and_continue()
    except Exception as e:
        raise e
    finally:
        unset_win()
```

运行结果：

![img](../img/python%E4%B8%AD%E5%9C%A8cmd%E6%89%93%E5%8D%B0%E5%BD%A9%E8%89%B2%E5%AD%97%E4%BD%93/python%E4%B8%AD%E5%9C%A8cmd%E6%89%93%E5%8D%B0%E5%BD%A9%E8%89%B2%E5%AD%97%E4%BD%933.png)

原文链接：https://blog.csdn.net/qq_15158911/article/details/88943571

https://blog.csdn.net/wls666/article/details/100867234

https://www.cnblogs.com/huchong/p/7516712.html