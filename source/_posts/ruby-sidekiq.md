---
title: sidekiq 简单使用
date: 2018-12-22 14:14:13
tags:
categories:
  - ruby
---

# 安装

  1. 将sidekiq添加到Gemfile中
  ```
  gem 'sidekiq', '~> 5.2', '>= 5.2.3'
  ```
  2. 执行 bundle install

# 配置
sidekiq 将执行任务的各种信息默认保存到 redis 中，所以需要配置redis客户端

1. 在 `config/initializers/sidekiq.rb`中加入如下配置，如过不配置，默认使用`localhost:6379`的redis
```
Sidekiq.configure_server do |config|
  config.redis = { url: 'redis://redis.example.com:7372/0' }
end

Sidekiq.configure_client do |config|
  config.redis = { url: 'redis://redis.example.com:7372/0' }
end
```

> 更多信息可参考 https://github.com/mperham/sidekiq/wiki/Using-Redis

# 编写后台任务

所有后台任务都写在`app/workers`文件夹下，可手动创建，也可以通过以下命令创建
```
rails g sidekiq:worker <name>
```

创建好的任务文件格式如下
```ruby
class HardWorker
  include Sidekiq::Worker
  def perform(name, count)
    # do something
  end
end
```

# 执行后台任务

1. 在需要执行的地方像如下代码调用即可
```ruby
# 异步运行
HardWorker.perform_async('bob', 5)
# 5 分钟后运行
HardWorker.perform_in(5.minutes, 'bob', 5)
```

2. 启动sidekiq进行
```
bundle exec sidekiq
```
> sidekiq 和 raile 以及 redis 都要启动，仅启动其中一个是无法执行后台任务的

# sidekiq 配置文件

默认sidekiq 读取`config/sidekiq.yaml`这个配置文件，如果配置文件不是这个可在启动`sidekiq`时通过`-C`指定配置文件，像这样：
```
bundle exec sidekiq -C config/myapp_sidekiq.yml
```

配置文件也可以没有，这个不会影响任务的执行，但如果想要定制sidekiq的一些行为，就需要配置了

> 配置详细信息可参考 https://github.com/mperham/sidekiq/wiki/Advanced-Options

# sidekiq web ui 界面
sidekiq 自带一个web 管理界面，可通过下面方式开启

1. 在`config/routes.rb` 中加入如下内容
```ruby
 require 'sidekiq/web'
  mount Sidekiq::Web => '/sidekiq'
```

2. 访问`http://localhost:port/sidekiq` 即可

> 更多细节可参考 https://github.com/mperham/sidekiq/wiki

# sidekiq 定时任务

在上面的基础上继续做如下事情

1. 添加一个gem包到Gemfile中
```
gem 'sidekiq-cron', '~> 1.0', '>= 1.0.4'
```

2. 编写worker任务，跟sidekiq是一样的

4. 通过下面的任一方法，将定时任务添加到sidekiq中
```ruby
job = Sidekiq::Cron::Job.create(name: 'Hard worker - every 5min', cron: '*/5 * * * *', class: 'HardWorker')
job.save
```

```ruby
hash = {
  'name_of_job' => {
    'class' => 'MyClass',
    'cron'  => '1 * * * *',
    'args'  => '(OPTIONAL) [Array or Hash]'
  },
  'My super iber cool job' => {
    'class' => 'SecondClass',
    'cron'  => '*/5 * * * *'
  }
}

Sidekiq::Cron::Job.load_from_hash hash
```

```ruby
array = [
  {
    'name'  => 'name_of_job',
    'class' => 'MyClass',
    'cron'  => '1 * * * *',
    'args'  => '(OPTIONAL) [Array or Hash]'
  },
  {
    'name'  => 'Cool Job for Second Class',
    'class' => 'SecondClass',
    'cron'  => '*/5 * * * *'
  }
]

Sidekiq::Cron::Job.load_from_array array
```

> 相同的任务多次创建时，只会创建一个

> 更多信息可参考 https://github.com/ondrejbartas/sidekiq-cron

5. 开启 web ui 界面

在`config/routes.rb` 中加入如下内容
```ruby
 require 'sidekiq/web'
 require 'sidekiq/cron/web'
  mount Sidekiq::Web => '/sidekiq'
```
同样访问`http://localhost:port/sidekiq` 即可
