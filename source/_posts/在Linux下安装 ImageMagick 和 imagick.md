---
title: ImageMagick安装和PHP imagick扩展
date: 2018-09-04 10:12:44
tags:
  - imagick
  - ImageMagick
  - PHP
categories:
  - Linux
---

## 在Linux下安装 ImageMagick 和 imagick

ImageMagick 介绍
ImageMagick 是一套功能强大、稳定而且免费的工具集和开发包，可以用来读、写和处理超过185种基本格式的图片文件，包括流行的TIFF, JPEG, GIF, PNG, PDF以及PhotoCD等格式。利用 ImageMagick 你可以根据web应用程序的需要动态生成图片, 还可以对一个（或一组）图片进行改变大小、旋转、锐化、减色或增加特效等操作，并将操作的结果以相同格式或其它格式保存。
ImageMagick 的官网：http://www.imagemagick.org
详细使用手册：http://www.imagemagick.org/Usage

imagick 介绍
一个可以供PHP调用 ImageMagick 功能的PHP扩展，使用这个扩展可以使PHP具备和ImageMagick相同的功能。
imagick 官网：http://pecl.php.net/package/imagick


### ImageMagick 安装
#### 1.Yum安装 (不推荐 会缺少解码器)
```
yum install ImageMagick
yum install ImageMagick-devel
yum install gcc gcc-c++ autoconf automake
```
卸载
```
yum remove ImageMagick
```
#### 2.编译安装 (推荐)
```
wget https://www.imagemagick.org/download/ImageMagick.tar.gz
tar zxvf ImageMagick.tar.gz
cd ImageMagick-7.0.8-11
./configure --prefix=/usr/local/ImageMagick/ --with-bzlib=yes --with-fontconfig=yes --with-freetype=yes --with-gslib=yes --with-gvc=yes --with-jpeg=yes --with-jp2=yes --with-png=yes --with-tiff=yes --enable-lzw --with-modules --with-quantum-depth=8?--enable-shared --disable-openmp

./configure --prefix=/usr/local --with-bzlib=yes --with-fontconfig=yes --with-freetype=yes --with-gslib=yes --with-gvc=yes --with-jpeg=yes --with-jp2=yes --with-png=yes --with-tiff=yes --enable-shared --disable-openmp


make && make install
```
编译安装过程时间比较长，请耐心等待！
##### ImageMagick安装说明
1. 在安装时也可以加入–without-xxx来禁止一些选项，具体的就 ./configure –help | grep without来查看有哪些吧；
2. --with-XX=yes ：选中安装文件解码
3. –enable-shared 编译成共享库；
4. –disable-openmp 禁用多线程，使用多线程性能并没有提高，但CPU占用达到了100%，所以禁用；
5. 卸载命令：make uninstall。

安装完成后执行：`/usr/local/ImageMagick/bin/convert logo: logo.gif` 测试一下 ImageMagick 是否可以正常运行，如果没有提示任何错误，然后检查执行命令时所在的目录，看看是否生成了logo.gif 这个文件。

也可以在安装完成后，运行 `convert -version` 命令检测，应该会出现类似下面内容的信息：
```
<p>Version: ImageMagick 6.8.9-7 Q8 x86_64 2014-08-20 
http://www.imagemagick.org<br>
Copyright: Copyright (C) 1999-2008 ImageMagick Studio LLC</p>
```
convert默认安装到了 `/usr/local/ImageMagick/bin` 下面，上面的命令可能提示找不到convert命令，那么可以在/usr/bin下面创建一个到/usr/local/ImageMagick/bin/convert的链接：
```
cd /usr/bin
ln -s /usr/local/ImageMagick/bin/convert convert
```

## PHP扩展imagick安装
### 1.编译安装
从http://pecl.php.net/package/imagick找到imagick的最新的版本
```
wget http://pecl.php.net/get/imagick-3.4.3.tgz
tar zxvf imagick-3.4.3.tgz
cd imagick-3.4.3
phpize
./configure --with-php-config=/php路径/php-config --with-imagick=/usr/local/ImageMagick/
make && make install
```
### 2.pecl安装
```
pecl install imagick
```
### 配置文件 `/etc/php.ini`
```
extension=imagick.so
```
查看phpinfo();


## imagick无法安装时的解决办法
有时安装imagick是会提示：
```
“configure: error: not found. Please provide a path to MagickWand-config or Wand-config program.”
```
这是因为只安装了“ImageMagick”而没有安装“ImageMagick-devel”，通过下面的命令行安装ImageMagick-devel，然后重新按上面的步骤安装imagick就好了。

```
yum install ImageMagick-devel
```
## 关闭多线程
单线程转换每张图片大概50ms，两个线程却需要500ms？
用convert –version命令查看，看是否出现openMP字样，出现的话，是因为机器不支持openMP导致的，需要重新编译`./configure –disable-openmp`，再进行安装。

### 查看文件支持
```
convert -list format | egrep 'PNG|JPEG'
```

### 文件系统解码
```
yum install libjpeg libjpeg-devel freetype-devel libpng libpng-devel libtiff libtiff-devel
```



