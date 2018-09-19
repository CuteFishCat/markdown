[![](https://github.com/CuteFishCat/markdown/blob/master/source/image/01magedulogo.png)](http://www.magedu.com/)
## 一. 标题

1.1 一级标题的写法：  
`# 我是一级标题`  

显示效果如下:
# 我是一级标题

1.2 在内容下方使用任意个=号将上方内容显示成一级标题，书写方式如下：  

`Linux and Python`   
`================`

效果如下：  

Linux and Python
================

1.3 二级标题的写法：  
`## 我是二级标题`  

显示效果如下:
## 我是二级标题

1.4 在内容下方使用任意个-号将上方内容显示成一级标题，书写方式如下：  

`C  and perl `   
`----------`

显示效果如下: 

C  and perl
-----------

1.5 其他等级标题  
除了一二级标题外，mardown还支持3到6，一共6个标题大小级别，使用方式是在对应个# 号后面加上标题内容即可，3到6级标题如下：  

`######  hello world`  
`##### hello world`  
`#### hello world`  
`### hello world`  

显示效果：
###### hell world
##### hello world
#### hello world
### hello world

1.6 标题的完整书写格式  
使用对应的#号将标题前后包裹起来，格式看起来良好一些比如：  
`### hello world ###`   
显示效果：
### hello world ###

   
## 二. 段落
2.1 文章段落的基本使用   
我写一个短文，内涵两个段落，使用空行实现段落分隔效果。如下图,：  

`渔小喵老师是IT暖男一枚，忠实的开源爱好者，全栈工程师。擅长以通俗易懂的方式阐述技术原理，高度贯彻授之以渔的教学理念，深受广大学员喜爱!`

`马哥为计算机安全专业硕士，Linux核心专家、51CTO专家博主。马哥具有多年Linux实战和教学经验，擅长讲授Linux运维、企业级运维自动化、系统架构和优化、IaaS云技术等相关的课程，马哥Linux系列培训课程一直被网友们称为业内最专业的Linux培训课程。在Linux业界一直有”马哥出品必是精品”之说，其教学方法及治学态度广受赞誉，直接或间接受教的真实学员近万人，现学员已在腾讯、百度、大众点评、巨人、盛大、九城、淘宝、快钱、一号店等知名公司担当要职！`

效果：  
&ensp;&ensp;&ensp;&ensp;渔小喵老师是IT暖男一枚，忠实的开源爱好者，全栈工程师。擅长以通俗易懂的方式阐述技术原理，高度贯彻授之以渔的教学理念，深受广大学员喜爱!。

&ensp;&ensp;&ensp;&ensp;马哥为计算机安全专业硕士，Linux核心专家、51CTO专家博主。马哥具有多年Linux实战和教学经验，擅长讲授Linux运维、企业级运维自动化、系统架构和优化、IaaS云技术等相关的课程，马哥Linux系列培训课程一直被网友们称为业内最专业的Linux培训课程。在Linux业界一直有”马哥出品必是精品”之说，其教学方法及治学态度广受赞誉，直接或间接受教的真实学员近万人，现学员已在腾讯、百度、大众点评、巨人、盛大、九城、淘宝、快钱、一号店等知名公司担当要职！  

2.2 文章实现换行  
在一个内容的后面添加两个以上的空格，然后输入回车键，实现换行，比如：  
`今天天气不错，心情挺好的。我们下午没有课`  
`今天天气不错，心情挺好的。  (两个空格，然后回车)`  
`我们下午没有课`

效果:  
今天天气不错，心情挺好的。我们下午没有课  
今天天气不错，心情挺好的。  
我们下午没有课

## 三. 字体强调
3.1 使用两个 * 号 或者 _ 将要加粗的内容字体加粗  
`hello world`  
`hello **world**`  
`hello __world__`

效果对比如下：   
hello world  
hello **world**  
hello __world__ 


3.2 和加粗类似，使用一个 * 或者 _ 包裹对应内容设置为意大利斜体字样  
`hello world`  
`hello *world*`  
`hello _world_`  

对比效果如下:  
hello world  
hello *world*  
hello _world_  

3.3 使用三个 * 或 _ 加粗和斜体效果同时显示  
`hello ***world***`   
`hello ___world___`   

对比效果如下：  
hello ***world***  
hello ___world___

当然还可以使用 加粗和斜体的组合方式，有需要的可以试一下。


## 四. 引用
4.1 可以使用 > 符号引用内容，比如：  
我写的第一行代码功能就是输出 hello world  
`> hello world`

效果如下：  
我写的第一行代码功能就是输出 hello world  
> hello world  

4.2 当然你也可以引用多行，比如：  
我会多门编程语言：  
`> C `  
`> Shell `  
`> Go`  
`> Java`  
`> Python`  

效果如下：
我会多门编程语言：  
> C  
> Shell   
> Go  
> Java  
> Python

4.3 还可以使用嵌套引用
比如：  

我会多门编程语言：  
`> C `  
`> Shell `  
`> Go`  
`> Java`  
`>> Python2.7`  
`>> Python3.6`

效果如下：
我会多门编程语言：  
> C  
> Shell   
> Go  
> Java  
>> Python2.7  
>> Python3.6


## 五. 列表的使用

5.1 有序列表是用 数字 和 . 构成列表序号，如下 ：  
`1. Unix`  
`2. Linux`  
`3. C 语言`  

显示效果如下：

1. Unix
2. Linux  
3. C 语言 

也可以偷懒，数字写成一样的，不过不太建议。


5.2 无序列表使用 * - + 号表示, 如下：  

`- Unix`  
`+ Linux`  
`* C 语言` 

效果如下：  
- Unix 
+ Linux  
* C 语言


## 六. 代码
6.1 使用 `` ` `` 包含的内容为代码，比如：  

在写shell程序时，先在开头写一个   
`` `#!/bin/bash` `` 

效果如下:  
`#!/bin/bash`

6.2 当需要创建代码块时，每行使用4个空格或者一个tab键，如下：

`   #!/bin/bash`  
`   echo "hello world"` 

效果如下：  
    #!/bin/bash  
    echo "hello world"


## 七. 分割线
7.1 使用三个 * 或 -  或 _ 生成下划线

`***`   
`---`  
`___`

效果如下,三条分割线：
***  
---
___

## 八. 超链接
8.1 创建一个连接,[]内写连接内容，()内写连接地址，如下：  
`[www.magedu.com](http://www.magedu.com)`

效果如下：
[www.magedu.com](http://www.magedu.com)

## 九. 图片
9.1 和链接类似，在连接的书写格式前面加上一个!号即可代表一个图片。
书写方式如下：
`![googel](https://www.google.com/images/branding/googlelogo/1x/googlelogo_color_272x92dp.png)`


效果如下：  

![googel](https://www.google.com/images/branding/googlelogo/1x/googlelogo_color_272x92dp.png)

## 十. 代码块
10.1 当文章中有代码时，我们可以使用代码块包含代码，使其成为一个整体，并具备特定的显示风格。  
比如写个java代码，用 `` ``` ``将代码包含，并在第一行写书java格式要求
  
![](https://github.com/CuteFishCat/markdown/blob/master/source/image/02code4java.png)  
显示效果如下：  
```java
public class HelloWorld {

       public static void main(String[] args) {

                 System.out.println("Hello,World!!!");

       }

}
```


Linux使用过程中，当想将命令行的执行过程作为整体显示时，也可以使用代码块，将上面的`` ``` ``后的java缓存bash即可:  
![](https://github.com/CuteFishCat/markdown/blob/master/source/image/02codebash.png)

显示效果如下：  
```bash
[root@localhost ~]# cd /
[root@localhost /]# ll
total 28
lrwxrwxrwx.   1 root root    7 Jun  2 09:55 bin -> usr/bin
dr-xr-xr-x.   5 root root 4096 Jun  2 10:08 boot
drwxr-xr-x.  18 root root 3200 Aug 27 19:48 dev
drwxr-xr-x. 137 root root 8192 Sep 18 14:30 etc
drwxr-xr-x.   3 root root   20 Jun  2 10:18 home
lrwxrwxrwx.   1 root root    7 Jun  2 09:55 lib -> usr/lib
lrwxrwxrwx.   1 root root    9 Jun  2 09:55 lib64 -> usr/lib64
drwxr-xr-x.   2 root root    6 Nov  5  2016 media
drwxr-xr-x.   2 root root    6 Nov  5  2016 mnt
drwxr-xr-x.   3 root root   16 Jun  2 10:03 opt
dr-xr-xr-x. 135 root root    0 Aug 27 19:44 proc
dr-xr-x---.  14 root root 4096 Sep 18 14:31 root
drwxr-xr-x.  40 root root 1160 Sep 18 14:30 run
lrwxrwxrwx.   1 root root    8 Jun  2 09:55 sbin -> usr/sbin
drwxr-xr-x.   2 root root    6 Nov  5  2016 srv
dr-xr-xr-x.  13 root root    0 Aug 27 19:44 sys
drwxrwxrwt.  13 root root 4096 Sep 18 14:31 tmp
drwxr-xr-x.  13 root root  155 Jun  2 09:55 usr
drwxr-xr-x.  21 root root 4096 Jun  2 10:12 var
[root@localhost /]# 
```


## 十一. 表格
11.1 表格是一种常见的数据内容展现形式，Markdown中也可以实现表格，书写表格的语法如下，使用 ` | `代表每行中的分隔符，在分隔符之间填写具体的数据就好，在第二行内书写`-`表示上一行为表头，可以再`-`的最左边或者最又面添加一个`:`来代表下面的内容是左对齐还是右对齐，同时在两边添加`:`冒号时代表居中  
![](https://github.com/CuteFishCat/markdown/blob/master/source/image/04codetable.png)  

显示效果如下，第一行为表头：  

|Id      |Name      |Age         |Gender    |Tel         |Addr    |
|:------:|----------|-----------:|:---------|------------|--------|
|000     |Gordon    |33          |M         |123456789012|HongKong|
|001     |Mage      |45          |M         |400-080-6560|Beijing |
|002     |Linux     |48          |M         |400-080-6560|NewYork |
|003     |Wang      |66          |M         |400-080-6560|Beijing |




