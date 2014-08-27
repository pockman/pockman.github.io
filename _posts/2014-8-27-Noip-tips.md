# NOIP Tips
## Handy Tools
### Editor
推荐使用Emacs
+ Emacs是一款优秀的编辑器
+ 统一的编辑、编译和调试环境
+ 精致的语法高亮和自动代码格式
+ 快速查找定位和替换功能
+ 超强的拓展能力
+ 是专业计算机研究和开发人员的利器

  
### Pascal模式
Emacs根据不同的文件后缀进入不同的编辑模式，可以理解为对特定的编辑任务进行的了操作优化。
当我们打开一个Pascal文件时，Emacs进入 #Pascal# 模式。

在Pascal模式下有用的命令：
| Key     | Meaning   |
|---------+-----------|
| C-x C-f | 打开文件  |
| C-x C-c | 退出Emacs |



#### 窗口控制
| Key   | Meaning                     |
|-------+-----------------------------|
| C-x 1 | 保留当前窗口                |
| C-x 2 | 垂直分割窗口                |
| C-x 3 | 水平分割窗口                |
| C-x o | other窗口，在窗口中循环切换 |

#### 剪切板

| Key     | Meaning              |
|---------+----------------------|
| M-h     | 选中当前函数、过程块 |
| C-x h   | 选中全部文字         |
| C-x C-p | 选中当前页文字       |
|         |                      |

+ 交互式选择方法，C-SPC或者C-@标记块起点，用键盘移动光标来确定块
| Key | Meaning |
|-----+---------|
| M-w | Copy    |
| C-w | Cut     |
| C-y | Paste   |
| C-/ | Undo    |
| C-_ | Undo    |

#### 搜索
C-s   输入字符串开始搜索，然后重复C-s在文中连续搜索

#### 替换
M-%   替换字符串

#### Pascal 专用命令
| Key     | Meaning                                |
|---------+----------------------------------------|
| C-c C-b | pascal-insert-block 插入BEGIN ... END; |
| C-c C-c | pascal-comment-area                    |
| C-c C-d | pascal-goto-defun 输入函数名定位光标   |
| C-c C-o | pascal-outline-mode                    |
| C-c C-u | pascal-uncomment-area                  |

在函数和过程中快速移动
| Key   | Meaning                  |
|-------+--------------------------|
| C-M-a | pascal-beg-of-defun      |
| C-M-e | pascal-end-of-defun      |
| C-M-h | pascal-mark-defun        |
| C-M-i | completion-at-point      |
| M-#   | pascal-star-comment      |
| M-?   | completion-help-at-point |
| M-<   |                          |
| M->   | 移动到文首/文末          |

#### Makefile
假设我们要编写的pascal文件为hello.pas。
则下列makefile将编译hello.pas并生成可执行文件hello。
+ -g
  debug 模式
+ -vr
  编译错误的位置去掉括号，方便emacs/vim定位错误位置


``` makefile
  TARGET = hello
  SRC = ${TARGET}.pas
  all :
          fpc -g -vr ${SRC} -o ${TARGET}  

```

#### Find and Remove Compile Errors

| Key             | Meaning                                                             |
|-----------------+---------------------------------------------------------------------|
| M-x compile     | Compile a program.                                                  |
| M-x first-error | Jump to the line containing the first error in the current program. |
| C-x \lsquo      | Go to the next error.                                               |
| M-x gdb         | Run the debugger on the current program.                            |
| C-x SPC         | Set a breakpoint at the current line.                               |
#### 执行shell命令

| M-x     | shell                   |
| M-!     |                         |
| M-x     | shell-command-on-region |
| M-\vert |                         |

加上前导命令 C-u 后，shell命令的结果会替换当前的region

在比赛过程中经常需要对数据进行查重， 并删除重复数据，使用emacs可以很容
易完成相关的数据准备。

选中数据后 C-u M-| sort|uniq
``` bash
2
8
3
09
7
9
3
4
6
1
```

执行命令后的效果如下：
``` bash
09
1
2
3
4
6
7
8
9
``` 


### 评测和对拍脚本

下面的评测脚本可以在NOIP竞赛时使用，核心代码很短。
测试时只需要把脚本和待测程序放置到数据文件的目录中去。
脚本假设
	输入文件：<文件名>.in,  
	输出文件：<文件名>.out。
如果数据文件后缀名有异，更名后评测。

``` bash
#!/bin/bash
if test $# -lt 1 
then 
	echo usage:
	echo   oj ./yourprogram
	echo ex:  oj ./helloworld
	echo  
	exit
fi

pwd
echo TEST PROGRAM $1
for i in `ls #.in` 
do
	$1 < $i > t1.txt	
	diff -qs t1.txt ${i%.#}.out
done
echo Judge again with WHITE  SPACES ignored.
echo TEST PROGRAM $1
for i in `ls #.in` 
do
	$1 < $i > t1.txt	
	diff -qsw t1.txt ${i%.#}.out
done

```

对拍脚本在比赛中很重要，因为NOIP没有评测反馈，所以边界测试用例需要选手自己来准备。
选手需要使用正确的朴素算法程序，在相同输入数据集下，对拍检查自己的优化版实现。
``` bash
#!/bin/bash
if test $# -lt 2 
then
	echo "Usage:"
	echo "   testscript ./program1 ./program2"
	exit
fi

pwd

for i in `ls #.in` 
do
	$1 < $i > t1.txt	
	$2 < $i > t2.txt	
	diff -qs t1.txt t2.txt
done

```
## System and math unit
System unit and Math unit are allowed to use

+ assertion
``` pascal
assert(boolean expression, message); 
```

+ the usage of (binstr):
``` pascal

program haha;
var
   i : longint;
begin
   for i := 1 to 1024 do
   begin
      writeln(binstr(i, 10));
   end;
end.
output : 
       	 0000000001
	 0000000010
	 ..........
	 1111111110
	 1111111111
	 0000000000


```
+ filling a buffer
``` pascal
fillbyte : fill memory region with 8-bit pattern
fillChar : character
fillword : 16-bit
fillDword : 32-bit
fillQword : 64-bit

```
 note: #sizeof() returns the length of a type in bytes#

+ the most useful procedures and functions in unit 'math':
``` pascal
floor : 
     Return the largest integer smaller than or equal to argument 
ifthen(val : boolean, a, b) : 
     Return one of two values, depending on a boolean condition
Inrange(val : int, max, min : int) 
     the function will return true if val is in the range of min and max, 
     else the function will return false
x ## pow 
     f = x^pow
max, min(a, b) 
     you know...
Maxintvalue(data : array of integer) 
     return the largest element in the integer array
Minintvalue(data : array of integer) 
     the smallest...
sumint(data :  array of int64) 
      data[1]+data[2]+...+data[len]
```

