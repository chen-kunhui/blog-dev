---
title: GitHub Pages 搭建个人博客
date: 2018-07-17 21:38:45
tags:
  - Blog
  - Base
categories:
  - other
---

# 概述

[GitHub Pages] 是一种静态站点托管服务，旨在直接从GitHub存储库托管您的个人，组织或项目页面,详细的文档可阅读 [GitHub Pages]文档。

利用 [GitHub Pages] 搭建个人博客的主流方案有以下两种：
- [GitHub Pages] + [Hexo]
- [GitHub Pages] + [jekyll]

# GitHub Pages

> 参考 [GitHub Help][GitHub Pages]

[GitHub Pages] 的使用基本有以下三种方式：
- 通过创建一个名为 `<yourname>.github.io` 的项目
- 通过在任一项目中创建一个名为  `gh-pages` 的分支
- 通过在任一项目的 `master` 分支中，创建一个名为 `docs` 的文件夹

以上三种方式都可以启用 [GitHub Pages] 功能，当然三个也可以同时使用，拥有多个 [GitHub Pages], 但通常我们使用第一种方式来搭建一个个人的博客。

接下来我们列举每一种方式的使用。

## 创建 `<yourname>.github.io` 项目使用 GitHub Pages

通过创建一个名为 `<yourname>.github.io` 的项目的方式来使用 [GitHub Pages] 主要包含以下几步：
1. 注册一个 github 账号
2. 创建一个 `<yourname>.github.io` 的工程
3. 在工程的 `master` 分支根目录下创建一个 `index.html` 或 `index.md` 的文件
4. 访问`http://<yourname>.github.io` 就可以看到你的页面了

**注：**
- 这种方式 [GitHub Pages] 使用的是 `master` 分支
- `master` 根目录下必须要有 `index.html` 或 `index.md` 的文件。
- `<yourname>.github.io` 中的 `<yourname>` 应该和图中箭头标志部分一致，设置不对，[GitHub Pages] 不会生效。

![your github name](/images/github-page.jpg)

## 创建 `gh-pages` 分支使用 GitHub Pages

通过创建一个名为 `gh-pages` 分支的方式来使用 [GitHub Pages] 主要包含以下几步：
1. 注册一个 github 账号
2. 在一个新工程(工程名随意)或在已有工程中拉取一个名为 `gh-pages` 的分支
3. 在 `gh-pages` 分支的根目录下创建一个 `index.html` 或 `index.md` 的文件
4. 检查 project 的 `setting` 中的 `GitHub Pages` 部分正确设置了分支，如图：

![GitHub Pages setting](/images/gh-pages-seting-brh.jpg)

5. 访问`http://<yourname>.github.io/<projectName>` 就可以看到你的页面了

**注：**
- 这种方式 [GitHub Pages] 使用的是 `gh-pages` 分支
- 分支名必须为 `gh-pages`
- `http://<yourname>.github.io/<projectName>` 中 `yourname`  见[前面的描述](#创建-lt-yourname-gt-github-io-项目使用-GitHub-Help),`projectName` 为 `gh-pages` 分支所在项目的项目名。
- `gh-pages` 根目录下必须要有 `index.html` 或 `index.md` 的文件。

## 创建 `docs` 文件夹使用 GitHub Pages

通过创建一个名为 `docs` 文件中的方式来使用 [GitHub Pages] 主要包含以下几步：
1. 注册一个 github 账号
2. 在一个新工程(工程名随意)或在已有工程的 `master` 下，创建一个名为 `docs` 的文件夹
3. 在 `docs` 文件中创建一个名为 `index.md` 的文件
4. 检查 project 的 `setting` 中的 `GitHub Pages` 部分正确设置了分支，如图：

![GitHub Pages setting](/images/gh-pages-doc.jpg)

5. 访问`http://<yourname>.github.io/<projectName>` 就可以看到你的页面了

**注：**
- 这种方式 [GitHub Pages] 使用的是 `master` 分支下 `docs` 文件夹中的文件。
- 文件夹名必须为 `docs`
- `http://<yourname>.github.io/<projectName>` 中 `yourname`  见[前面的描述](#创建-lt-yourname-gt-github-io-项目使用-GitHub-Help),`projectName` 为 `gh-pages` 分支所在项目的项目名。
- `docs` 根目录下应该有 `index.md` 文件。

## GitHub Pages 绑定自己的域名

1. 购买域名
2. 将域名解析映射到你 github pages 的网址。
3. 在你 github pages 中设置里的 Custom domain 填写购买的域名即可。

# GitHub Pages + jekyll

通过利用 GitHub Pages + jekyll ，很轻松得就可以搭建一个个人的博客。
[jekyll] 是一个 [ruby] 的 [gem] 包，所有在使用前，需要你机器中安装了 ruby。

## 安装 ruby
ruby的安装可参考：
- [ruby-lang.org](http://www.ruby-lang.org/zh_cn/downloads/)
- [rvm 管理 ruby](https://ruby-china.org/wiki/rvm-guide)
- [rbenv 管理 ruby](https://ruby-china.org/wiki/rbenv-guide)

安装好 ruby 后，运行以下命令，应该就可以看到 ruby 和 gem 的版本。
```
ruby -v
gem -v
```

## 安装 jekyll

参考资料：
- 可参考[ruby-china](https://gems.ruby-china.com/)配置 gem 源，这样安装的时候会快些
- [jekyll 文档](https://jekyllrb.com/docs/installation/)

执行以下命令安装 jekyll：
```
gem install jekyll bundler
```

查看 jekyll 和 bundler 版本
```
jekyll -v
bundle -v
```

## 创建一个 jekyll 项目

运行下面命令，新建一个 jekyll 项目：
```
jekyll new proj
```

得到的项目目录结构大概像下面这样：
```
proj
├── 404.html
├── about.md
├── _config.yml
├── Gemfile
├── Gemfile.lock
├── index.md
└── _posts
    └── 2018-07-18-welcome-to-jekyll.markdown
```

## 在github上创建`<yourname>.github.io`工程

这里可参考前文的介绍

## 将jekyll 项目推送到github上

在github上新建新建一个`<yourname>.github.io`的工程，然后将proj下的所有文件推到你新建工程的`master`分支中。访问`http://<yourname>.github.io`即可看到页面。

## 本地预览
访问`http://<yourname>.github.io`可看到你的github pages页面，如果要做本地预览，可运行：
```
jekyll serve
```
访问 `http://localhost:4000` 即可。

## 备注
- 除了 上述列出的文件是必须推送到github外，其他开发过程中生成的文件，可选择性忽略或推送上去
- 更多 jekyll 使用，可参考 [jekyll 文档][jekyll]
- 要写博客，直接在`_posts`文件夹下，新建一个 `yyyy-mm-dd-your-post-name.md`的文件即可。
- 新加的文章，文件名必须以`yyyy-mm-dd-`的形式开头

# GitHub Pages + Hexo

利用 GitHub Pages + Hexo 的方式，也是可以很方便搭建和管理个人博客的。
[Hexo] 是一个 npm 包，使用 hexo 需要安装 [nodejs] 和 [npm]。

## 安装 nodejs 和 npm

[nodejs] 和 [npm] 的按装可参考：
- https://www.runoob.com/nodejs/nodejs-install-setup.html

通常NPM是随同NodeJS一起安装的，所以当你按装好nodejs后，npm也随之安装好了。

## 安装 Hexo

Hexo 有详细中文文档这里就不细介绍，可参考
- [Hexo中文文档](https://hexo.io/zh-cn/docs/)

## 创建一个 Hexo 工程

运行下面命令，新建一个 Hexo 项目：
```
hexo init proj
```

得到的项目目录下至少会包含以下目录：
```
_config.yml  node_modules  package.json  package-lock.json  scaffolds  source  themes
```

## 在github上创建`<yourname>.github.io`工程

这里可参考前文的介绍

## 发布到 guthub pages

hexo 与 jekyll 不同，不能直接通过推送整个工程，来生成博客。可通过以下两种方式发布：
1. 运行 `hexo generate` 生成所有静态文件到public文件夹中，然后将public下的所有文件推送到`<yourname>.github.io`工程的master分支中。
2. 同配置hexo，运行 `hexo deploy`,自动发布（推荐使用）

接下来详细说明第二种，以 git 为例，更多可参考[hexo 部署](https://hexo.io/zh-cn/docs/deployment.html)。

1. 安装依赖
```
npm install hexo-deployer-git --save

```

2. 修改 `_config.yml` 文件中的以下部分
```
deploy:
  type: git
  repo: git@github.com:chen-kunhui/chen-kunhui.github.io.git
  branch: master
```
注：将 repo 部分更换成你工程的 clone 链接。

3. 运行以下命令部署
```
hexo deploy
```
注： 如果部署没生效，可运行`hexo clean` 再运行上面命令。

4. 访问 `http://<yourname>.github.io` 即可看到部署的页面。

## 本地预览
访问`http://<yourname>.github.io`可看到你的github pages页面，如果要做本地预览，可运行：
```
hexo server
```
访问 `http://localhost:4000` 即可。

## 备注

详细使用，可阅读[hexo 文档][Hexo]

# 其他

GitHub Pages 网站受有使用限制：
- GitHub Pages源存储库的限制为1GB。
- 发布的GitHub Pages网站可能不超过1GB。
- GitHub的网页的网站有一个每月100GB的带宽限制。
- GitHub Pages网站的限制为每小时10个版本。 

[GitHub Pages]: https://help.github.com/categories/github-pages-basics/
[Hexo]: https://hexo.io/zh-cn/docs/index.html
[jekyll]: https://jekyllrb.com/docs/home/
[ruby]: http://www.ruby-lang.org/zh_cn/
[gem]: https://rubygems.org/
[nodejs]: http://nodejs.cn/
[npm]: https://www.npmjs.com/