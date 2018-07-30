1. 查看显卡的使用情况  
$ nvidia-smi
2. 从零开始训练  
./darknet detector train cfg/voc.data cfg/yolov3-voc.cfg darknet53.conv.74 
3. 断点继续训练  
./darknet detector train cfg/voc.data cfg/yolov3-voc.cfg backup/voc/yolov3-voc.backup -gpu 0

4. 查看文件夹的空间  
df -h
4.1 列出目录下文件的详细信息  
ll  
5. 回到账号的home  
cd -
6. 多GPU训练  
./darknet detector train cfg/voc.data cfg/yolov3-voc.cfg backup/voc/yolov3-voc.backup -gpus 0,1,2,3
6.1 【2>&1 | tee paul_train_log.txt】存储了训练日志  
./darknet detector train cfg/voc_my_cfg.data cfg/yolov3-voc.cfg backup/yolov3-voc_97000.weights -gpus 0,1,2,3 2>&1 | tee paul_train_log.txt

7. 测试自己训练的权重  
./darknet detect cfg/yolov3-voc.cfg backup/voc/yolov3-voc_final.weights data/person.jpg

8. 建立链接  
sudo ln -s 地址 .

9. 修改某个文件夹的权限，使其能被  
sudo chmod -R 777 caffe/

10. more 浏览某个文件 esc退出  

11. rm -rf 文件夹   
迭代移除文件夹
11.1 重命名一个文件  
mv <src> <der>

12. 修改密码  
1、sudo su转为root用户  
2、sudo passwd user(user 是对应的用户名)   

13. 查找位于当前路径下包含某变量的文件，具体到某个文件的第几行
grep -rnI il_in_ .  
grep -rnI ConvolutionRistretto .  
grep -rnI REGISTER_LAYER_CLASS .  

14. 查看git的版本并初始化  
git log  
git init  
14.1. git中加入文件  
```
git add *.cpp  
git add *.c  
git add *.h  
```
15. 显示文件状态（查看哪些文件被add到git中了），也用于找到有改动的文件  
git status  

16. 提交新版本  
git commit -s  
git commit -m <message>
 
17. 邮箱和账户名   
git config --global user.email "chensuyue@hik.com"  
git config --global user.name "chensuyue"  

18. 找到某个文件中修改的细节    
git diff [file]    

19. 回到上一个版本  
git checkout [file]  

20. 查看电脑资源使用情况  
htop  如果没有就安装 sudo apt-get install htop  

21. 查看两个文件之间的区别  
vimdiff <file> <file>

22. 将日志和错误同时输出到文本和命令行  
2>&1 | tee models/SqueezeNet/log/funetune_log.txt
2是标准错误，&1是标准输出，2>&1意思就是将标准错误输出到标准输出中。  
如果没有2>&1,只会有标准输出，没有错误，tee的作用同时输出到控制台和文件。

23. cat主要有三大功能  
- 一次显示整个文件。  
$ cat   filename
- 从键盘创建一个文件。
$ cat  >  filename
只能创建新文件,不能编辑已有文件.   
- 将几个文件合并为一个文件。  
$cat   file1   file2  > file  

24. chomd 修改权限  
chmod 777 <file>  # 可读可写可执行 等同于+rwx  
7 = 4 + 2 + 1， 4 可读； 6 可读可写； 7 可执行；  
每一文件或目录的访问权限都有三组，每组用三位表示。（以上每个数字代表一组 7 可以改为 +rwx ； 777就是 a+rwx）    
分别为文件属主的读、写和执行权限；与属主同组的用户的读、写和执行权限；系统中其他用户的读、写和执行权限。  
u 表示“用户（user）”，即文件或目录的所有者。    
g 表示“同组（group）用户”，即与文件属主有相同组ID的所有用户。   
o 表示“其他（others）用户”。    
a 表示“所有（all）用户”。它是系统默认值。   

25. 压缩文件  
压缩当前的文件夹 zip -r ./xahot.zip ./* -r表示递归  
zip [参数] [打包后的文件名] [打包的目录路径]  
解压 unzip xahot.zip  
  
tar -zcvf /home/xahot.tar.gz /xahot  
tar -zcvf 打包后生成的文件名全路径 要打包的目录  
例子：把/xahot文件夹打包后生成一个/home/xahot.tar.gz的文件。  
tar -xf all.tar 这条命令是解出all.tar包中所有文件，-x是解开的意思。  
