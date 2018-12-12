---
title: ruby 简单发邮件脚本
date: 2018-11-28 21:09:41
tags:
 - ruby
 - email
categories:
 - ruby
---

# 通过 'net/smtp' 标准库发送email

```ruby
require 'net/smtp'

message = <<MESSAGE_END
From: Private Person <chen_kunhui@163.com>
To: A Test User <c.k.h@foxmail.com>
MIME-Version: 1.0
Content-type: text/html
Subject: SMTP e-mail test

<b>This is HTML message.</b>
<h1>This is headline.</h1>
MESSAGE_END

Net::SMTP.start('smtp.163.com', 25, '163.com', 'chen_kunhui', '*****',:plain) do |smtp|
  smtp.send_message message, 'chen_kunhui@163.com',
                             'c.k.h@foxmail.com'
end
```

> 主要 message 的格式，一个电子邮件要要From，To和Subject，文本内容与头部信息间需要一个空行

# 通过 第三方 gem 包的方式

```
gem install mail
```

```ruby
require 'mail'

smtp = { :address => 'smtp.163.com', :port => 25, :domain => '163.com', :user_name => 'chen_kunhui', :password => '***', :enable_starttls_auto => true, :openssl_verify_mode => 'none' }
Mail.defaults { delivery_method :smtp, smtp }
mail = Mail.new do
  from 'chen_kunhui@163.com'
  to 'c.k.h@foxmail.com'
  subject 'test'
  content_type 'text/html'
  body "<h1>hahaha</h1>"
end
mail.deliver!
```

# 通过单独使用 rails 的 action mailer 组件的方式

```ruby
require 'action_mailer'

ActionMailer::Base.smtp_settings = {
  address:        'smtp.163.com',        # default: localhost
  port:           '25',                  # default: 25
  user_name:      'chen_kunhui',
  password:       '***',
  authentication: :plain                 # :plain, :login or :cram_md5
}

class Notifier < ActionMailer::Base
  default from: 'chen_kunhui@163.com'

  def welcome_email(recipient, email_body)
    mail(to: recipient,
         body: email_body,
         content_type: "text/html",
         subject: "Already rendered!")
  end
end

msg = Notifier.welcome_email('c.k.h@foxmail.com', "<h1> hello!</h1>")
msg.deliver_now
```

> http://www.runoob.com/ruby/ruby-sending-email.html

> https://github.com/mikel/mail/

> https://github.com/rails/rails/tree/v5.2.1.1/actionmailer

> https://ruby-china.github.io/rails-guides/action_mailer_basics.html