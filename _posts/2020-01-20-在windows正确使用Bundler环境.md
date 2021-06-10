---
layout: post
title: "Windows环境使用Bundler"
description: "如何正确的使用Bundler"
categories: [Bundler]
tags: [Bundler]
redirect_from:
  - /2020/01/20/
permalink: /:categories/:title
---

> 记录 - 网上关于这部分的文档杂乱老旧,各类安装包之间依赖关系强，没有哪一个系统整理的流程资料，
> 我将我踩坑各种失败后完成的操作流程写下，版本建议和如下一致，否则将需要你从头探索。


**第一步骤：安装ruby**
rubyinstaller-2.6.5-1-x64.exe

#rubyinstaller-2.4.9-1-x64.exe 2.4版本很多宝石不支持，最新版2.7也一样

安装目录名一定是英文，否则后面的步骤中会遇到麻烦

打开cmd
```
ruby -v
gem -v
```

**第二步骤：安装devikit**
DevKit-mingw64-64-4.7.2-20130224-1432-sfx.exe
配置：
在devkit解压的根目录执行
```
ruby dk.rb init
```
编辑_config.yml文件, 把ruby的根目录添加到最后一行
```- C:\Ruby22-x64```
```
ruby dk.rb review 
ruby dk.rb install
```
**第三步骤：安装msys2**
msys2-x86_64-20190524.exe

安装完成后先不要进入msys2shell，执行以下步骤

若到此处出现SSL文件错误，则需要下载cacert.pem

此文件放到D:\Ruby26-x64z\lib\ruby\2.4.0\rubygems\ssl_certs目录下，gem更换镜像源文件就不会报SSL错误了
到我的电脑高级设置里配置PATH 系统环境变量

gem更改镜像源
```
gem sources --remove https://rubygems.org/
gem sources --add https://gems.ruby-china.com
gem sources -l
```

MSYS2使用清华大学TUNA镜像
找到D:\msys64\etc\pacman.d目录

```
编辑 /etc/pacman.d/mirrorlist.mingw32 ，在文件开头添加：
Server = https://mirrors.tuna.tsinghua.edu.cn/msys2/mingw/i686

编辑 /etc/pacman.d/mirrorlist.mingw64 ，在文件开头添加：
Server = https://mirrors.tuna.tsinghua.edu.cn/msys2/mingw/x86_64

编辑 /etc/pacman.d/mirrorlist.msys ，在文件开头添加：
Server = https://mirrors.tuna.tsinghua.edu.cn/msys2/msys/$arch
```

打开msys2根目录下64位exe: "mingw64.exe"
然后执行 ``` pacman -Sy ``` 刷新软件包数据即可


ridk.cmd
执行```ridk install```
```MSYS2（optional）
-- MSYS2 base installation
-- MSYS2 system update (optional)
-- MSYS2 and MINGW development toolchain
```


ENTER[1,2,3]
 * 2
 * 3

由于在没有MSYS2的环境下执行base insatllation是不成功的，所以我们事先在第三步骤安装了MSYS2中包含了1

安装宝石
```
gem install jekyll bundler
jekyll -v
bundler -v
```

使用以下命令替换 bundler 默认源 bash
```bundle config mirror.https://rubygems.org https://mirrors.tuna.tsinghua.edu.cn/rubygems```

可能在安装的过程中还有一些更新操作，可以依据提示升级。
RubyGems版本可能过低
```
gem update  --system
gem -v
```



## 以下是gem常用命令
Install/Uninstall gem
```
gem install jekyll
gem uninstall jekyll
```

Install specific version of gem
```
gem install pygments.rb --version 0.4.2
```
Uninstall specific versions of gem
Prompt 'Select gem to uninstall' and let the user choose
```
gem uninstall jekyll
```
Uninstall specific version
```
gem uninstall jekyll --version 1.0.1
gem uninstall jekyll --version '<1.0.1'
```
Remove all old versions of jekyll
```
gem cleanup jekyll
```
List all local gem
```
gem list --local
```
List all versions of gem
```
gem list --all
```
List gem with specific name
```
gem list jekyll
```
Update installed gem
```
gem update
```
Update installed system gem
```
gem update --system
```

## 使用simpleture
clone texture文件，在Gemfile注释掉这行
```
#gemspec 
```
使用bundle Gemfile安装宝石，exec serve
 
