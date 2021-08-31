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
    for arg in elem1 elem2 elem3 ... elemN; do
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

    **举例1**

    ```shell
    # 将所有.bash 文件移动到scritp文件夹，并将
    DIR=/home/zp
    for FILE in ${DIR}/*.sh;do
    	mv "$FILE" “${DIR}/scripts”
    done
    ```

    

  * while循环

    > 检测一个条件，只要条件为真，就执行命令。
    >
    > 被检测的条件跟if..then中使用的基元并无二意。

    ```shell
    while [[ 条件 ]]; do
    	### 语句
    done
    ```

    举例1

    ```shell
    ### 0到9之间每个数的平方
    x=0
    while [[ ${x} -lt 10 ]]; do
    	echo $((x * x))
    	x=$((x+1))
    done
    ```

    

  * until循环

    > 先检测一个测试条件。
    >
    > 条件为假，就循环，跟while相反

    ```shell
    x=0
    untile [[ ${x} -ge 5 ]]; do
    	echo ${x}
    	x=`expr ${x} + 1`
    done
    ```

    

  * select循环

    > 帮助我们组织一个用户菜单。语法几乎跟for循环一致。

    ```shell
    select answer in elem1 elem2 elem3 ... elemN; do
    	### 语句
    done
    ```

    **举例1**

    ```shell
    #! /usr/bin/env bash
    
    #-------------------------------------------------------------------------
    # 用法：select会打印elem1 .. elemN以及它们的序列号到屏幕上，之后提示用户输入。
    # 输入一个数字。
    # 通常看到的是$?（PS3变量）。
    # 用户选择结果会被保存到answer中。如果answer是一个在1 .. N之间的数字，那么语句会被执行，
    # 紧接着会进行下一次迭代--如果不想这样，我们可以使用break语句。
    #--------------------------------------------------------------------------
    
    PS3="Choose the package manager: "  # 先询问用户想使用什么包管理器
    select answer in bower npm gem pip; do
    	echo -n "Enter the package name: " && read PACKAGE  # 询问想安装什么包
    	case ${PACKAGE} in
    		bower) bower install ${PACKAGE};;
    		npm) npm install ${PACKAGE};;
    		gem) gem install ${PACKAGE};;
    		pip) pip install ${PACKAGE};;
    	esac
    	break #避免无限循环
    done
    ```



* break: 提前结束当前循环

  ```shell
  # 查找10以内第一个能整除2和3的正整数
  i=1
  while [[ ${i} -lt 10 ]];do
  	if [[ $((i % 3)) -eq 0 ]] && [[ ${i % 2} -eq 0 ]]; then
  		echo ${i}
  		break
  	fi
  	i=`expr ${i} + 1`
  done
  ```

  

  

* continue：跳过某次迭代

  ```shell
  # 打印10以内的奇数
  for (( i=0;i<10;i++ )); do
  	if [[ $((i % 2)) -eq 0 ]];then
  		continue
  	fi
  	echo "${i}"
  done
  ```

  

### 8.函数

* 函数定义

  > 1. 定义函数，function关键字可有可无。
  > 2. 函数返回值（0-255）。如果不加，那么最后一条命令的结果，作为函数的返回值。
  > 3. 调用该函数后，可以用$?来获取返回值。
  > 4. 函数在使用前必须定义。这意味着必须将函数放在脚本的开始部分，直到shell解释器首次发现它时，才可以调用。
  > 5. 调用函数，仅使用其函数名即可。

  ```shell
  [ function ] function_name [()] {
  	action;
  	[return int;]
  }
  ```

  **举例1**

  ```shell
  #!/usr/bin/env bash
  
  calc(){
      PS3="Choose the oper: "
      select oper in + - \* /; do # 生成操作符菜单
          echo -n "enter first num: " && read x
          echo -n "enter second num: " && read y
          exec
          case ${oper} in
              "+") return $((${x} + ${y}));;
              "-") return $((${x} - ${y}));;
              "*") return $((${x} * ${y}));;
              "/") return $((${x} / ${y}));;
              *) echo "${oper} is not support!"; return 0;;
          esac
          break
      done
  }
  calc
  echo "the result is: $?" # $? 获取calc函数的返回值
  ```

  

* 位置参数:调用一个函数并传给它参数时，创建的变量

  位置函数变量表：

  | 变量           | 描述                           |
  | -------------- | ------------------------------ |
  | `$0`           | 脚本名称                       |
  | `$1 … $9`      | 第 1 个到第 9 个参数列表       |
  | `${10} … ${N}` | 第 10 个到 N 个参数列表        |
  | `$*` or `$@`   | 除了`$0`外的所有位置参数       |
  | `$#`           | 不包括`$0`在内的位置参数的个数 |
  | `$FUNCNAME`    | 函数名称（仅在函数内部有值）   |

  ```shell
  #!/usr/bin/env bash
  
  x=0
  if [[ -n $1 ]];then
      echo "第一个参数为：$1"
      x=$1
  else
      echo "第一个参数为空"
  fi
  
  y=0
  if [[ -n $2 ]];then
      echo "第二个参数为： $2"
      y=$2
  else
      echo "第二个参数为空"
  fi
  
  paramsFunction(){
      echo "函数第一个入参： $1"
      echo "函数第一个入参： $2"
  }
  
  paramsFunction ${x} ${y}
  ```

  

* 函数处理参数

  | 参数处理 | 说明                                             |
  | -------- | ------------------------------------------------ |
  | `$#`     | 返回参数个数                                     |
  | `$*`     | 返回所有参数                                     |
  | `$$`     | 脚本运行的当前进程 ID 号                         |
  | `$!`     | 后台运行的最后一个进程的 ID 号                   |
  | `$@`     | 返回所有参数                                     |
  | `$-`     | 返回 Shell 使用的当前选项，与 set 命令功能相同。 |
  | `$?`     | 函数返回值                                       |

  举例

  ```shell
  #!/usr/bin/env bash
  
  
  runner(){
      return 0
  }
  
  name=cxh
  paramsFunction(){
      echo "函数第一个入参：$1"
      echo "函数第二个入参：$2"
      echo "传递到脚本的参数个数：$#"
      echo "所有参数： "
      printf "+ %s\n" "$*"
      echo "脚本运行的当前进程ID号：$$"
      echo "后台运行的最后一个进程的ID号：$!"
      echo "所有参数："
      printf "+ %s\n" "$@"
      echo "Shell 使用的当前选项：$-"
      runner
      echo "runner 函数的返回值：$?"
  }
  
  paramsFunction 1 "abc" "hello, \"cxh\""
  
  ```

  

### 9.Shell扩展

> 1. 扩展发生在一行命令被分成一个个的记号（tokens）之后。
> 2. 换言之，扩展是一种执行数学运算的机制，还可以用来保存命令的执行。

* 大括号扩展

  ```shell
  echo beg{i,a,u}n # begin began begun
  echo {0..5}      # 0 1 2 3 4 5
  echo {00..8..2}  # 00 02 04 06 08
  ```

  

* 算术扩展

  ```shell
  # $(())
  result=$(( ((10 + 5*3) - 7) / 2 ))
  echo $result ### 9
  
  # 在算术表达式中，使用变量无需带上$前缀; 其实(())构造了一种C语言环境。
  x=4
  y=7
  echo $(( x + y ))      # 11
  echo $(( ++x + y++ ))  # 12
  echo $(( x + y ))      # 13
  ```

  

* 单引号和双引号

  > 1. 在双引号中，变量引用或者命令置换是会被展开的。
  > 2. 在单引号中不会。

  ```shell
  echo "Your home: $HOME" # Your home: /Users/<username>
  echo 'Your home: $HOME' # Your home: $HOME
  ```

  

* 命令置换

  > 对一个命令求值, 将其置换到另一个命令或者变量赋值表达式中。
  >
  > ``
  >
  > $()

  ```shell
  now=`date +%T`
  ### or
  now=$(date +%T)
  
  echo $now # 19:23:30
  ```

  > 当局部变量和环境变量包含空格时，它们在引号中的扩展要格外注意。

  ```shell
  # 调用第一个echo时，给了它5个单独的参数 -- INPUT被分成了单独的词，echo 在每个词之间打印了一个空格。第二种情况，调用echo时，只给了它一个参数（整个$INPUT的值，包括其中的空格）。 
  INPUT="A string with   strange    whitespace."
  echo $INPUT   ### A string with strange whitespace.
  echo "$INPUT" ### A string with   strange    whitespace.
  ```

  更严肃的例子

  ```shell
  # 尽管这个问题可以通过把 FILE 重命名成Favorite-Things.txt来解决，但是，假如这个值来自某个环境变量，来自一个位置参数，或者来自其它命令（find, cat, 等等）呢。因此，如果输入 可能 包含空格，务必要用引号把表达式包起来
  FILE="Favorite Things.txt"
  cat $FILE   # 尝试输出两个文件： `Favorite`和`Things.txt`
  cat "$FILE" # 输出一个文件： `Favorite Things.txt`
  ```

  

### 10.流和重定向

> 使用流，我们能将一个程序的输出发送到另一个程序或文件，因此，我们能方便地记录日志或做一些其它我们想做的事。

* 输入、输出流

  > Bash接收输入，并以字符序列或字符流的形式产生输出。
  >
  > 这些流能被重定向到文件或另一个流中。

  三个文件描述符：

  | 代码 | 描述符   | 描述         |
  | ---- | -------- | ------------ |
  | `0`  | `stdin`  | 标准输入     |
  | `1`  | `stdout` | 标准输出     |
  | `2`  | `stderr` | 标准错误输出 |

* 重定向

  > 重定向可以让我们控制一个命令的输入来自哪里，输出结果到什么地方。

  常用运算符：

  | 操作符 | 描述                                                         |
  | ------ | ------------------------------------------------------------ |
  | `>`    | 重定向输出                                                   |
  | `&>`   | 重定向输出和错误输出                                         |
  | `&>>`  | 以附加的形式重定向输出和错误输出                             |
  | `<`    | 重定向输入                                                   |
  | `<<`   | [Here 文档](http://tldp.org/LDP/abs/html/here-docs.html) 语法 |
  | `<<<`  | [Here 字符串](http://www.tldp.org/LDP/abs/html/x17837.html)  |

  举例

  ```shell
  # ls的结果将会被写到list.txt中
  ls -l > list.txt
  
  # 将输出附加到list.txt中
  ls -a >> list.txt
  
  # 所有的错误信息会被写到errors.txt中
  grep da * 2> errors.txt
  # 从errors.txt中读取输入
  less < errors.txt
  ```

  

* **/dev/null**文件

  > 如果希望执行某个命令，但又不希望在屏幕上显示输出结果，那么可以将输出重定向到 /dev/null：

  ```shell
  command > /dev/null
  ```

  > 如果希望屏蔽stdout和stderr，可以这样写：

  ```shell
  command > /dev/null 2>&1
  # 简化
  command &> /dev/null  # 推荐
  command >& /dev/null
  ```

  

### 11.Debug

>在shebang中使用选项

```shell
#!/bin/bash options
```

一些有用的选择：

| Short | Name        | Description                                                |
| ----- | ----------- | ---------------------------------------------------------- |
| `-f`  | noglob      | 禁止文件名展开（globbing）                                 |
| `-i`  | interactive | 让脚本以 *交互* 模式运行                                   |
| `-n`  | noexec      | 读取命令，但不执行（语法检查）                             |
| `-t`  | —           | 执行完第一条命令后退出                                     |
| `-v`  | verbose     | 在执行每条命令前，向`stderr`输出该命令                     |
| `-x`  | xtrace      | 在执行每条命令前，向`stderr`输出该命令以及该命令的扩展参数 |

举例

```shell
#!/bin/bash -x

for (( i=0;i<3;i++ ));do
	echo $i
done
```

结果：

```shell
$ ./my_script
+ (( i = 0 ))
+ (( i < 3 ))
+ echo 0
0
+ (( i++  ))
+ (( i < 3 ))
+ echo 1
1
+ (( i++  ))
+ (( i < 3 ))
+ echo 2
2
+ (( i++  ))
+ (( i < 3 ))
```

> 有时候，我们需要debug脚本的一部分。
>
> 此时，使用set命令会很方便。

```shell
# 开启 debug
set -x
for (( i = 0; i < 3; i++ ));do
	printf ${i}
done
# 关闭 debug
set +x

#  Output:
#  + (( i = 0 ))
#  + (( i < 3 ))
#  + printf 0
#  0+ (( i++  ))
#  + (( i < 3 ))
#  + printf 1
#  1+ (( i++  ))
#  + (( i < 3 ))
#  + printf 2
#  2+ (( i++  ))
#  + (( i < 3 ))
#  + set +x

for i in {1..5}; do printf ${i};done
printf "\n"
# Output: 12345
```



### 12.更多内容

- [awesome-shell](https://github.com/alebcay/awesome-shell)，shell 资源列表
- [awesome-bash](https://github.com/awesome-lists/awesome-bash)，bash 资源列表
- [bash-handbook](https://github.com/denysdovhan/bash-handbook)
- [bash-guide](https://github.com/vuuihc/bash-guide) ，bash 基本用法指南
- [bash-it](https://github.com/Bash-it/bash-it)，为你日常使用，开发以及维护 shell 脚本和自定义命令提供了一个可靠的框架
- [dotfiles.github.io](http://dotfiles.github.io/)，上面有 bash 和其它 shell 的各种 dotfiles 集合以及 shell 框架的链接
- [Runoob Shell 教程](http://www.runoob.com/linux/linux-shell.html)
- [shellcheck](https://github.com/koalaman/shellcheck) 一个静态 shell 脚本分析工具，本质上是 bash／sh／zsh 的 lint。

最后，Stack Overflow 上 [bash 标签下](https://stackoverflow.com/questions/tagged/bash)有很多你可以学习的问题，当你遇到问题时，也是一个提问的好地方。

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

