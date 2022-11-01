##### 1. [.bat] 文件（windows批处理）简述

```shell
1. .bat 是一种windows脚本文件，类似于linux中的shell。不需要编译，直接双击即可运行。
2. .bat中支持cmd命令，直接写在脚本中即可，保险起见还是要指定一下命令的路径，不然有可能找不到。
3. .bat文件语法出错后，编写时不会有提示，但是运行时会直接退出，不会留下错误信息，这点很坑，所以写完一块就要运行一下试试，确保写过的是正确的。
```

##### 2. .bat 之 变量

```shell
#注：
.bat中的变量定义和赋值都是通过set完成的，一般来讲定义变量建议使用【SET】，二次赋值建议使用【set】以做区分
.bat中不对数据类型做区分，定义后直接赋值即可

# 1. 预处理
预处理机制：批处理读取命令时是按行读取的（另外例如 for 命令等，其后用一对圆括号闭合的所有语句也当作一行），在处理之前要完成必要的预处理工作，这其中就包括对该行命令中的变量赋值。在不启用变量延迟，也不对变量动态捕获其扩展变化时，变量在预处理阶段不作改变。

# 2. 启用延迟变量【对变量动态捕获扩展变化】，有循环赋值时必须开启
setlocal enabledelayedexpansion 

# 3. 定义变量 赋值语句两端不要有空格【不然会出错】
SET variable=value

# 4. 给已有变量赋值
set variable=value_new

# 5. 变量之间的数值运算
set /a variable += 1

# 6. 普通变量使用
%variable%

# 7. 循环赋值变量使用，因为是循环赋值，所以需要动态捕获变量值
!variable!

# 8. 字符串拼接【想加什么直接拼在后面就好】
set newstr=%variable%string

# 9. 字符串截取
%variable:~0,1% # 从第0个字符开始，截取一个
%variable:~4,1% # 从倒数第4个字符开始，截取一个

# 10. 字符串替换
set variable=ABCDEA
set variable=%variable:A=F% #把A替换成F ==》FBCDEF
```

##### 3. .bat 之 for循环--文件处理

```shell
#注：更多for循环的使用请参考【https://sites.google.com/site/yao27bat/home/basic/for-2/for_f】
使用

# 1. 循环格式
for /f "skip=10" %%i in(command1) do (
	command2
)

# 2. 格式解析
for in do 是关键字
in 中包含待循环的内容，command1可以是变量，字符串，文件等等
for 后面是一个括号，包含处理逻辑，command2是对 %%i 的处理逻辑
/f 表示待循环的内容是文件，对这个文件按行遍历，遍历出的内容赋给 %%i
%%i 这里要注意一定是两个%【cmd中是一个%，但是批处理中是两个】
"skip=10" 表示 从第11行开始处理

# 3. 使用注意事项
.bat 中的 for循环没有break，如果想退出可以使用goto语句跳出，但是在for中使用goto后会使循环失效，这点要注意
```

##### 4. .bat 之 程序停止

```shell
# 1. 执行完后窗口不退出
pause

# 2. 退出程序，不再继续执行
cmd.exe exit
```

