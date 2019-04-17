---
title: ruby 遇到过的问题集锦
date: 2019-04-17 21:42:14
tags:
  - issues
categories:
  - ruby
---

# nokogiri  安装问题

**ERROR: cannot discover where libxml2 is located on your system. please make sure `pkg-config` is installed.**

报错信息：

    current directory: /Users/chenkunhui/.rvm/gems/ruby-2.5.1@toolgems/gems/nokogiri-1.8.4/ext/nokogiri
    /Users/chenkunhui/.rvm/rubies/ruby-2.5.1/bin/ruby -r ./siteconf20190327-6771-1hthttu.rb extconf.rb --use-system-libraries
    checking if the C compiler accepts ... yes
    checking if the C compiler accepts -Wno-error=unused-command-line-argument-hard-error-in-future... no
    Building nokogiri using system libraries.
    Using pkg-config gem version 1.3.7
    checking for libxml-2.0... yes
    checking for libxslt... yes
    checking for libexslt... yes
    ERROR: cannot discover where libxml2 is located on your system. please make sure `pkg-config` is installed.
    *** extconf.rb failed ***
    Could not create Makefile due to some reason, probably lack of necessary
    libraries and/or headers.  Check the mkmf.log file for more details.  You may
    need configuration options.
    
    ...


系统已经安装了 `pkg-config` 和 `libxml2`,  但却报错说找不到 `libxml2`,
解决办法是安装 gem 包时，指定 `libxml2` 的位置。

解决方案：

> 参考： https://gist.github.com/sobstel/f6a490d854a2e5a214c3f2cd9c366032

执行以下代码:

    gem install nokogiri -v '1.8.4' -- --with-xml2-include=/usr/local/Cellar/libxml2/2.9.9_2/include/libxml2 --use-system-libraries`

可以看到一下输出

    Building native extensions with: '--with-xml2-include=/usr/local/Cellar/libxml2/2.9.9_2/include/libxml2 --use-system-libraries'
    This could take a while...
    Successfully installed nokogiri-1.8.4
    Parsing documentation for nokogiri-1.8.4
    Installing ri documentation for nokogiri-1.8.4
    Done installing documentation for nokogiri after 12 seconds
    1 gem installed

------------------------------------------------------------------------------------------------------------------------------------