![googel](https://github.com/CuteFishCat/markdown/blob/master/logo.png)
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

`IT暖男一枚，忠实的开源爱好者，全栈工程师。擅长以通俗易懂的方式阐述技术原理，高度贯彻授之以渔的教学理念，深受广大学员喜爱的''渔小喵''。学习Linux技术，走遍天下都不怕。`

`学习Linux技术，走遍天下都不怕；学会Linux技术有饭吃，不饿肚子。`

效果：  
IT暖男一枚，忠实的开源爱好者，全栈工程师。擅长以通俗易懂的方式阐述技术原理，高度贯彻授之以渔的教学理念，深受广大学员喜爱的''渔小喵''。

学习Linux技术，走遍天下都不怕；学会Linux技术有饭吃，不饿肚子。

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
















