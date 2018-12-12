---
title: 在简单的 ruby 应用中使用 Gemfile
date: 2018-11-23 19:22:50
tags:
  - ruby
  - bundle
  - Gemfile
categories:
  - ruby
---

# 在简单的 ruby 应用中使用 Gemfile

在写一些稍复杂的脚本时，如果想使用一些 gem 包，但又不想将gem安装到系统目录中，该怎么做呢？下面将讲述如何在不使用任何框架的情况下，如何使用 bundle 来管理 gem 包。

1. 创建一个文件，用来存放你的脚本及相关依赖
```shell
$ mkdir script
```

2. 执行下面命令，初始化 Gemfile
```shell
$ bundle init
```

3. 接下来，你就可以在 Gemfile 中像其他应用一样，引入你的依赖 gem

4. 执行以下命令， 将 gem 安装到当前文件夹下的 `vendor/bundle` 目录下
```shell
$bundle install --path=./vendor/bundle
```

5. 在你脚本的中加入以下代码，即可使用 Gemfile 中的依赖 gem
```ruby
#!/usr/bin/env ruby
ENV['BUNDLE_GEMFILE'] ||= File.expand_path('../Gemfile', __FILE__)
require 'bundler/setup'
Bundler.require(:default)

# 接下来就可以写你的应用代码了

```

> 这时直接使用依赖的 gem 就可以了，不需在使用前 require 了，`Bundler.require(:default)` 已经将 gem 引入了
> `Bundler.require(:default)`引入的是没有在group中的gem
> `Bundler.require(:test)` 表示仅引入名为test group中的gem
> `Bundler.require(:default,:test)` 表示引入没有group中的gem和test group中的gem

# 其他

有时候我写有多个 ruby 文件，在使用时需要一个一个地require，文件很多的时候通常很繁琐，我们可以通过一些方式，批量引入，例如批量引入当前文件夹下 lib 文件下的所有 ruby 文件。
```ruby
Dir[ File.expand_path('../dsl/search/queries/**/*.rb', __FILE__) ].each        { |f| require f }
```

> 需要留意的是，如果lib文件夹的ruby文件又相互require，那么必须要确保被requrie的文件要提前引入

# 其他参考资料

> https://ruby-china.org/topics/26655

> https://ruby-china.org/topics/25530