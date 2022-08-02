


cmd脚本编程基础命令

天啦噜噜

已于 2022-05-23 16:47:19 修改

2258
 收藏 4
文章标签： cmd
版权
cmd脚本编程基础命令
1.外部命令
调用system32/64 目录下的应用程序。

2.内部命令
2.1. 显示、设置或删除环境变量。
command	des
set	显示所有变量（系统变量和自定义变量）
set /?	查询set用法
set name=aaa	自定义变量
set name	打印值
set name=	删除变量
set name=“zhangsan”	name的值为“张三”，注意set name ="zhangsan"变量名为name ，多了一个空格！
所以变量名和=之间不要加空格
set /a 5+7	参数/a后接表达式可以进行运算
set /a var=4/2	var=2
set /p	接收用户输入的信息
动态变量	描述
%变量名%	引用变量
%CD%	扩展到当前目录字符串。
%DATE%	用跟 DATE 命令同样的格式扩展到当前日期。
%TIME%	用跟 TIME 命令同样的格式扩展到当前时间。
%RANDOM%	扩展到 0 和 32767 之间的任意十进制数字。
%ERRORLEVEL%	扩展到当前 ERRORLEVEL 数值。
%CMDEXTVERSION%	扩展到当前命令处理器扩展版本号。
%CMDCMDLINE%	扩展到调用命令处理器的原始命令行
%HIGHESTNUMANODENUMBER%	扩展到此计算机上的最高 NUMA 节点号。
2.2 修改控制台界面属性命令
command	des
title …	修改cmd标题
mode 80，20	修改cmd长宽属性
color 12	修改背景、字体颜色
2.3.网络相关命令
command	des
ping	检测网络连通性
ipconfig	显示网络配置相关信息
2.4.start命令(可以打开文件、程序、网页等)
command	des
start f:	打开f盘
start /max f:\	以最大化方式打开f盘
start f:\1.txt	打开文件1.txt
start www.baidu.com	打开百度
start “”‘ ’aa bb"	打开“ aabb”如有空格加单引号,不加识别不出空格
2.5.call命令（程序的互相调用，在主程序中调用其他程序）
command	des
call demo.bat	相对路径
call f:\demo.bat	绝对路径
2.6.sort命令
command	des
sort 1.txt	将文件内容按照每行首字母排序,输出到显示屏，源文件不改变。
sort 1.txt > 2.txt	将1.txt内容排序后重定向到2.txt。
sort 1.txt /o 2.txt	效果同上
2.7.重定向
command	des
>	覆盖
>>	追加
<	从文件中读取数据流到屏幕上
di >right.txt 2>error.txt	运行正确保存在right，错误保存在error中
3.bat文件语法
command	des
echo	打印信息
pause	暂停
@echo off	关闭所有回显
echo off	关闭回显
rem	注释
exit	退出不执行下面命令
goto part1
:part1//跳转到此处
echo this is part1	跳转到part1
%1	代表传给脚本的第一个参数，可以从1到9
%~1	代表传给脚本的第一个参数，当参数以引号开头时，%~1会自动将引号删除
%*	从第一个参数开始的所有参数
%cd%	bat脚本的工作路径
%~0	取文件名（名+扩展名）
%~f0	取全路径
%~d0	取驱动器名
%~p0	只取路径（不包驱动器）
%~n0	只取文件名
%~x0	只取文件扩展名
%~s0	取缩写全路径名
%~a0	取文件属性
%~t0	取文件创建时间
%~z0	取文件大小
if,else	注意格式很容易报错
//bat脚本示例
set /a var=4+5
echo %var%
pause >nul

@echo off
set /p var=输入一个数字：
echo 输入的数字：%var%
pause >nul//中文可能会乱码
————————————————
版权声明：本文为CSDN博主「天啦噜噜」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/weixin_45819246/article/details/106931554