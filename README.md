# AdvancedBash

- [1 分支选择](#分支选择)  
- [2 IF语句](#IF语句)  
    - [2.1 文件测试](#文件测试)
    - [2.2 整数比较](#整数比较)
    - [2.3 字符串比较](#2.3字符串比较)
- [3 数字常量](#数字常量)  
- [4 循环与分支](#4循环与分支)  

## 分支选择

```bash
case "$variable" in  
    abc) echo "\$variable = abc";;
    xyz) echo "\$variable = xyz";;
esac
```
## IF语句

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

### 文件测试
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

### 整数比较
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

### 2.3字符串比较
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

## 数字常量
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
## 4循环与分支
