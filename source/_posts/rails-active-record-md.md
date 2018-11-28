---
title: 不依赖框架情况下使用 ORM
date: 2018-11-23 20:52:53
tags:
categories:
---

# ActiveRecord

ActiveRecord 是一个 orm 框架，它是rails的一部分，但我们也可以在rails 以外单独使用

1. 安装依赖的gem包
```shell
$ bundle install activerecord
$ bundle install mysql2
```

2. 示例脚本
```ruby
#!/usr/bin/env ruby

require 'active_record'
require 'mysql2'

# 需要提前创建好数据库
ActiveRecord::Base.establish_connection(
:adapter => "mysql2",
:username => "root",
:host => "localhost",
:password => "root",
:database => "testdb"
)

# ActiveRecord::Migration[4.2] 中的[4.2]是必须的，表示rails的版本
# 虽然我们没有使用rails 但这里还是需要指定，可以是其他版本也可以，否则会报错
# Directly inheriting from ActiveRecord::Migration is not supported.
# Please specify the Rails release the migration was written for: (StandardError)
class CreateUsers < ActiveRecord::Migration[4.2]
  def change
    create_table :labels do |t|
      t.string :name
    end
  end
end

# 执行数据迁移创建users表
CreateUsers.new.change

class User < ActiveRecord::Base
end

User.create name: 'test'
p User.find 1
```

> https://ruby-china.org/topics/29816