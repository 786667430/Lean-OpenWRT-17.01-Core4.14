欢迎来到Lean的Openwrt源码仓库！
=
Welcome to Lean's  git source of OpenWrt and packages
forked from coolsnowwolf/openwrt
=
中文：如何编译自己需要的 OpenWrt 固件
-
注意：
-
1. **不**要用 **root** 用户 git 和编译！！！
2. 国内用户编译前最好准备好梯子
3. 默认登陆IP 192.168.1.1, 密码 password

编译命令如下:
-
1. 首先装好 Ubuntu 64bit，推荐 Ubuntu 18 LTS x64 或者 Ubuntu 20 LTS x64 ，此两个系统测试编译通过

2. 命令行输入 `sudo apt-get update` ，然后输入
`
sudo apt-get -y install build-essential asciidoc binutils bzip2 gawk gettext git libncurses5-dev libz-dev patch python3.5 python2.7 unzip zlib1g-dev lib32gcc1 libc6-dev-i386 subversion flex uglifyjs git-core gcc-multilib p7zip p7zip-full msmtp libssl-dev texinfo libglib2.0-dev xmlto qemu-utils upx libelf-dev autoconf automake libtool autopoint device-tree-compiler g++-multilib antlr3 gperf wget
`

3. 使用 `git clone https://github.com/coolsnowwolf/openwrt` 命令下载好源代码，然后 `cd openwrt` 进入目录

4. ```bash
   ./scripts/feeds update -a
   ./scripts/feeds install -a
   make menuconfig
   ```

5. `make -j8 download V=s` 下载dl库（国内请尽量全局科学上网）


6. 输入 `make -j1 V=s` （-j1 后面是线程数。第一次编译推荐用单线程）即可开始编译你要的固件了。

本套代码保证肯定可以编译成功。里面包括了 R20 所有源代码，包括 IPK 的。

你可以自由使用，但源码编译二次发布请注明我的 GitHub 仓库链接。谢谢合作！
=

二次编译：
```bash
cd openwrt
git pull
./scripts/feeds update -a && ./scripts/feeds install -a
make defconfig
make -j4 download
make -j4 V=s
#make -j$(($(nproc) + 1)) V=s #这里j后面参数建议为实际CPU线程数1~2倍。
```

如果需要重新配置：
```bash
rm -rf ./tmp && rm -rf .config
make menuconfig
make -j4 V=s
#make -j$(($(nproc) + 1)) V=s 
```

编译完成后输出路径：/openwrt/bin/targets

错误修正记录（2022-9-8 Written By LYH&LLC）：
修复[Package * is missing dependencies for the following libraries: libcap.so.2 问题](https://github.com/coolsnowwolf/lede/issues/9670)

其中目前已知需要用到的包有：samba4-libs、ip-full、tc，如有其他请自己修改源码补充。
此次编译错误，主要是因为系统libcap更新以后，出现兼容性问题，其实源码中已经自带一个，需要修改相应的Makefile，[参考链接](https://github.com/coolsnowwolf/openwrt/commit/867914c8abe2ff6eca796a8b804fa8cf48aaa50c)。
修改方法如下：
```bash
vi package/network/utils/iproute2/Makefile
/Package/ip-full
# Package/ip-full和Package/tc(52行和56行)添加上+libcap ，按ESC，ZZ保存退出。
make menuconfig —> libraries：选中 libcap （一般会默认选中，但需要重新保存下.config文件）
make -j4 V=s
```

特别提示：
------
1.源代码中绝不含任何后门和可以监控或者劫持你的 HTTPS 的闭源软件，SSL 安全是互联网最后的壁垒。安全干净才是固件应该做到的；

2.如有技术问题需要讨论，欢迎加入 QQ 讨论群：OP共享技术交流群 ,号码 297253733 ，加群链接: 点击链接加入群聊【OP共享技术交流群】：[点击加入](https://jq.qq.com/?_wv=1027&k=5yCRuXL "OP共享技术交流群")

3.想学习OpenWrt开发，但是摸不着门道？自学没毅力？基础太差？怕太难学不会？跟着佐大学OpenWrt开发入门培训班助你能学有所成
报名地址：[点击报名](http://forgotfun.org/2018/04/openwrt-training-2018.html "报名")

## 软路由介绍
友情推荐不恰饭：如果你在寻找一个低功耗小体积性能不错的 x86/x64 路由器，我个人建议可以考虑 
小马v1 的铝合金版本 (N3710 4千兆)：[页面介绍](https://item.taobao.com/item.htm?spm=a230r.1.14.20.144c763fRkK0VZ&id=561126544764 " 小马v1 的铝合金版本")

![xm1](doc/xm5.jpg)
![xm2](doc/xm6.jpg)

## Donate

如果你觉得此项目对你有帮助，可以捐助我们，以鼓励项目能持续发展，更加完善

### Alipay 支付宝

![alipay](doc/alipay_donate.jpg)

### Wechat 微信

![wechat](doc/wechat_donate.jpg)

------

English Version: How to make your Openwrt firmware.
-
Note:
--
1. DO **NOT** USE **ROOT** USER TO CONFIGURE!!!

2. Login IP is 192.168.1.1 and login password is "password".

Let's start!
---
First, you need a computer with a linux system. It's better to use Ubuntu 18 LTS 64-bit.

Next you need gcc, binutils, bzip2, flex, python3.5+, perl, make, find, grep, diff, unzip, gawk, getopt, subversion, libz-dev and libc headers installed.

To install these program, please login root users and type
`
sudo apt-get -y install build-essential asciidoc binutils bzip2 gawk gettext git libncurses5-dev libz-dev patch python3.5 unzip zlib1g-dev lib32gcc1 libc6-dev-i386 subversion flex uglifyjs git-core gcc-multilib p7zip p7zip-full msmtp libssl-dev texinfo libglib2.0-dev xmlto qemu-utils upx libelf-dev autoconf automake libtool autopoint device-tree-compiler g++-multilib
`
in terminal

Third, logout of root users. And type this `git clone https://github.com/coolsnowwolf/lede` in terminal to clone this source.

After these please type `cd lede` to cd into the source.

Please Run `./scripts/feeds update -a` to get all the latest package definitions
defined in `feeds.conf` / `feeds.conf.default` respectively
and `./scripts/feeds install -a` to install symlinks of all of them into
`package/feeds/` .

Please use `make menuconfig` to choose your preferred
configuration for the toolchain and firmware.

Use `make menuconfig` to configure your image.

Simply running `make` will build your firmware.
It will download all sources, build the cross-compile toolchain,
the kernel and all choosen applications.

To build your own firmware you need to have access to a Linux, BSD or MacOSX system
(case-sensitive filesystem required). Cygwin will not be supported because of
the lack of case sensitiveness in the file system.

## Note: Addition Lean's private package source code in `./package/lean` directory. Use it under GPL v3.

## GPLv3 is compatible with more licenses than GPLv2: it allows you to make combinations with code that has specific kinds of additional requirements that are not in GPLv3 itself. Section 7 has more information about this, including the list of additional requirements that are permitted.
