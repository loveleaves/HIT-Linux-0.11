# 报错解决

## 0-安装环境

``` ssh
# error
./gdb: error while loading shared libraries: libncurses.so.5: cannot open shared object file: No such file or directory
#solution
sudo apt-get install libexpat1-dev:i386
sudo ln -s /usr/lib/vmware-tools/lib32/libexpat.so.0/libexpat.so.0 /usr/lib/libexpat.so.1
sudo apt-get install lib32ncurses5
```



## 1-Boot

