# Shell脚本

## 1. 概念

Shell连接用户和Linux内核，让用户能够更加高效的使用Linux内核，可以看作是一个"中间商"。

Liunx系统脚本语言，编写完后不用编译便可直接运行。 

解释型语言，一边解释一边执行，如果脚本中包含错误(ex:调用了不存在的函数)，只要没执行到这一行，就不会报错。

> ps：Linux执行可执行文件命令`./文件`就可执行，或者是`sh 文件名`

第一个例子：

```shell
#!/bin/bash

####################################
# @description 第一个shell脚本
#              #!   => 是一个约定的标记，它告诉系统这个脚本需要什么解释器来执行，即使用哪一种 Shell。
#              echo => 命令用于向窗口输出文本。
# @example => sh 02-helloworld.sh 如何去执行启动
# @author miaowei
# @date 2021/10/13 11:31 下午
####################################

echo "Hello World !
```

## 2.命令介绍

### 2.1 注释

单行注释：以#开头所在一行的内容

多行注释：	

```shell
:<<EOF
注释内容...
注释内容...
注释内容...
EOF
```

```shell
:<<'
注释内容...
注释内容...
注释内容...
'
```

```shell
:<<!
注释内容...
注释内容...
注释内容...
!
```

```shell
function annotation(){
  注释...
  echo "这是一个不被执行的函数，也可作为注释"
  注释...
}
```

### 2.2 输出

```shell
#!/bin/bash

echo "================= ↓↓↓↓↓↓ echo输出文本 ↓↓↓↓↓↓ ================="

# echo会自动在末尾添加换行符
# 输出普通字符串
echo "Hello World !"
echo Hello World !

# 输出转义字符
echo "\"Hello World !\""

# 显示变量
name=miaowei
echo ${name}
echo $name
echo "$name"
unset name	#  删除变量
echo "删除变量：${name}" #输出为空 

# \n显示换行
echo -e "Hi ! \n" # -e 开启转义
echo "嗨!"

# \c 显示不换行
echo -e "Hello ! \c" # -e 开启转义 
echo "你好"

# 输出至文件，文件不存在会自动创建一个新的
echo "hi" > hi.txt
echo ">:重定向文件内容   >>:追加内容" >> hi.txt

# 原样输出字符串，不进行转义或取变量(用单引号)
echo '$name\"'

# 显示命令执行结果
echo `date +"%Y-%m-%d %H:%M:%S"`  # 输出结果：2022-12-08 09:18:41	应该是将日期按照某种格式进行输出



printf "================= ↓↓↓↓↓↓ printf输出文本 ↓↓↓↓↓↓ =================\n"

# printf需手动在末尾添加\n实现像echo一样的换行
printf "Hello, Shell\n"

# %s %c %d %f 都是格式替代符，％s 输出一个字符串，％d 整型输出，％c 输出一个字符，％f 输出实数，以小数形式输出。
# %-10s 指一个宽度为 10 个字符（- 表示左对齐，没有则表示右对齐），任何字符都会被显示在 10 个字符宽的字符内，如果不足则自动以空格填充，超过也会将内容全部显示出来。
# %-4.2f 指格式化为小数，其中 .2 指保留2位小数。
printf "%-10s %-9s %-4s\n" 姓名 性别 体重kg
printf "%-10s %-8s %-4.2f\n" 张三 男 66.1234
printf "%-10s %-8s %-4.3f\n" 李四 男 88.8888
printf "%-10s %-8s %d\n" 王二 男 55
# 如果格式只指定了一个参数，但多出的参数仍然会按照该格式输出，format-string 被重用
printf "%s %s %s\n" a b c d e f g hi j
# 如果没有arguments，那么 %s 用NULL代替，%d 用 0 代替
printf "%s and %d \n"
```

### 2.3 变量

使用:

> 1. $name
> 2. ${name}

输出：**echo ${name}**---建议第二种加花括号是为了帮助解释器识别变量的边界

```shell
readonly name # 只读变量 即声明之后不能修改其值
unset name # 变量被删除后不能再次使用，注：unset命令不能删除只读变量
```

命令规则：

1. 命名只能使用英文字母，数字和下划线，首个字符不能以数字开头
2. 中间不能有空格，可以使用下划线 _
3. 不能使用标点符号
4. 不能使用bash里的关键字
5. 等号两边不能有空格！

变量类型：

1. 局部变量: 局部变量在脚本或命令中定义，仅在当前shell实例中有效，其他shell启动的程序不能访问局部变量。
2. 环境变量: 所有的程序，包括shell启动的程序，都能访问环境变量，有些程序需要环境变量来保证其正常运行。必要的时候shell脚本也可以定义环境变量。
3. shell变量: shell变量是由shell程序设置的特殊变量。shell变量中有一部分是环境变量，有一部分是局部变量，这些变量保证了shell的正常运行

### 2.4 字符串

单引号：

1. 单引号需成对出现
2. 单引号中的所有内容都会被当作普通字符，原样输出

双引号：

1. 可以输出变量值
2. 可识别转义字符

```shell
str='this is a string'	# 输出：this is a string

name='miaowei'
name_new="${name}-new"
echo "姓名: \"${name_new}\""		# 姓名: "miaowei-new"

# 获取字符串长度
name='miaowei'
echo "name长度: ${#name}" 	# name长度： 7

# 从字符串第5个字符开始(不包括第5个字符)截取4个字符
name='miaowei'
echo ${name:5:4} 		# ei

# 查找字符e或h的位置(哪个字母先出现就计算哪个)
name='miaowei'
echo `expr index "${name}" ai`   # 2
```

### 2.5 Shell传递参数

执行脚本时，后面用英文空格分隔要传递的多个参数值 如果有单个参数中包含空格，可用双引号包裹参数值，避免shell将此识别为多个参数

- **#** 代表数量
- **@** 全部

```shell
sh test.sh 1 2 "3 3"
```

```shell
echo "执行的文件名：${0}"

echo "第1个参数为：${1}"
echo "第2个参数为：${2}"
echo "第3个参数为：${3}"

echo "参数个数：${#}"
echo "所有参数信息(输出作为一个参数，即循环只打印1个参数值)：${*}"
for i in "${*}"; do
    echo ${i}
done

echo "所有参数信息(输出作为传递的n个参数，即可循环打印多个参数值)：${@}"
for i in "${@}"; do
    echo ${i}
done

echo "脚本运行的当前进程ID号：${$}"
echo "后台运行的最后一个进程的ID号：${!}"
echo "显示Shell使用的当前选项(与Linux-set命令功能相同)：${-}"
echo "显示最后命令的退出状态。0表示没有错误，其他任何值表明有错误：${?}"
```

![image-20221208101049806](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202212081010904.png)

### 2.6 数组

用英文括号来表示数组，数组元素用"空格"符号分割开 下标从0开始计数，下标无限制

```shell
# 声明数组
array_name=(q1 q2 q3)
# 单个定义
array_name[10]=q10

# 取值-根据下标取单个
echo ${array_name[0]} # q1
# 取值-全部
echo ${array_name[@]} # q1 q2 q3 q10

# 获取数组元素个数
echo ${#array_name[@]} # 4
# 或
echo ${#array_name[*]} # 4
# 获取数组单个元素的长度
echo ${#array_name[10]} # 3
```

其中：

- @： 代表全部
- *： 代表全部

### 2.7 运算符

```shell
sum=`expr 2 + 3`
echo "和: ${sum}" # 5
```

1. 表达式需用**``**包裹
2. 表达式和运算符两边需有空格

**算术运算符**

| 运算符 | 说明                                          |
| ------ | --------------------------------------------- |
| +      | 加法                                          |
| -      | 减法                                          |
| *      | 乘法                                          |
| /      | 除法                                          |
| %      | 取余                                          |
| =      | 赋值                                          |
| ==     | 相等。用于比较两个数字，相同则返回 true。     |
| !=     | 不相等。用于比较两个数字，不相同则返回 true。 |

```shell
a=10
b=20
echo "a=${a}"
echo "b=${b}"

echo "\n================= ↓↓↓↓↓↓ 算术运算符 ↓↓↓↓↓↓ ================="

echo "a + b : `expr ${a} + ${b}`"	# 30
echo "a - b : `expr ${a} - ${b}`"	# -10
echo "a * b : `expr ${a} \* ${b}`"	# 200
echo "b / a : `expr ${b} / ${a}`"	# 2
echo "b % a : `expr ${b} % ${a}`"	# 0

if [ ${a} == ${b} ]
then
   echo "a 等于 b"
fi

if [ ${a} != ${b} ]
then
   echo "a 不等于 b"		# 输出
fi
```

**关系运算符**

| 运算符 | 说明     |
| ------ | -------- |
| -eq    | 等于     |
| -ne    | 不等于   |
| -gt    | 大于     |
| -ge    | 大于等于 |
| -lt    | 小于     |
| -le    | 小于等于 |

```shell
a=10
b=20
echo "a=${a}"
echo "b=${b}"

echo "\n================= ↓↓↓↓↓↓ 关系运算符 ↓↓↓↓↓↓ ================="

if [ ${a} -eq ${b} ]
then
   echo "${a} -eq ${b} : a 等于 b"
else
   echo "${a} -eq ${b}: a 不等于 b"
fi

if [ ${a} -ne ${b} ]
then
   echo "${a} -ne ${b}: a 不等于 b"
else
   echo "${a} -ne ${b} : a 等于 b"
fi

if [ ${a} -gt ${b} ]
then
   echo "${a} -gt ${b}: a 大于 b"
else
   echo "${a} -gt ${b}: a 不大于 b"
fi

if [ ${a} -ge ${b} ]
then
   echo "${a} -ge ${b}: a 大于或等于 b"
else
   echo "${a} -ge ${b}: a 小于 b"
fi

if [ ${a} -lt ${b} ]
then
   echo "${a} -lt ${b}: a 小于 b"
else
   echo "${a} -lt ${b}: a 不小于 b"
fi

if [ ${a} -le ${b} ]
then
   echo "${a} -le ${b}: a 小于或等于 b"
else
   echo "${a} -le ${b}: a 大于 b"
fi
```

**布尔运算符**

| 运算符 | 说明                                                |
| ------ | --------------------------------------------------- |
| !      | 非运算，表达式为 true 则返回 false，否则返回 true。 |
| -o     | 或运算，有一个表达式为 true 则返回 true。           |
| -a     | 与运算，两个表达式都为 true 才返回 true             |

```shell
a=10
b=20
echo "a=${a}"
echo "b=${b}"

echo "\n================= ↓↓↓↓↓↓ 布尔运算符 ↓↓↓↓↓↓ ================="

if [ ${a} != ${b} ]
then
   echo "${a} != ${b} : a 不等于 b"
else
   echo "${a} == ${b}: a 等于 b"
fi

if [ ${a} -lt 100 -a ${b} -gt 15 ]
then
   echo "${a} 小于 100 且 ${b} 大于 15 : 返回 true"
else
   echo "${a} 小于 100 且 ${b} 大于 15 : 返回 false"
fi

if [ ${a} -lt 100 -o ${b} -gt 100 ]
then
   echo "${a} 小于 100 或 ${b} 大于 100 : 返回 true"
else
   echo "${a} 小于 100 或 ${b} 大于 100 : 返回 false"
fi

if [ ${a} -lt 5 -o ${b} -gt 100 ]
then
   echo "${a} 小于 5 或 ${b} 大于 100 : 返回 true"
else
   echo "${a} 小于 5 或 ${b} 大于 100 : 返回 false"
fi
```

**字符串运算符**

| 运算符 | 说明                |
| ------ | ------------------- |
| =      | 等于                |
| !=     | 不等于              |
| -z     | 字符串长度是否为0   |
| -n     | 字符串长度是否不为0 |
| $      | 字符串是否为空      |

```shell
echo "\n================= ↓↓↓↓↓↓ 字符串运算符 ↓↓↓↓↓↓ ================="

a="abc"
b="efg"
echo "a=${a}"
echo "b=${b}"

if [ ${a} = ${b} ]
then
   echo "${a} = ${b} : a 等于 b"
else
   echo "${a} = ${b}: a 不等于 b"
fi

if [ ${a} != ${b} ]
then
   echo "${a} != ${b} : a 不等于 b"
else
   echo "${a} != ${b}: a 等于 b"
fi

if [ -z ${a} ]
then
   echo "-z ${a} : 字符串长度为 0"
else
   echo "-z ${a} : 字符串长度不为 0"
fi

if [ -n "${a}" ]
then
   echo "-n ${a} : 字符串长度不为 0"
else
   echo "-n ${a} : 字符串长度为 0"
fi

if [ ${a} ]
then
   echo "${a} : 字符串不为空"
else
   echo "${a} : 字符串为空"
fi
```

**文件测试运算符**

| 操作符  | 说明                                         |
| ------- | -------------------------------------------- |
| -b file | 是否是块设备文件                             |
| -c file | 是否是字符设备文件                           |
| -d file | 是否是目录                                   |
| -f file | 是否是普通文件（既不是目录，也不是设备文件） |
| -g file | 是否设置了 SGID 位                           |
| -k file | 是否设置了粘着位(Sticky Bit)                 |
| -p file | 是否是有名管道                               |
| -u file | 是否设置了 SUID 位                           |
| -r file | 是否可读                                     |
| -w file | 是否可写                                     |
| -x file | 是否可执行                                   |
| -s file | 是否不为空（文件大小是否大于0）              |
| -e file | （包括目录）是否存在                         |

```shell
echo "\n================= ↓↓↓↓↓↓ 文件测试运算符 ↓↓↓↓↓↓ ================="

file="./test.sh"
echo "文件路径: ${file}"

if [ -r ${file} ]
then
   echo "文件可读"
else
   echo "文件不可读"
fi

if [ -w ${file} ]
then
   echo "文件可写"
else
   echo "文件不可写"
fi

if [ -x ${file} ]
then
   echo "文件可执行"
else
   echo "文件不可执行"
fi

if [ -f ${file} ]
then
   echo "文件为普通文件"
else
   echo "文件为特殊文件"
fi

if [ -d ${file} ]
then
   echo "文件是个目录"
else
   echo "文件不是个目录"
fi

if [ -s ${file} ]
then
   echo "文件不为空"
else
   echo "文件为空"
fi

if [ -e ${file} ]
then
   echo "文件存在"
else
   echo "文件不存在"
fi
```

**逻辑运算符**

| 运算符 | 说明       |
| ------ | ---------- |
| &&     | 逻辑的 AND |
| \|\|   | 逻辑的 OR  |

```shell
echo "\n================= ↓↓↓↓↓↓ 逻辑运算符 ↓↓↓↓↓↓ ================="

if [[ ${a} -lt 100 && ${b} -gt 100 ]]
then
   echo "${a} 小于 100 并且 ${b} 大于 100 : 返回 true"
else
   echo "${a} 小于 100 并且 ${b} 大于 100 : 返回 false"
fi

if [[ ${a} -lt 100 || ${b} -gt 100 ]]
then
   echo "${a} 小于 100 或者 ${b} 大于 100 : 返回 true"
else
   echo "${a} 小于 100 或者 ${b} 大于 100 : 返回 false"
fi
```

### 2.8 函数

```shell
# shell函数

# 定义函数
function hi() {
    echo "hello world!"
}

# 执行函数
hi

getNum(){
    echo "这是一个有返回值的函数"
    return 6
}
getNum
# $?获取函数返回值
echo "result: $?"


# 传参给函数
printNum(){
  echo "第1个参数： ${1}"
  echo "第2个参数： ${2}"
}
printNum 66 888
```

### 2.9 条件语句

```shell
echo "\n================= ↓↓↓↓↓↓ if-else ↓↓↓↓↓↓ ================="

a=10
b=20

if [ $a == $b ]
then
   echo "a 等于 b"
elif [ $a -gt $b ]
then
   echo "a 大于 b"
elif [ $a -lt $b ]
then
   echo "a 小于 b"
else
   echo "没有符合的条件"
fi

# 或
if [ $a == $b ]; then echo "a = b"; else echo "a != b"; fi


echo "\n================= ↓↓↓↓↓↓ case ... esac ↓↓↓↓↓↓ ================="

echo '请输入1到4之间的数字:'
echo '你输入的数字为:'
# 输入
read num
case ${num} in
    1)  echo '你选择了 1'
    ;;
    2)  echo '你选择了 2'
    ;;
    3)  echo '你选择了 3'
    ;;
    4)  echo '你选择了 4'
    ;;
    *)  echo '你没有输入1到4之间的数字！'
    ;;
esac
```

![image-20221208114304352](https://springcloud-hrm-miao.oss-cn-beijing.aliyuncs.com/markdown/202212081143424.png)

### 3.0 循环语句

```shell
echo "\n================= ↓↓↓↓↓↓ for循环 ↓↓↓↓↓↓ ================="

for receive_num in 1 2 3 4 5
do
    echo "value: $receive_num"
done


app_name_list='demo,test,tool'
# 拆分以英文逗号分隔的字符串，循环打印
for app_name in ${app_name_list//,/ }; do
    echo "app_name: [${app_name}]"
done



for receive_num in 1 2 3 4 5; do echo "* value: $receive_num"; done;

echo "\n================= ↓↓↓↓↓↓ while循环 ↓↓↓↓↓↓ ================="

int=1
while(( $int<=3 ))
do
    echo $int
    let "int++"
done


echo "\n================= ↓↓↓↓↓↓ 无限循环 ↓↓↓↓↓↓ ================="

#   # 示例1
#   for (( ; ; ))
#
#   # 示例2
#   while :
#   do
#       command
#   done
#
#   # 示例3
#   while true
#   do
#       command
#   done


echo "\n================= ↓↓↓↓↓↓ until循环 ↓↓↓↓↓↓ ================="

a=0
until [ ! $a -lt 3 ]
do
   echo $a
   a=`expr $a + 1`
done


echo "\n================= ↓↓↓↓↓↓ 跳出循环-break ↓↓↓↓↓↓ ================="
# break:跳出最外层循环
while :
do
    echo -n "输入1到5之间的数字:"
    read receive_num
    case $receive_num in
        1|2|3|4|5) echo "你输入的数字为 $receive_num!"
        ;;
        *) echo "你输入的数字不是1到5之间的! 游戏结束"
            break
        ;;
    esac
done


echo "\n================= ↓↓↓↓↓↓ 跳出循环-continue ↓↓↓↓↓↓ ================="
# continue:结束当前循环
while :
do
    echo -n "输入1到5之间的数字: "
    read receive_num
    case $receive_num in
        1|2|3|4|5) echo "你输入的数字为 $receive_num!"
        ;;
        *) echo "你输入的数字不是1到5之间的!"
            continue
            echo "游戏结束"
        ;;
    esac
done
```

