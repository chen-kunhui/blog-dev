---
title: rails.5.2 之 master.key 文件
date: 2018-08-18 22:28:22
tags:
categories:
  - ruby
---

> 引用自 https://ruby-china.org/topics/36081

先回顾在5.2版本以前，我们为了处理很多私密的信息，需要在.gitignore里加入多少的yml文件，有secrets.yml、database.yml、cable.yml，甚至会加入一些自定义的配置文件，如：application.yml。
然后在部署配置中：
    
    set :linked_files, fetch(:linked_files, []).push('config/database.yml', 'config/secrets.yml', 'config/cable.yml'...)

# 把私密信息放入credentials中
因为我们在使用credentials.yml.enc后，可以把所有私密的信息都放到里面，然后通过master.key进行解密。所以当我们团队在一起完成一个项目时，需要分享master.key文件，但是考虑到安全的问题，不要把master.key上传到git的仓库中。

当我们打开credentials.yml.enc发现内容是一串随机串，如下：

    qEbH/+fadmlPVeMVgFvpwk4ADadW2LbMkzMKJKkHrZeFNooiKKJvOoe5YJlbab1wJLHL77nSohvEm6MYnl9krXLFnDG0iSWm/svtipruMCc1FVfhSpmXSvLNJI1RUk2VeZCFjYkT8/PG4N7oj1OLrSq4yeRsbKTrS/3izcMm9ndJkcd4/wR7WAMReQSRGt5YGNZ4E3Jt9Wgg7ls2okZcwxEv/3brdgIyHrmfyEWb50YSe5oDDyscfRNX70uwZieSVGn99fFcexYUL8F0dxSrVNaix/h/UAeApq6Ifhs0/p9eXk0349f8dEMFkp5A3I4j0ubgjZ/ncdLTct37OxxhfucWukCtP6oSFvpC+5ma2epjjTSJM25+Vv3GQy7xfSdwbsEq8jm3tqT/zGr2M9iRuEX+LJrxzhDHnHC0--jQ+G6M9HWGe7zlFb--ltdsqYuI+4O8cqw/bcJRJw==

非常棒，secret_key_base之类的信息都在里面安全的保护着，可是如果我要修改里面的内容，怎么办？我们不可能直接修改加密后的内容啊。

不要着急，Rails这里给我们提供了一个方便的方法：EDITOR="mate --wait" bin/rails credentials:edit。当然，EDITOR的信息需要根据我们使用的工具进行调整。

    EDITOR=vim bin/rails credentials:edit  # 使用VIM编辑
    EDITOR="subl --wait" bin/rails credentials:edit  # 使用Sublime编辑


输入命令后，我们可以清楚的看到里面的内容：

    # aws:
    #   access_key_id: 123
    #   secret_access_key: 345
    # Used as the base secret for all MessageVerifiers in Rails, including the one protecting cookies.
    secret_key_base: 0b4d8892f41d7b89127b3ad997ca9ca581fe7f84bf890c85095b635c651a9de00edd1032aeb377a5753f

然后在我们需要用到这些加密信息的地方，调用：
    Rails.application.credentials[Rails.env][:database][:database]
    Rails.application.credentials[Rails.env][:database][:password]

剩下部署的时候，就记得把master.key放到服务器里，不要弄丢了。