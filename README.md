# Bash

- [1 CASE语句](#1)  
- [2 IF语句](#2)  
    - [2.1 文件测试](#2.1)
    - [2.2 整数比较](#2.2)
    - [2.3 字符串比较](#2.3)
- [3 数字常量](#3)  
- [4 FOR语句](#4)  
- [5 WHILE语句](#5)  
- [6 UNTIL语句](#6)  
- [7 CONTINUE/BREAK语句](#7)  
- [8 常用命令](#8)  
    - [8.1 echo](#8.1)
- [9 正则表达式](#9)  

<h2 name="1">1 case语句</h2>
```bash
case "$variable" in
    $condition1" )
    command
    ;
    $condition2" )
    command
    ;
esac
```
(1)对变量使用""并不是强制的, 因为不会发生单词分割.  
(2)每句测试行, 都以右小括号)来结尾.  
(3)每个条件判断语句块都以一对分号结尾.  
<h2 name="2">2 IF语句</h2>

```bash
if grep -q User test.txt; then
    echo "test.txt contains at least one occurrence of User."
fi
#如果test.txt中存在User, 则grep返回0(linux 一般用0表示成功), 否则返回1;
#-q选项用来禁止输出;

sentences=Linux
word=inu
if echo "$sentences" | grep -q "$word"
then
    echo "$word found in $sentences"
else
    echo "$word not found in $sentences"
fi


#if空格[空格表达式空格]
if [ 0 ]; then #把0替换成1、-1结果一样
    echo "0 is true."       # 0为真.
else
    echo "0 is false."
fi 

if [ xyz ]; then
    echo "Random string is true." #任意字符串为真.
else
    echo "Random string is false."
fi

if [ ]; then
    echo "NULL is true."
else
    echo "NULL is false."   # NULL为假.
fi 

if [ $xyz ]; then
    echo "Uninitialized variable is true."
else
    echo "Uninitialized variable is false." #未初始化的变量为假
fi

xyz= #初始化了, 但是赋null值.
if [ -n "$xyz" ]; then
    echo "Null variable is true."
else
    echo "Null variable is false."  #null变量为假.
fi 

if [ "false" ]; then
    echo "\"false\" is true."
else
    echo "\"false\" is false." # "false" 为真，本质只是一个字符串
fi

if [ "$false" ]; then
    echo "\"\$false\" is true."
else
    echo "\"\$false\" is false." #为假，本质是一个未初始化的变量
fi

#elif结构
if [ condition1 ]; then
    command1
    command2
    command3
elif [ condition2 ]; then
    command4
    command5
else
    default-command
fi


if test condition-true
if [ condition-true ]
#效果相同
```
[[ ]]比[ ]结构更通用, 且 &&, ||, <, 和> 操作符能够正常存在于[[ ]]条件判断结构中, 但在[ ]结构中会报错.

<h3 name="2.1">2.1 文件测试</h3>

```bash
if [[ -e /etc/passwd ]]; then
    echo "exist passwd file"
fi
```
-e 文件存在  
-f 表示这个文件是一个一般文件(并不是目录或者设备文件)  
-s 文件大小不为零  
-d 表示这是一个目录  
-b 表示这是一个块设备(软盘, 光驱, 等等.)  
-c 表示这是一个字符设备(键盘, modem, 声卡, 等等.)  
-p 这个文件是一个管道  
-h 这是一个符号链接  
-L 这是一个符号链接  
-S 表示这是一个socket  
-t 文件(描述符)被关联到一个终端设备上  
    这个测试选项一般被用来检测脚本中的stdin([ -t 0 ]) 或者stdout([ -t 1 ])是否来自于一个终端.  
-r 文件是否具有可读权限(指的是正在运行这个测试命令的用户是否具有读权限)  
-w 文件是否具有可写权限(指的是正在运行这个测试命令的用户是否具有写权限)  
-x 文件是否具有可执行权限(指的是正在运行这个测试命令的用户是否具有可执行权限)  
-g set-group-id(sgid)标记被设置到文件或目录上  
如果目录具有sgid标记的话, 那么在这个目录下所创建的文件将属于拥有这个目录  
-O 判断你是否是文件的拥有者  
-G 文件的group-id是否与你的相同  
-N 从文件上一次被读取到现在为止, 文件是否被修改过  
f1 -nt f2 文件f1比文件f2新  
f1 -ot f2 文件f1比文件f2旧  
f1 -ef f2 文件f1和文件f2是相同文件的硬链接  
!  "非" -- 反转上边所有测试的结果(如果没给出条件, 那么返回真).  

<h3 name="2.2">2.2 整数比较</h3>
在中括号中使用  
等于        if [ "$a" -eq "$b" ]  
不等于      if [ "$a" -ne "$b" ]  
大于        if [ "$a" -gt "$b" ]  
大于等于    if [ "$a" -ge "$b" ]  
小于        if [ "$a" -lt "$b" ]  
小于等于    if [ "$a" -le "$b" ]  
在双括号中使用  
等于        if (("$a" == "$b"))  
小于        if (("$a" < "$b"))  
小于等于    if (("$a" <= "$b"))  
大于        if (("$a" > "$b"))  
大于等于    if (("$a" >= "$b"))  

<h3 name="2.3">2.3 字符串比较</h3>
等于        if [ "$a" = "$b" ]  
            if [ "$a" == "$b" ]  
不等号      if [ "$a" != "$b" ]  
小于        if [[ "$a" < "$b" ]] 按照ASCII字符进行排序   
            if [ "$a" \< "$b" ]  注意"<"使用在[ ]结构中的时候需要被转义  
大于        if [[ "$a" > "$b" ]]  
            if [ "$a" \> "$b" ]  
-z 字符串为"null", 意思就是字符串长度为零  
-n 字符串不为"null".  当-n使用在中括号中进行条件测试的时候, 必须要把字符串用双引号引用起来  

-a exp1 -a exp2 如果表达式exp1和exp2为真, 结果为真.  
-o exp1 -o exp2 如果表达式exp1或exp2为真, 结果为真.  
if [[ condition1 && condition2 ]]  
if [ "$exp1" -a "$exp2" ]  


<h2 name="3">3 数字常量</h2>
## 
默认10进制
以数字0开头8进制数;
以数字0x开头16进制数;
其他进制: BASE#NUMBER
#BASE的范围在2到64之间.

```bash
let "bin = 2#111100111001101"
echo "binary number = $bin" # 31181
```
```bash
n=1
let "n = n + 1"
(( n = n + 1 ))
let "n++"
(( n++ ))
```
Bash 不能处理浮点运算, 脚本中用bc或数学库才行

<h2 name="4">4 FOR语句</h2>

for arg in [list] #list中的参数允许包含通配符.
do
    command(s)
done

```bash
for planet in Mercury Venus Earth Mars Jupiter Saturn Uranus Neptune Pluto; do
    echo $planet # 每个行星都被单独打印在一行上.
done

for planet in "Mercury Venus Earth Mars Jupiter Saturn Uranus Neptune Pluto"
do
    echo $planet # 整个'list'都被双引号封成了一个变量.
done

for planet in "Mercury 36" "Venus 67" "Earth 93" "Mars 142" "Jupiter 483"
do
    IFS="*" 
    set -- $planet 
    echo "$1 $2"
done
#set -- "$X"就是把X的值返回给$1; 
#set -- $X  就是把X作为一个表达式的值一一返回,根据分隔符IF,把值依次赋给$1,$2,$3。如可以IFS="*"。

IFS=:
for dir in $PATH;do
    ls -ld $dir
done

for dir in $PATH; do
    if [ -z "$dir" ]; then dir=.; fi
    if ! [ -e "$dir" ]; then
        echo "$dir doesn't exist"
    elif ! [ -d "$dir" ]; then
        echo "$dir isn't a directory"
    else
        ls -ld $dir
    fi
done

for file in [jx]*
do
    rm -f $file # 删除当前目录下以"j"或"x"开头的文件.
done

#忽略in [list]部分的话, 将会使循环操作$@(从命令行传递给脚本的位置参数)
for a; do
    echo "$a "
done

#也可以使用命令替换 来产生for循环的[list].
NUMBERS="9 7 3 8 37.53"
for number in `echo $NUMBERS`; do
    echo -n "$number "
done

directory=/usr/bin/
fstring="Free Software Foundation"
for file in $( find $directory -type f -name '*' | sort ); do
    strings -f $file | grep "$fstring" | sed -e "s%$directory%%"
done
# 在"sed"表达式中,
# 我们必须替换掉正常的替换分隔符"/",
# 因为"/"碰巧是我们需要过滤的字符串之一.
# 如果不用"%"代替"/"作为分隔符,那么这个操作将失败,并给出一个错误消息.

OUTFILE=symlinks.list # 保存符号链接文件名的文件
directory=${1-`pwd`}
for file in "$( find $directory -type l )" # -type l = 符号链接
do
    echo "$file"
done | sort >> "$OUTFILE"

LIMIT=10
for ((a=1, b=1; a <= LIMIT ; a++, b++))
do
    echo -n "$a-$b "
done

```
<h2 name="5">5 WHILE语句</h2>

```bash
while [condition]
do
    command...
done

LIMIT=10
a=1
while [ "$a" -le $LIMIT ]; do
    echo -n "$a "
    let "a+=1"
done

((a = 1))
while (( a <= LIMIT )); do
    echo -n "$a "
    ((a += 1))
done
```
<h2 name="6">6 UNTIL语句</h2>
这个结构在循环的顶部判断条件, 并且如果条件一直为false, 那么就一直循环下去. (与while循环相反).  

```bash
until [condition-is-true]
do
    command...
done
```

<h2 name="7">7 CONTINUE/BREAK语句</h2>

```bash
LIMIT=19 # 上限
a=0
while [ $a -le "$LIMIT" ];do
    a=$(($a+1))
    if [ "$a" -eq 3 ] || [ "$a" -eq 11 ]; then
        continue
        #break
    fi
done
```
break N可以退出N层循环,不带参数的break命令只能退出最内层的循环.  
continue N 同理，但很少用.  

<h2 name="8">8 常用命令</h2>

<h3 name="8.1">8.1 echo</h3>
echo 打印一个表达式或变量,需要使用
-e 来打印转义字符; 
每个 echo 命令都会在终端上新起一行,但是-n 选项将会阻止新起一行;
echo 命令可以用来作为一系列命令的管道输入;

```bash
if echo "$VAR" | grep -q txt
then
    echo "$VAR contains the substring sequence \"txt\""
fi

```
echo `command`将会删除任何由命令产生的换行符;

```bash
ls -l /usr/share/apps/kjezz/sounds
echo `ls -l /usr/share/apps/kjezz/sounds`

```

<h2 name="9">9 正则表达式</h2>
