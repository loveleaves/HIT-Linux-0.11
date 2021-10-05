# hit-oslab
该目录下涵盖了操作系统实验所需的实验环境配置脚本，以及linux-0.11源码。针对Ubuntu系统，一键式搭建好实验环境，节省环境配置时间。

## 非原著声明		
本安装脚本setup.sh主要修改自github仓库[DeathKing/hit-oslab](https://github.com/DeathKing/hit-oslab)，适用于ubuntu16.04 amd64，不适用于WSL（额外需安装32框架包等）。
## 安装
setup.sh脚本会将实验环境安装在用户home目录下，目录名为oslab。如果有特殊需要，请自行移动文件夹位置。注意，**请不要使用超级用户执行此安装命令**，当有需要时会请求超级用户权限。		
```sh
git clone https://github.com/loveleaves/HIT-Linux-0.11.git ~/hit-oslab-loveleaves
cd ~/hit-oslab-loveleaves/0-preEnv/hit-oslab
./setup.sh
```
## 复原		
考虑到操作系统实验每次需要重置linux-0.11目录，特别添加了重置功能。本功能由./run命令提供。
```sh
# in oslab directory ./oslab
./run init
```
