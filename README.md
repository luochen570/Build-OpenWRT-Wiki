
## 欢迎来到luochen570的OpenWRT编译指南

建议使用ubuntu或者debian进行编译

安装依赖请先更换国内软件源

更换过的的小伙伴可以跳过这一步

- 清华源
[Debian](https://mirrors.tuna.tsinghua.edu.cn/help/debian/)
[Ubuntu](https://mirrors.tuna.tsinghua.edu.cn/help/ubuntu/)

- 中科大源
[Debian](https://mirrors.ustc.edu.cn/help/debian.html)
[Ubuntu](https://mirrors.ustc.edu.cn/help/ubuntu.html)

更新软件列表
```yaml
sudo apt update -y
```
全面更新系统
```yaml
sudo apt full-upgrade -y
```
安装所需依赖
```yaml
sudo apt install -y ack antlr3 asciidoc autoconf automake autopoint binutils bison build-essential \
bzip2 ccache cmake cpio curl device-tree-compiler fastjar flex gawk gettext gcc-multilib g++-multilib \
git gperf haveged help2man intltool libc6-dev-i386 libelf-dev libglib2.0-dev libgmp3-dev libltdl-dev \
libmpc-dev libmpfr-dev libncurses5-dev libncursesw5-dev libreadline-dev libssl-dev libtool lrzsz \
mkisofs msmtp nano ninja-build p7zip p7zip-full patch pkgconf python2.7 python3 python3-pip qemu-utils \
rsync scons squashfs-tools subversion swig texinfo uglifyjs upx-ucl unzip vim wget xmlto xxd zlib1g-dev
```
- 其他系统依赖请参考[OpenWRT Wiki Prerequisites](https://openwrt.org/docs/guide-developer/toolchain/install-buildsystem)


本次拉取lean大源码

拉取lean-Openwrt 源码
```yaml
git clone https://github.com/coolsnowwolf/lede
```
- 其他源码地址
```yaml
#官方源：
git clone https://openwrt.org/openwrt/openwrt.git
#Lean 源：
git clone https://github.com/coolsnowwolf/lede
#Lienol 源：
git clone https://github.com/Lienol/openwrt
#immortalwrt 源：
git clone https://github.com/immortalwrt/immortalwrt
```

进入编译文件夹（只要位置对了就可以）
```yaml
cd ~/lede
```

- 添加部分插件源码-可以选择添加feeds源或者直接git拉源代码（可选建议选上这一步）

添加feeds源

一键命令
```yaml
sed -i '$a src-git kenzo https://github.com/kenzok8/openwrt-packages' feeds.conf.default
sed -i '$a src-git small https://github.com/kenzok8/small' feeds.conf.default
```
编辑文件——>feeds.conf.default

文件在源码根目录中，使用自己喜欢的文本编辑器编辑（vim nano）添加以下几行
```yaml
nano feeds.conf.default
```
通常前两个就够了
```yaml
src-git kenzo https://github.com/kenzok8/openwrt-packages
src-git small https://github.com/kenzok8/small 

#src-git helloworld https://github.com/fw876/helloworld
#src-git passwall https://github.com/xiaorouji/openwrt-passwall
#src-git lienol https://github.com/Lienol/openwrt-package
#src-git opentopd  https://github.com/sirpdboy/sirpdboy-package
```

- 也可以选择拉取插件源码
- 示例：git拉取Hello World源码
```yaml
cd lede/package/lean/  
git clone https://github.com/jerrykuku/lua-maxminddb.git  #git lua-maxminddb 依赖
git clone https://github.com/jerrykuku/luci-app-vssr.git  

```

更新feeds文件
```yaml
./scripts/feeds update -a
./scripts/feeds install -a
```

制作配置文件
- [插件翻译Luci - Applications](https://www.right.com.cn/forum/thread-344825-1-1.html) By xtwz

```yaml
make menuconfig
```

下载 dll库，编译固件 （-j 后面是线程数，取决于cpu线程数）
## 切记全局代理(因为服务器在国外)
```yaml
make download -j8
```
开始编译文件，多线程会更快哦（-j 后面是线程数，第一次编译推荐用单线程）

最大线程数不要超过cpu线程数的2倍
```yaml
make V=s -j1
```
- 编译完成后输出路径：bin/targets


## 二次编译：
```yaml
cd lede
git pull
./scripts/feeds update -a 
./scripts/feeds install -a
make menuconfig
make download -j8
make V=s -j$(nproc)
```

## 如果需要重新配置：
```yaml
rm -rf ./tmp && rm -rf .config #清理缓存和配置文件
make menuconfig
make V=s -j$(nproc)
```

## 友情链接
+ [GitHub 在线编译](https://p3terx.com/archives/build-openwrt-with-github-actions.html)  By P3TERX
+ [编译openwrt基础教程](https://kmeer.cn/7.html)  By 年年

## 本教程由luochen570编写若转载引用请说明出处
