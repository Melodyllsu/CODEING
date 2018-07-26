参考链接：  
https://www.cnblogs.com/yinheyi/p/6648242.html  
菜鸟教程：  


常见的编程语言分为两类：一个是编译型语言，如：c/c++/java等，它们远行前全部一起要经过编译器的编译。  
另一个解释型语言，执行时，需要使用解释器一行一行地转换为代码，如：awk, perl, python与shell等。  
Shell 经过了POSIX的标准化，所以它是可以在不同的linux系统上进行移植。

- 关于注释的问题： 在shell中使用#进行注释，注意，sh里面没有多行注释，只能每一行加一个#号；  

- echo 命令  
用于字符串的输出    
echo "It is a test"
选项：  
-n 不换行输出    
-e 启用反斜线转义解释  
-E 禁用反斜线转义解释（默认）     

$0 
当前脚本的文件名

$n 
传递给脚本或函数的参数。n 是一个数字，表示第几个参数。例如，第一个参数是$1，第二个参数是$2。

$# 
传递给脚本或函数的参数个数。

$* 
传递给脚本或函数的所有参数。

$@ 
传递给脚本或函数的所有参数。被双引号(" ")包含时，与 $* 稍有不同，下面将会讲到。

$? 
上个命令的退出状态，或函数的返回值。

$$ 
当前Shell进程ID。对于 Shell 脚本，就是这些脚本所在的进程ID。

- if语句 if -z -n -f -eq -ne -lt  
```
if  条件
then
 Command
else
 Command
fi       别忘了这个结尾
```
文件表达式  
if [ -f  file ]    如果文件存在   
if [ -d ...   ]    如果目录存在  
if [ -s file  ]    如果文件存在且非空    
if [ -r file  ]    如果文件存在且可读    
if [ -w file  ]    如果文件存在且可写    
if [ -x file  ]    如果文件存在且可执行       
-z STRING  
the length of STRING is zero  
-e FILE  
FILE exists  文件存在  

- awk 处理文本文件的语言   
awk '{[pattern] action}' {filenames}   # 行匹配语句 awk '' 只能用单引号  

```shell
log.txt文本内容如下：

2 this is a test
3 Are you like awk
This's a test
10 There are orange,apple,mongo


# 每行按空格或TAB分割，输出文本中的1、4项
 $ awk '{print $1,$4}' log.txt
 ---------------------------------------------
 2 a
 3 like
 This's
 10 orange,apple,mongo

# 格式化输出
 $ awk '{printf "%-8s %-10s\n",$1,$4}' log.txt
 ---------------------------------------------
 2        a
 3        like
 This's
 10       orange,apple,mongo
```

- sed  
sed -i '/^$/d' a.txt会将没内容的空行删除  


- 代码解读  
 ``` shell
  #!/bin/sh
  1 
  2 echo -n "log file: $1\n"  # 输出第一个参数
  3 if [ ! -e $1 ] || [ -z $1 ]   # 如果文件不存在或者第一个参数长度为零
  4 then
  5         echo -n "file do not exists or wrong input!\n"
  6         echo -n "valid usage: gen_acc_loss.sh your_log_file\n"
  7         exit 1
  8 fi
  9 
 10 echo -n "generating loss_record!\n"
 11 grep Iteration $1 | grep loss | awk '{print $13}' | sed '/^$/d' > loss_record # 找到含有Interaction和loss的行 awk输出第13个，sed删除空行（因为最后输出总loss时是没有那么长的，会存在空行）， > 写入文件
 12 grep Iteration $1 | grep loss | tail -n 1 | awk '{print $9}' >> loss_record # 将含有Interation和loss的最后一行 awk输出第9个数， 合并到loss_record文件中
 13 
 14 echo -n "generating acc_top1!\n"
 15 grep "Test net output" $1 | grep "accuracy =" | awk '{print $11}' > acc_top1
 16 #grep "Test net output" $1 | grep "top-1" | awk '{print $11}' > acc_top1
 17 
 18 echo -n "generating acc_top5!\n"
 19 grep "Test net output" $1 | grep "accuracy_top5" | awk '{print $11}' > acc_top5
 20 
 21 echo -n "Done!\n"

 ```
