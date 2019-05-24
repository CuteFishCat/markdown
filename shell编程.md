#   shell脚本编程  
#!/bin/bash
#
#*****************************************************************  

#Author:               dengbingbing  

#number:                 51  

#QQ:                    790827253  

#Date:                  2018-10-14  

#FileName：             f11.sh  

#URL:                   http://www.790827253@qq.com  

#Description：          The test script  

#Copyright (C):         2018 All rights reserved  

#********************************************************************

Shell 编程跟 java、php 编程一样，只要有一个能编写代码的文本编辑器和一个能解释执行的脚本解释器就可以了。  

###  Linux 的 Shell 种类众多，常见的有：  

Bourne Shell（/usr/bin/sh或/bin/sh）  

Bourne Again Shell（/bin/bash）  

C Shell（/usr/bin/csh）  

K Shell（/usr/bin/ksh）  

Shell for Root（/sbin/sh）  

# shell变量  

定义变量时，变量名不加美元符号  
如：your_name="runoob.com"  

使用一个定义过的变量，只要在变量名前面加美元符号即可  
如：your_name="qinjx"  

echo $your_name  

echo ${your_name}  
使用 unset 命令可以删除变量  

unset variable_name  
变量类型
运行shell时，会同时存在三种变量：
1) 局部变量 局部变量在脚本或命令中定义，仅在当前shell实例中有效，其他shell启动的程序不能访问局部变量。
2) 环境变量 所有的程序，包括shell启动的程序，都能访问环境变量，有些程序需要环境变量来保证其正常运行。必要的时候shell脚本也可以定义环境变量。
3) shell变量 shell变量是由shell程序设置的特殊变量。shell变量中有一部分是环境变量，有一部分是局部变量，这些变量保证了shell的正常运行

# shell传递参数  

$#：传递到脚本的参数个数  

$*：以一个单字符串显示所有向脚本传递的参数。
如"$*"用「"」括起来的情况、以"$1 $2 … $n"的形式输出所有参数。  

$$：脚本运行的当前进程ID号  

$!：后台运行的最后一个进程的ID号  

$@：与$*相同，但是使用时加引号，并在引号中返回每个参数。
如"$@"用「"」括起来的情况、以"$1" "$2" … "$n" 的形式输出所有参数。  

$-：显示Shell使用的当前选项，与set命令功能相同。  

$?：显示最后命令的退出状态。0表示没有错误，其他任何值表明有错误。  


# shell运算符  

下表列出了常用的算术运算符，假定变量 a 为 10，变量 b 为 20：  

+：加法`expr $a + $b` 结果为 30  

-：减法`expr $a - $b` 结果为 -10  

*：乘法`expr $a \* $b` 结果为  200  

/：除法`expr $b / $a` 结果为 2   

%：取余`expr $b % $a` 结果为 0  

=：赋值a=$b 将把变量 b 的值赋给 a  

==：相等。用于比较两个数字，相同则返回 true，[ $a == $b ] 返回 false。  

!=：不相等。用于比较两个数字，不相同则返回 true。[ $a != $b ] 返回 true  

注意：条件表达式要放在方括号之间，并且要有空格，[$a==$b] 是错误的，必须写成 [ $a == $b ]

# shell echo命令
1.显示普通字符串:  
echo "It is a test"这里的双引号完全可以省略，以下命令与上面实例效果一致：echo It is a test  

2.显示转义字符  

echo "\"It is a test\""  

结果将是:"It is a test"同样，双引号也可以省略  

3.显示变量  

read 命令从标准输入中读取一行,并把输入行的每个字段的值指定给 shell 变量  

#!/bin/sh 

read name   

echo "$name It is a test"以上代码保存为 test.sh，name 接收标准输入的变量，结果将是: [root@www ~]# sh test.sh  OK                     #标准输入
OK It is a test        #输出  

4.显示换行  

echo -e "OK! \n" # -e 开启转义  

echo "It is a test"  

输出结果：OK!It is a test  

5.显示不换行  

#!/bin/sh  

echo -e "OK! \c" # -e 开启转义 \c 不换行
echo "It is a test"  

输出结果：OK! It is a test  

6.显示结果定向至文件  

echo "It is a test" > myfile  

7.原样输出字符串，不进行转义或取变量(用单引号)  

echo '$name\"'  

输出结果：$name\"  

8.显示命令执行结果  

echo `date`
注意： 这里使用的是反引号 `, 而不是单引号 '。
结果将显示当前日期

# 其他
逻辑运算

cmd1    与    cmd2       符号  &  

cmd1为真  与   cmd2    结果？  

cmd1为假  与    cmd2   结果假  

cmd1     或     cmd2      符号   |  

cmd1为真   或    cmd2    结果真   

cmd1为假   或    cmd2    结果？  

cmd1    短路与   cmd2     符号   &&  

cmd1为真      则执行cmd2   

cmd1为假       则不执行cmd2  

cmd1     短路或   cmd2    符号   ||  

cmd1为真       则不执行cmd2  

cmd1为假       则执行cmd2   


 命cmd2 cmd3 真  

cmd1 && cmd2 || cmd3   

如果cmd1为真，cmd2执行，cmd3不执行  

如果cmd1为假，cmd2不执行，cmd3执行  


命cmd2 cmd3 真
cmd1 || cmd2 && cmd3   

如果cmd1为真，cmd2不执行，cmd3执行   

如果cmd1为假，cmd2执行，cmd3执行  

###   算术运算
bash中的算术运算:help let   
+, -, *, /, %取模（取余）, **（乘方）   
实现算术运算：   
(1) let var=算术表达式   
(2) var=$[算术表达式]  
(3) var=$((算术表达式))  
(4) var=$(expr arg1 arg2 arg3 ...)  
(5) declare –i var = 数值   
(6) echo ‘算术表达式’ | bc   
乘法符号有些场景中需要转义，如*   
bash有内建的随机数生成器：$RANDOM（0-32767） echo $[$RANDOM%50] ：0-49之间随机数
