# Evaluate-system-for-webp
Description:
============
系统实现内容：

评估指定目录下所有png或者jpg图片转换为不同质量[50,75,95,lossness]的webp是否合理。合理性是从性能和图片size两个维度考虑。

原理：

values是兼顾图片size和图片解码时间按6:4算出的权衡值.values小于1,即是建议优化为webp图片格式.

Usage:
=========
1.首先要手动锁频锁核

	步骤：
       1). 关闭内核调度
        adb root;adb remount;
        adb shell
		echo 0 > /proc/hps/enabled　　
       2).关闭mtk频率调度
        adb root;adb remount;
        adb shell
		echo 0 > /proc/ppm/enabled
		3).写cpu是否上线
		此时i=6；将cpu0～cpu5都上线
		echo 1 >/sys/devices/system/cpu/cpu${i}/online
		4))写cpu频率
		此时i=6
		查看cpu可以写的频率
		cat /sys/devices/system/cpu/cpu{i}/cpufreq/scaling_available_frequencies
		echo $CPU_FREQ > /sys/devices/system/cpu/cpu{i}/cpufreq/scaling_min_freq
		echo $CPU_FREQ > /sys/devices/system/cpu/cpu{i}/cpufreq/scaling_max_freq

2.安装解码apk

	adb install -r decodeBitmap.apk
     
3.设置手机状态
	
	将手机自动锁屏设置为永不
	
4.运行脚步

	python Evaluwebp.py dir [sizeWeights] [Specifiedsize] [devices]

	dir： 指定的目录,此目录以及子目录下所有png或者jpg图片将可能被转换为webp，图片将以路径和name命名。
	sizeWeights： 解码时间和图片size权重，默认是4:6，sizeWeights=6。
	Specifiedsize： 图片大小超过Specifiedsize才转换webp，默认Specifiedsize=50KB，Specifiedsize单位是KB。
	devices： 是device name，只有一个终端时可以不用设置，默认是空。

	eg：python Evaluwebp.py  ~/demo/Life


FAQ:
========

1."/bin/sh: 1: cwebp: not found"

first method(cp):
```java
sudo cp cwebp /usr/local/bin/
```
second method(download the command line tools cwebp):
```java
1)download libwebp-0.6.0-linux-x86-32.tar.gz 
	https://storage.googleapis.com/downloads.webmproject.org/releases/webp/libwebp-0.6.0-linux-x86-32.tar.gz
2).tar xvzf  libwebp-0.6.0-linux-x86-32.tar.gz
3).sudo cp libwebp-0.6.0-linux-x86-32/bin/cwebp  /usr/local/bin/
```
third method(build the cwebp yourself for linux):
```java
1).sudo apt-get install libjpeg-dev libpng-dev libtiff-dev libgif-dev
2).download source,libwebp-0.6.0.tar.gz 
	https://storage.googleapis.com/downloads.webmproject.org/releases/webp/libwebp-0.6.0.tar.gz
3).Compiling the source. 
	tar xvzf libwebp-0.6.0.tar.gz
	cd libwebp-0.6.0
	./configure --enable-everything
	make
	sudo make install
```

Discuss:
========
tanhaiqing89@126.com
