[TOC]

## introduction

欢迎您使用shell开发文档

希望以下文档可以帮助您解决开发中遇到的函数问题，如有纰漏之处，欢迎您向我们反馈。

## 前言

```shell
███████╗██╗  ██╗███████╗██╗     ██╗
██╔════╝██║  ██║██╔════╝██║     ██║
███████╗███████║█████╗  ██║     ██║
╚════██║██╔══██║██╔══╝  ██║     ██║
███████║██║  ██║███████╗███████╗███████╗
```

由于**bash**是**Linux**标准默认的**shell**解释器，可以说**bash**是**shell**编程的基础。

本文主要介绍**bash**的语法，对于**Linux**指令不做过多介绍。

## 开发指南

## 学习前的准备

## 更新记录

## Shell编程

### 1.简介

* 什么是shell

  * 底层是C语言；
  * 一种命令语言，又是一种程序设计语言；
  * 一个应用程序，带界面的；

* 什么是shell脚本

  * 一种为shell编写的脚本程序
  * 后缀：`.sh`
  * 业界所说的**shell**是指**shell**脚本；

* shell环境

  * 文本编辑器+解释器

  * 解释器：

    | 解释器 | 说明                                             |
    | ------ | ------------------------------------------------ |
    | sh     | 即Bourne Shell。sh是Unix标准默认的shell          |
    | bash   | 即Bourne Again Shell。bash是Linux标准默认的shell |
    | fish   | 智能和用户友好的命令行shell                      |
    | xiki   | 使shell控制台更有好，更强大                      |
    | zsh    | 功能更强大的shell与脚本语言                      |

    

  * 指定脚本解释器,**开头注释**

    ```shell
    #!/bin/sh
    #!/bin/bash
    #!/usr/bin/env bash      #推荐使用，有时候不确定bash在哪里，这样做程序自动取path中查找
    ```

* 模式

  ​        shell有交互和非交互两种模式

  * 交互模式：执行命令行

  * 非交互模式：执行shell脚本，shell程序从文件或管道中读取命令并执行

    ```shell
    sh xx.sh
    bash xx.sh
    source xx.sh  # 等价与 . /xx.sh
    . /xx.sh      # 注意空格
    ```

    通过添加**可执行权限**，来执行

    ```shell
    chmod +x scripts.sh  
    ./scripts.sh   # 在脚本的first line，必须指定解释器，否则不能运行
    ```

    `scripts.sh`

    ```shell
    #!/usr/bin/env bash
    echo "Hello, world!"
    ```

    

### 2.基本语法

* 解释器

  * shebang/hashbang: #!  #:hash  !:bang

* 注释:被解释器忽略

  * 单行：以`#`开头，到行尾结束。

  * 多行：以`:<<EOF`开头，到`EOF`结束。

    ```shell 
    #--------------------------------
    #  shell 注释示例
    #  author： cxh
    #--------------------------------
    
    # echo '这是单行注释'
    
    ############## 这是分割线 ###########
    
    :<<EOF
    echo '这是多行注释'
    echo '这是多行注释'
    echo '这是多行注释'
    EOF
    ```

    

* echo: 用于输出字符串

  ```shell
  echo "hello, world" # 输出： hello, world
  
  echo "hello,\"cxh\"" # 输出： hello, "cxh"
  
  name="cxh"
  echo "hello,\"${name}\"" # 输出：hello, "cxh"
  
  echo "YES\nNO"  # 输出："YES\nNO"
  
  echo -e "YES\nNO" #输出：
  #YES
  #NO
  
  echo -n "YES" # echo默认添加换行符，输出结尾不换行
  echo -e "YES\C" # 同上
  
  echo "test" > test.txt # 输出重定向
  
  echo `pwd` # 输出执行结果
  ```

  

* printf：用于格式化输出字符串

  ```shell
  printf '%d %s\n' 1 "abc" # Output:1 abc
  printf "%d %s\n" 1 "abc" # Output:1 abc
  printf %s abcdef # Output:abcdef
  
  # 格式指定一个参数要输入，但多出的参数仍然会按照该格式输出；类似jq中的迭代过滤
  printf "%s\n" abc def 
  # abc
  # def
  
  printf "%s %s %s\n" a b c d e f g h i j
  # a b c
  # d e f
  # g h i
  # j
  
  # 如果没有参数
  printf "%s and %d\n"
  #   and 0
  
  # 格式化输出
  printf "%-10s %-8s %-4s\n" 姓名 性别 体重Kg   # - 左对齐，键盘上-号在+号的左边
  printf "%-10s %-8s %-4.2f\n" 郭靖 男 66.1234
  printf "%-10s %-8s %-4.2f\n" 杨过 男 48.6543
  printf "%-10s %-8s %-4.2f\n" 郭芙 女 47.9876
  ```

  **printf 的转义符**

  | 序列    | 说明                                                         |
  | ------- | ------------------------------------------------------------ |
  | `\a`    | 警告字符，通常为 ASCII 的 BEL 字符                           |
  | `\b`    | 后退                                                         |
  | `\c`    | 抑制（不显示）输出结果中任何结尾的换行字符（只在%b 格式指示符控制下的参数字符串中有效），而且，任何留在参数里的字符、任何接下来的参数以及任何留在格式字符串中的字符，都被忽略 |
  | `\f`    | 换页（formfeed）                                             |
  | `\n`    | 换行                                                         |
  | `\r`    | 回车（Carriage return）                                      |
  | `\t`    | 水平制表符                                                   |
  | `\v`    | 垂直制表符                                                   |
  | `\\`    | 一个字面上的反斜杠字符                                       |
  | `\ddd`  | 表示 1 到 3 位数八进制值的字符。仅在格式字符串中有效         |
  | `\0ddd` | 表示 1 到 3 位的八进制值字符                                 |

### 3.变量

* 介绍
  * bash中无数据类型
  * 变量中可以保存：数字、字符、字符串
  * 无需提前声明变量，给变量赋值时，直接就创建变量了

* 变量命名原则

  * 英文、字符、下划线，首个字符不能是数字
  * 中间不能有空格，可以使用下划线代替
  * 不能使用标点符号
  * 不是使用bash中的关键字

* 声明变量

  * 访问变量：`${var}`和`$var`
  * 推荐使用带`{}`的，以帮助解释器识别变量的边界

* 只读变量

  * `readonly`命令可以将变量定义为只读，只读变量的值不能被改变。

    ```shell
    rword="hello"
    echo "${rword}"
    readonly rword
    # rword="bye"  # 如果放开注释，执行时会报错
    ```

    

* 删除变量

  * unset命令删除变量，删除后不能被访问；注意：不能删除只读变量；

    ```shell 
    dword="hello" # 声明变量
    echo "${dword}" # 可以访问
    
    unset dword # 删除变量
    echo "${dword}" # 输出为空
    ```

    

* 变量类型

  * 局部变量：脚本内的变量，不可以被其他程序或脚本使用。

  * 环境变量：当前shell会话内所有的程序或脚本都可以见的变量。使用`export`关键字创建。shell脚本也可以定义环境变量。

  * 常见环境变量：

    | 变量    | 描述                                              |
    | ------- | ------------------------------------------------- |
    | $HOME   | 当前用户的用户目录                                |
    | $PATH   | 用分号分隔的目录列表，shell会到这些目录中查找命令 |
    | $PWD    | 当前工作目录                                      |
    | $RANDOM | 0-32767之间的整数                                 |
    | $UID    | 数值类型，当前用户的用户ID                        |
    | $PS1    | 主要系统输入提示符                                |
    | $PS2    | 次要系统输入提示符                                |

    [这里](https://tldp.org/LDP/Bash-Beginners-Guide/html/sect_03_02.html###sect_03_02_04)有一张更全面的Bash环境变量列表。

### 4.字符串

* 单引号与双引号：可以使用单引号、双引号、也可以不实用引号。

  * 单引号：
    * 不识别变量
    * 单引号里面不能出现单独的单引号，使用转义也不行，但可以成对出现，作为字符串拼接使用。
  * 双引号
    * 识别变量
    * 可以出现转义

* 拼接字符串

  ```shell
  # 单引号拼接
  name1='white'
  str1='hello, '${name1}''  # 注意这里是三个字符串的拼接，'hello, ' ${name2} ''
  str2='hello, ${name1}'
  echo ${str1}_${str2}
  # hello, white_hello, ${name1}
  
  # 双引号拼接
  name2="black"
  str3="hello, "${name2}""
  str4="hello, ${name2}"
  echo ${str3}_${str4}
  # hello, white_hello, white
  ```

  

* 获取字符串的长度

  ```shell
  text="12345"
  echo ${#text}
  # 5
  ```

  

* 截取子字符串

  ```shell
  text="12345"
  echo ${text:2:2}   # 从0开始计数，截取2个数据
  # 34
  ```

  

* 查找子字符串

  ```shell
  #!/usr/bin/env bash
  
  text="hello"
  echo `expr index "${text}" ll`
  ```

  

### 5.数组

* 简介
  * 一维数组
  * 下标从0开始
  * 下标：整数、算术表达式
  * 下标>=0

* 创建数组

  ```shell
  nums=([2]=2 [0]=0 [1]=1)
  colors=(red yellow "dark blue")
  ```

  

* 访问数组

  ```shell
  # 访问单个元素
  echo "${nums[1]}"
  ```
  ```shell
  # 访问部分元素
  echo "${nums[@]:0:2}"
  ```

  ```shell
  # 访问所有元素
  echo "${nums[*]}"     # 废弃,不适用于元素带空格的字符串
  echo "${nums[@]}"   # 推荐
  ```

  

* 访问数组长度

  ```shell
  echo ${#nums[@]}
  echo ${#nums[*]}
  ```

  

* 向数组中添加元素

  ```shell
  colors=(white "${colors[@]}" green black)
  echo "${colors[@]}"
  # white red yellow dark blue green black
  ```

  

* 从数组中删除元素

  ```shell
  unset nums[0]
  echo "${nums[@]}"
  
  ```

  

### 6.运算符

* 算术运算符

  | 运算符 | 说明                                          | 举例                           |
  | ------ | --------------------------------------------- | ------------------------------ |
  | +      | 加法                                          | `expr $x + $y` 结果为 30。     |
  | -      | 减法                                          | `expr $x - $y` 结果为 -10。    |
  | *      | 乘法                                          | `expr $x * $y` 结果为 200。    |
  | /      | 除法                                          | `expr $y / $x` 结果为 2。      |
  | %      | 取余                                          | `expr $y % $x` 结果为 0。      |
  | =      | 赋值                                          | `x=$y` 将把变量 y 的值赋给 x。 |
  | ==     | 相等。用于比较两个数字，相同则返回 true。     | `[ $x == $y ]` 返回 false。    |
  | !=     | 不相等。用于比较两个数字，不相同则返回 true。 | `[ $x != $y ]` 返回 true。     |

  

* 关系运算符

  > 关系运算符只支持数字，不支持字符串，除非字符串的值是数字

  | 运算符 | 说明                                                  | 举例                         |
  | ------ | ----------------------------------------------------- | ---------------------------- |
  | `-eq`  | 检测两个数是否相等，相等返回 true。                   | `[ $a -eq $b ]`返回 false。  |
  | `-ne`  | 检测两个数是否相等，不相等返回 true。                 | `[ $a -ne $b ]` 返回 true。  |
  | `-gt`  | 检测左边的数是否大于右边的，如果是，则返回 true。     | `[ $a -gt $b ]` 返回 false。 |
  | `-lt`  | 检测左边的数是否小于右边的，如果是，则返回 true。     | `[ $a -lt $b ]` 返回 true。  |
  | `-ge`  | 检测左边的数是否大于等于右边的，如果是，则返回 true。 | `[ $a -ge $b ]` 返回 false。 |
  | `-le`  | 检测左边的数是否小于等于右边的，如果是，则返回 true。 | `[ $a -le $b ]`返回 true。   |

* 布尔运算符

  | 运算符 | 说明                                                | 举例                                       |
  | ------ | --------------------------------------------------- | ------------------------------------------ |
  | `!`    | 非运算，表达式为 true 则返回 false，否则返回 true。 | `[ ! false ]` 返回 true。                  |
  | `-o`   | 或运算，有一个表达式为 true 则返回 true。           | `[ $a -lt 20 -o $b -gt 100 ]` 返回 true。  |
  | `-a`   | 与运算，两个表达式都为 true 才返回 true。           | `[ $a -lt 20 -a $b -gt 100 ]` 返回 false。 |

* 逻辑运算符

  | 运算符 | 说明       | 举例                                            |
  | ------ | ---------- | ----------------------------------------------- |
  | `&&`   | 逻辑的 AND | `[[ ${x} -lt 100 && ${y} -gt 100 ]]` 返回 false |
  | `||`   | 逻辑的 OR  | `[[ ${x} -lt 100 || ${y} -gt 100 ]]` 返回 true  |

* 字符串运算符

  | 运算符 | 说明                                       | 举例                       |
  | ------ | ------------------------------------------ | -------------------------- |
  | `=`    | 检测两个字符串是否相等，相等返回 true。    | `[ $a = $b ]` 返回 false。 |
  | `!=`   | 检测两个字符串是否相等，不相等返回 true。  | `[ $a != $b ]` 返回 true。 |
  | `-z`   | 检测字符串长度是否为 0，为 0 返回 true。   | `[ -z $a ]` 返回 false。   |
  | `-n`   | 检测字符串长度是否为 0，不为 0 返回 true。 | `[ -n $a ]` 返回 true。    |
  | `str`  | 检测字符串是否为空，不为空返回 true。      | `[ $a ]` 返回 true。       |

* 文件测试运算符

  | 操作符  | 说明                                                         | 举例                        |
  | ------- | ------------------------------------------------------------ | --------------------------- |
  | -b file | 检测文件是否是块设备文件，如果是，则返回 true。              | `[ -b $file ]` 返回 false。 |
  | -c file | 检测文件是否是字符设备文件，如果是，则返回 true。            | `[ -c $file ]` 返回 false。 |
  | -d file | 检测文件是否是目录，如果是，则返回 true。                    | `[ -d $file ]` 返回 false。 |
  | -f file | 检测文件是否是普通文件（既不是目录，也不是设备文件），如果是，则返回 true。 | `[ -f $file ]` 返回 true。  |
  | -g file | 检测文件是否设置了 SGID 位，如果是，则返回 true。            | `[ -g $file ]` 返回 false。 |
  | -k file | 检测文件是否设置了粘着位(Sticky Bit)，如果是，则返回 true。  | `[ -k $file ]`返回 false。  |
  | -p file | 检测文件是否是有名管道，如果是，则返回 true。                | `[ -p $file ]` 返回 false。 |
  | -u file | 检测文件是否设置了 SUID 位，如果是，则返回 true。            | `[ -u $file ]` 返回 false。 |
  | -r file | 检测文件是否可读，如果是，则返回 true。                      | `[ -r $file ]` 返回 true。  |
  | -w file | 检测文件是否可写，如果是，则返回 true。                      | `[ -w $file ]` 返回 true。  |
  | -x file | 检测文件是否可执行，如果是，则返回 true。                    | `[ -x $file ]` 返回 true。  |
  | -s file | 检测文件是否为空（文件大小是否大于 0），不为空返回 true。    | `[ -s $file ]` 返回 true。  |
  | -e file | 检测文件（包括目录）是否存在，如果是，则返回 true。          | `[ -e $file ]` 返回 true。  |

### 7.控制语句

* 条件语句

  * 条件取决于一个包在`[[  ]]`里的表达式。又叫**检测命令**或**基元**。

  * `[[  ]]`是`Bash`中用的。

  * `[  ]`是`sh`中用的。

  * if语句

    ```shell
    # 写成一行
    if [[ 1 -eq 1 ]];then echo "1 -eq 1 result is: true";fi
    # 写成多行
    if [[ "abc" = "abc" ]];then
    	echo ""abc" -eq "abc" result is: true"
    fi
    ```

    

  * if-else语句

    ```shell
    if [[ 2 -ne 1 ]];then
    	echo "true"
    else
    	echo "false"
    fi
    ```

    

  * if-elif-else语句

    ```shell
    x=10
    y=20
    if [[ ${x} > ${y} ]];then
    	echo "${x} > ${y}"
    elif [[ ${x} < ${y} ]];then
    	echo "${x} < ${y}"
    else
    	echo "${x} = ${y}"
    fi
    ```

    

  * case语句:解决复杂的条件判断

    > 1. 每种情况都是匹配了某个模式的表达式。
    > 2. |用来分隔多个模式。
    > 3. ）用来结束一个模式的序列。
    > 4. 第一个匹配上的模式对应的命令将会被执行。
    > 5. *代表任何不匹配以上给定模式的模式
    > 6. 命令块之间用;;分隔

    ```shell
    case ${oper} in
    "+")
    	val=`expr ${x} + ${y}`
    	echo "${x} + ${y} = ${val}"
    ;;
    "-")
    	val=`expr ${x} - ${y}`
    	echo "${x} - ${y} = ${val}"
    ;;
    "*")
    	val=`expr ${x} '*' ${y}`
    	echo "${x} * ${y} = ${val}"
    ;;
    "/")
    	val=`expr ${x} / ${y}`
    	echo "${x} / ${y} = ${val}"
    ;;
    *)
    	echo "Unknow oper!"
    ;;
    esac
    ```

    

* 循环语句

  * for循环

    > arg依次被赋值为从elem1到elemN；
    >
    > 这些值还可以是通配符或者大括号扩展；

    ```shell
    for arg in elem1 elem2 elem3 ... elemN;do
    	# 执行命令语句
    done
    ```

    ```shell
    for i in {1..5};do echo ${i};done
    ```

    > 类似C语言

    ```shell
    for (( i=0; i<10; i++ ));do
    	echo ${i}
    done
    ```

    

  * while循环

  * until循环

  * select循环

### 8.函数

* 位置参数
* 函数处理参数

### 9.Shell扩展

### 10.流和重定向

* 输入、输出流
* 重定向
* **/dev/null**文件

### 11.Debug

### 12.更多内容

## Shell常用函数

## 基本函数

## 文本输入

## 文字识别

## 日志输出

## 应用

## 文件操作

## 网络

## 数据库

## 字符串

## 系统

## 脚本操作

## 解压缩

## 协程/多线程

