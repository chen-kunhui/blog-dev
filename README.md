# 本地安装及启动

clone 到本地：
```
git clone https://github.com/chen-kunhui/blog-dev.git
```

安装依赖包
```
npm install
```

启动服务
```sh
# 默认运行在 4000 端口
hexo s

# 启动服务预览草稿文件
hexo s --draft
```

发布
```
hexo deploy
```
**注：** 如果发布后没有出现新添加的文章则本地运行`hexo clean`后再发布

# 其他常用命令

```sh
# 新建一篇文章或页面等
hexo new [layout] <tittle>
# 比如新件一篇名为abc的文章
hexo new post abc
```

```sh
# 发布草稿
hexo publish [layout] <filename>
# 比如发布 _drafts文件夹下的 abc.md 文件
hexo publish post abc
```

# hexo 文档
> https://hexo.io/zh-cn/docs/