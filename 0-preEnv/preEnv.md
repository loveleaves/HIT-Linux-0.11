# Linux-0.11实验环境准备

**适用系统**：ubuntu 16.04 amd64


如果是ubuntu 32位系统(i386)或者是ubuntu version <= 14.04的64位系统，可以直接移步这个[快速配置教程](https://github.com/DeathKing/hit-oslab)，但是**64位系统按上面提到的照配置教程来会缺少诸如dbg-asm dbg-c等调试脚本命令**。并且ubuntu 16.04 64位按该配置快速配置教程来，在我的机器上有问题，编译之后的linux-0.11系统不能正常启动。所以我对该配置教程进行了改写，具体见以下内容。

## 我的16.04 amd64快速配置脚本		
前几天，自学了下shell脚本的相关知识，参考上面[快速配置教程](https://github.com/DeathKing/hit-oslab)的setup.sh脚本，以及对应的材料，自己改写了这个setup.sh脚本，以及对应的hit-oslab-linux-20110823.tar.gz材料，并重新打包为hit-oslab-linux-20110823.tar.gz，并将所需材料以及配置脚本setup.sh上传至我的github[配置实验环境](./prepEnv)，经过测试，在我64位16.04的ubuntu中可以完成设置。		

## 实验材料
1. hit-oslab-linux-20110823.tar.gz
	包含linux-0.11源码，bochs虚拟机等
2. gcc-3.4-ubuntu.tar.gz
	编译linux-0.11需要用到的低版本的gcc

## 安装
1. 克隆该仓库
	```bash
	git clone https://github.com/loveleaves/HIT-Linux-0.11.git ~/hit-oslab-loveleaves
	```
2. 进入到环境配置文件夹
	```bash
	cd ~/hit-oslab-loveleaves/0-preEnv/hit-oslab
	```

3. 执行脚本。     
本安装脚本会将实验环境安装在当前登录用户的*HOME*目录下，文件名为*oslab*。如果有特殊需要，请自行移动文件夹位置。注意，请不要使用超级用户执行此命令，当有需要时该脚本会请求超级用户权限。
	
	```bash
	./setup.sh
	```

## 复原
考虑到操作系统实验每次需要重置linux-0.11目录，添加了重置linux-0.11目录下所有文件的功能。本命令由`./run`命令提供。
 ```bash
 ./run init
 ```

## setup.sh具体操作细节(不关心也可以)
1.	解压hit-oslab-linux		
	在终端执行
	```sh
	tar zxvf hit-oslab-linux-20110823.tar.gz
	```
2.	安装gcc-3.4		
	在终端执行		
	```sh
	cp gcc-3.4-ubuntu.tar.gz /tmp
	cd /tmp		
	tar zxvf gcc-3.4-ubuntu.tar.gz		
	cd gcc-4.3		
	sudo ./inst.sh amd64		
	```
3.	安装as86 ld86		
	as86 ld86用于编译和链接linux-0.11/boot下的bootsect.s和setup.s，它们采用as86汇编语法；而linux-0.11下的其他汇编文件采用gas的语法AT&		
	搜索包含as86 ld86的包：		
	```sh
	apt-cache search as86 ld86
	```
	执行结果：				
	```sh
	bin86 - 16-bit x86 assembler and loader
	```
	安装bin86：				
	```sh
	sudo apt-get install bin86
	```
4.	64位系统需要安装32位兼容库				
	```sh
	sudo apt-get install libc6-dev-i386
	```
5.	C语言编译环境		
	```sh
	sudo apt-get install build-essential
	```
6.	编译内核
	
	```sh
	cd oslab/linux-0.11
	make
	```

7.	运行linux-0.11
	
	```sh
	./run
	```

8.	出现错误
	
	```sh
	./bochs/bochs-gdb: error while loading shared libraries: libSM.so.6: cannot open shared object file: No such file or directory
	```

9.	打印动态链接配置
	
	```sh
	ldconfig -p | grep libSM.so.6
	```

10.	libSM.so.6的链接信息		
	
	```sh
	libSM.so.6 (libc6,x86-64) => /usr/lib/x86_64-linux-gnu/libSM.so.6
	```

11.	我们需要的是32位的。搜索库对应的名称		
	
	```sh
	apt-file search libSM.so.6
	```

12.	打印结果		
	
	```sh
	libsm6: /usr/lib/x86_64-linux-gnu/libSM.so.6
	libsm6: /usr/lib/x86_64-linux-gnu/libSM.so.6.0.1
	```

13.	得到其对应的包名为libsm6，安装对应的32位库		
	
	```sh
	sudo apt-get install libsm6:i386
	```

14.	再次尝试，出现错误		
	```sh
	./bochs/bochs-gdb: error while loading shared libraries: libX11.so.6: cannot open shared object file: No such file or directory
	```
15.	同样可以按照步骤11来解决，不过这里用了另一个工具
	```sh
	dpkg-query -S libX11.so.6
	```
16.	打印结果		
	```sh
	libx11-6:i386: /usr/lib/i386-linux-gnu/libX11.so.6.3.0
	libx11-6:amd64: /usr/lib/x86_64-linux-gnu/libX11.so.6
	libx11-6:amd64: /usr/lib/x86_64-linux-gnu/libX11.so.6.3.0
	libx11-6:i386: /usr/lib/i386-linux-gnu/libX11.so.6
    ```
17.	得到其对应的包名为libx11-6，安装对应的32位库		
	```sh
	sudo apt-get install libx11-6:i386
	```
18.	再次尝试，出现错误		
	```sh
	./bochs/bochs-gdb: error while loading shared libraries: libXpm.so.4: cannot open shared object file: No such file or directory
	```
19.	继续使用步骤15的工具		
	```sh
	dpkg-query -S libXpm.so.4
	```
20.	打印结果		
	```sh
	libxpm4:i386: /usr/lib/i386-linux-gnu/libXpm.so.4
	libxpm4:amd64: /usr/lib/x86_64-linux-gnu/libXpm.so.4
	libxpm4:amd64: /usr/lib/x86_64-linux-gnu/libXpm.so.4.11.0
	libxpm4:i386: /usr/lib/i386-linux-gnu/libXpm.so.4.11.0
	```
21.	得到其对应的包名为libxpm4，安装对应的32位库		
	```sh 
	sudo apt-get install libxpm4:i386
	```
22.	再次尝试，运行成功！
