---
layout: post
title: sublime-全能小巧的IDE
category: experience
---
##sublime将tab改成默认2个space##
之前因为习惯了tab是两个space，当安装好sublime看到tab是四个空格，觉得十分的别扭（也许是我强迫症吧！）不改不舒服斯基，额。

sublime里面打开Preferences>>Settings-Default

你会看到它帮你打开了一个叫Preferences.sublime-settings的文本,位置在
	
	C:\Users\<user>\AppData\Roaming\Sublime Text 2\Packages\Default\

然后，把tab_size改成你想要的spaces数量。
##Sublime Text增加Build system类型，打造一个全能IDE##
Sublime text2是一款非常方便的文本编辑器，现在我基本上不用IDE去编写代码，一般都是在Sublime text2中编辑，当然，这里无法执行、debug是软肋，于是上网找了下资料，可以把添加Sublime text的build类型

比如添加php的执行模块（首先，你要安装php :) ）

* Tools > build system  >   new build system
* json 配置如下

	{
	"cmd": ["C:/wamp/bin/php/php5.4.3/php.exe","$file"],
	"file_regex": "^.+ in (.+) on line ([0-9]+)",
  "selector": "source.php"
	}

一般我会把它保存成<code>php.sublime-build</code>文件存放在C:\Users\<code>[user]</code>\AppData\Roaming\Sublime Text 2\Packages\php路径下。其实这个路径是Preferences > Browse Packages就能查看的到的。它是sublime的AppDomain路径。

然后就可以Ctrl+B执行PHP代码了！！！

那么其他的解释执行的语言模块应该也可以类比进行配置了。