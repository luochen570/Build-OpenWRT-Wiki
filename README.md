# 编译wiki（垃圾废物天天搞不好这些）

-建议使用ubuntu或者debian进行编译（arch也可以但是依赖不一样arch也有教程但是要自己搜）

-更新软件列表
```yaml
sudo apt update -y
```
#全面更新系统
```yaml
sudo apt full-upgrade -y
```
#安装所需依赖
```yaml
sudo apt install -y ack antlr3 asciidoc autoconf automake autopoint binutils bison build-essential \
bzip2 ccache cmake cpio curl device-tree-compiler fastjar flex gawk gettext gcc-multilib g++-multilib \
git gperf haveged help2man intltool libc6-dev-i386 libelf-dev libglib2.0-dev libgmp3-dev libltdl-dev \
libmpc-dev libmpfr-dev libncurses5-dev libncursesw5-dev libreadline-dev libssl-dev libtool lrzsz \
mkisofs msmtp nano ninja-build p7zip p7zip-full patch pkgconf python2.7 python3 python3-pip qemu-utils \
rsync scons squashfs-tools subversion swig texinfo uglifyjs upx-ucl unzip vim wget xmlto xxd zlib1g-dev
```

#本次拉取lean大源码
#拉取lean-Openwrt 源码
#https://github.com/coolsnowwolf/lede直接下载
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

#进入编译文件夹（只要位置对了就可以）
cd ~/lede


#添加部分插件源-可以选择添加feeds源或者直接git拉源代码（可选建议选上这一步）
#添加 feeds源
#一键命令
sed -i '$a src-git kenzo https://github.com/kenzok8/openwrt-packages' feeds.conf.default
sed -i '$a src-git small https://github.com/kenzok8/small' feeds.conf.default

#编辑文件 feeds.conf.default 文件在源码根目录中，使用自己喜欢的文本编辑器编辑（vim nano）添加一下几行
sudo nano feeds.conf.default
#通常这两个就够了
src-git kenzo https://github.com/kenzok8/openwrt-packages
src-git small https://github.com/kenzok8/small 

#src-git helloworld https://github.com/fw876/helloworld
#src-git passwall https://github.com/xiaorouji/openwrt-passwall
#src-git lienol https://github.com/Lienol/openwrt-package
#src-git opentopd  https://github.com/sirpdboy/sirpdboy-package

#也可以选择拉取插件源码
#git拉取Hello World源码
cd lede/package/lean/ 
#git lua-maxminddb 依赖 
git clone https://github.com/jerrykuku/lua-maxminddb.git
git clone https://github.com/jerrykuku/luci-app-vssr.git  


#更新feeds文件
./scripts/feeds update -a
./scripts/feeds install -a


#制作配置文件
#插件翻译Luci - Applications
#https://www.right.com.cn/forum/thread-344825-1-1.html
#保存时不用重命名我是个哈皮
make menuconfig


#下载 dll库，编译固件 （-j 后面是线程数，取决于cpu线程数）
#切记全局代理(因为服务器在国外)
make download -j8
#开始编译文件，多线程会更快哦（-j 后面是线程数，第一次编译推荐用单线程）
make V=s -j1

#编译完成后输出路径：bin/targets


#二次编译：
cd lede
git pull
./scripts/feeds update -a 
./scripts/feeds install -a
make defconfig
make download -j8
make V=s -j$(nproc)


#如果需要重新配置：
rm -rf ./tmp && rm -rf .config #清理缓存和配置文件
make menuconfig
make V=s -j$(nproc)


GitHub 在线编译

参考：https://p3terx.com/archives/build-openwrt-with-github-actions.html

#编译openwrt基础教程
https://kmeer.cn/7.html

本教程由luochen570编写
QQ：3424015617
