---
title: ruby Devise 登陆验证框架
date: 2018-08-05 13:54:34
tags:
categories:
  - ruby
---

>参考 https://github.com/plataformatec/devise
>参考 https://www.jianshu.com/p/8dfff067197d

# 概述

devise 是一个用于登陆验证的 ruby gem包

# 基本使用
1. 在 Gemfile 中加入以下代码
```
gem devise
```

2. 然后执行以下代码
```
# 下载安装 devise
bundle instll
# 初始化 devise
rails generate devise:install
# 生成一个 user 模型
rails g devise user
# 执行迁移命令
bundle exec rake db:migrate
# 执行devise提供的脚手架，生成相关视图
rails g devise:views
```

> 还需要定义在路由配置中定义好root path(如`root to: "home#index"`), 因为 devise 默认登陆成功后重定向到root_path，当然也可以在controller中定义`after_sign_in_path_for` 和 `after_sign_out_path_for` 方法，来重新定义用户登陆和退出后重定向的path。

当执行完以上命令后，devise 就可以使用了，以下是devise提供几个方法
```
# 通过在需要做登陆验证的控制器中加入这句代码，开启登陆验证
before_action :authenticate_user!
# 用于判断用户是否已登陆
user_signed_in?
# 获取当前登陆的用户
current_user
# 当前用户的存储域
user_session
```

# 其他用法

## 使用用户名登陆而不是使用邮箱登陆

1. 添加一个用于登陆验证的username字段
```
rails generate migration add_username_to_users username:string
bundle exec rake db:migrate
```

2. 在config/initializers/devise.rb中配置登录验证的字段
```
config.authentication_keys = [:username]
```

3. 在view试图中将表单中的email改为username

4. 修改完之后，接下来需要重写一个方法，用来配置登录和注册所允许的参数，将此方法写在application_controller.rb中
```ruby
class ApplicationController < ActionController::Base
  before_action :authenticate_user!
  before_action :configure_permitted_parameters, if: :devise_controller?

  protected

  def configure_permitted_parameters
    devise_parameter_sanitizer.permit(:sign_up, keys: [:username, :email, :password, :password_confirmation, :remember_me])
    devise_parameter_sanitizer.permit(:sign_in, keys: [:username, :password, :remember_me])
    devise_parameter_sanitizer.permit(:account_update, keys: [:username, :password, :password_confirmation, :current_password])
  end
end
```
