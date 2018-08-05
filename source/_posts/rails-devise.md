---
title: Devise
date: 2018-08-05 13:54:34
tags:
  - ruby
  - rails
categories:
---

>参考 https://github.com/plataformatec/devise
>参考 https://www.jianshu.com/p/8dfff067197d

# 概述

devise 是一个用于登陆验证的 ruby gem包

# 安装
1. 在 Gemfile 中加入以下代码
```ruby
gem devise
``` 
2. 然后执行以下代码
```
# 下载安装 devise
bundle instll
# 初始化 devise
rails generate devise:install
# 确保在路由文件中定义了 root 路由，比如下面这个，devise 登陆成功后，会跳转到root_path
root to: "home#index"
# 执行devise提供的脚手架，生成相关视图
rails g devise:views
# 生成一个 user 模型
rails g devise user
# 执行迁移命令
bundle exec rake db:migrate
```

# 自定义登陆验证字段
## 使用用户名登陆而不是使用邮箱登陆
1. 添加一个用于登陆验证的username字段
```
rails generate migration add_username_to_users username:string
bundle exec rake db:migrate
```
2. 在config/initializers/devise.rb中配置登录验证的字段
```
# config.authentication_keys 这个是必须配的，另外两个可不配
config.authentication_keys = [:username]
config.case_insensitive_keys = [:username]
config.strip_whitespace_keys = [:username]
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
