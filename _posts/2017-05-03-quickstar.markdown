---
layout:     post
title:      "「jekyll」小白开工好难！"
subtitle:   "第一次难免遇到很多简单却不知道答案的小问题"
date:       2017-05-03 12:00:00
author:     "Lilya"
header-img: "img/postPic/jekyll-bg.jpg"
catalog: true
tags:
    - jekyll
    - github
---

> 快速搭建可参考[黄玄的博客](http://huangxuan.me/)或者[小胖轩的博客](https://www.codeboy.me/)

想搭建 blog ，又不想花钱买服务器，[jekyll](http://jekyll.com.cn) 还是一个不错的选择。只要有github账户，就能完全免费发布自己的 blog ，还可以[自定义域名](https://help.github.com/articles/quick-start-setting-up-a-custom-domain/)。下面介绍 [Lilya blog](http://lilya.com.cn/) 的搭建过程，以及搭建过程中遇到的问题。

## 事前准备
jekyll 官方文档不建议在 Windows 平台上安装 Jekyll ,以下介绍都是基于 Ubuntu 完成的，Windows 用户可参照[文档](http://www.madhur.co.in/blog/2011/09/01/runningjekyllwindows.html)。

**1. Ruby 安装**
```
$ sudo apt-get install ruby-full
```
>其他平台的安装可参照[官方文档](https://www.ruby-lang.org/en/documentation/installation/)

**2. RubyGems 安装**

RubyGems（简称 gems）是一个用于对 Rails 组件进行打包的 Ruby 打包系统。它提供一个分发 Ruby 程序和库的标准格式，还提供一个管理程序包安装的工具。安装方式如下：

* 点击 [zip](https://rubygems.org/rubygems/rubygems-2.6.12.zip) 下载安装包
* 解压 zip 并在 terminal 中 `cd` 到解压目录
* 安装命令：`ruby setup.rb` (若需root权限，使用`sudo ruby setup.rb`)

**3. Jekyll 安装**

使用 RubyGems 安装 Jekyll，在 terminal 输入如下命令：
```
gem install jekyll
```

---

## 博客创建
准备工作做好以后，就可以正式开工了。
```
jekyll new myblog         //新建blog，命名为`myblog`
cd myblog                 //进入myblog目录  
~/myblog $ jekyll serve   //开启服务器
```
完成上述三条命令，即可通过 `http://localhost:4000` 在浏览器预览。如果你是前端大牛，或者想创造与众不同的blog，则可直接进入目录结构，修改配置，添加自己的头信息，开始撰写博客。具体内容可参考 [jekyll 文档](http://jekyll.com.cn/docs/structure/)。 另外一种快速省事的方式，就是去借鉴大神们的模板。以下是作者在搭建博客时参考的blog：

| Blog      |    Live Demo | Github Repository  |
| :-------- | :--------| :-- |  
| startbootstrap-clean-blog-jekyll  | [ demo for clean blog](http://blackrockdigital.github.io/startbootstrap-clean-blog-jekyll/) |  [github for clean blog](https://github.com/BlackrockDigital/startbootstrap-clean-blog-jekyll)   |
| Hux Blog     |   [demo for hux blog](https://huangxuan.me/) |  [github for  Hux blog](https://github.com/Huxpro/huxpro.github.io)  |
| Codeboy Blog      |   [demo for codeboy blog](http://blogtest.codeboy.me/) | [github for Codeboy blog](https://github.com/androiddevelop/CodeboyBlog)  |

如果想本地体验上述模板，可参考如下步骤（以Codeboy Blog为例）：

* **clone 项目**：git clone https://github.com/androiddevelop/CodeboyBlog.git
* **按需修改**：比如 `_config.yml` 中的配置，`_include` 中的 header、footer、nav文件等
* **运行命令**: `jekyll serve`，浏览器预览地址为 `http://localhost:4000`

想偷懒，可以编写如下脚本：
```
#!/bin/bash
ps aux |grep jekyll |awk '{print $2}' | xargs kill -9  //关闭 jekyll 进程
cd /blog path                                          //进入 blog 根目录
jekyll serve --watch                                   //开启服务
```
>脚本命名为serve.sh,在 terminal 中使用 `sh serve.sh` 即可运行脚本。

---

## Jekyll 部署
Jekyll 部署有很多方式，文中仅介绍将 Jekyll 部署到 Github Pages 上的流程（以**[用户和组织的站点](http://jekyll.com.cn/docs/github-pages/)**类型为例）。

**1. Github 仓库创建**

* 登录 GitHub，创建名为 username.github.io 的仓库（username 为 Github 用户名）
* 拷贝该仓库URL，形如 `git@github.com:username/username.github.io.git`（选择 Clone with SSH）
* 在 terminal 下运行 `git clone URL`，得到文件夹 `username.github.io`

>避免每次push需要输入用户名和密码：可进行如下操作：<br>
* 运行 ssh-keygen -t rsa 得到 SSH 公钥文件：id_rsa.pub
* 添加你的 SSH key（account settings -> SSH Keys -> Add SSH Key）
* 运行 git remote set-url origin URL_YOU_COPIED，让git使用密钥验证

**2. Blog 项目生成**

将 myblog 拷贝至文件夹 `username.github.io`，也就是 Github 仓库。可在图形界面操作，也可使用命令：
```
cp -r myblog/. username.github.io //拷贝 myblog 至 username.github.io
rm -r myblog                      //删除 myblog 源文件
```
直接在`username.github.io`中进行开发，比如配置修改及博客编写。

**3. Github 同步**

通过 git 操作将 jekyll 提交到仓库，为了减少简化操作，作者写了如下脚本去提交代码：
```
#!/bin/bash
cd ~/username.github.io/
git add .
git commit -m "XXX"
git push
```
>脚本命名为push.sh,在 terminal 中使用 `sh push.sh` 即可运行脚本。

操作完成以后，可在浏览器访问 username.github.io 查看blog。

**4. 自定义域名**

目前假定已经有自己的域名(xxx.cn)了，通过下面两步即可完成域名自定义。若没有，可先去购买，推荐 [DNSPod](http://domains.dnspod.cn/)或者是[腾讯云](https://dnspod.qcloud.com/)。
* 去自己的域名管理页，在域名中添加记录，记录类型若选择 `A` ，则记录值为 Github 主页 IP ；若选择 `CNAME`，记录值可填 `username.github.io`
* 进入 `username.github.io` 仓库，新建名为 `CNAME` 的文件，文件中写入自己的域名，形如 `xxx.cn`

>若不知道自己 Github 主页 IP ，可以在 terminal 中 ping Github 地址，命令为 `ping username.github.io`，正常情况下可在第一行看到 PING github.map.fastly.net (151.101.24.133):56(84) bytes of  data 字样，其中 151.101.24.133 即为 IP 地址。如下图：

![IPGet]({{ site.baseurl }}/img/postPic/2017-05-10-ipget.png)

至此，在浏览器中访问 `xxx.cn` 即可进入自己的 blog 啦。如果一路顺利，没有遇到任何问题，给自己撒花吧！！！

---

## 问题及解决方案

下面记录了作者在搭建 blog 时遇到的一些问题，及最后的解决方案。

**1. 问题**
>
**Dependency Error**: Yikes! It looks like you don’t have `jekyll-paginate` or one of its dependencies installed. In order to use Jekyll as currently configured, you’ll need to install this gem. The full error message from Ruby is: ‘cannot load such file – jekyll-paginate’ If you run into trouble, you can find helpful resources at Getting Help
jekyll 3.1.2  \|  Error: jekyll-paginate

**解决方案**：安装 jekyll 时候直接运行 `gem install jekyll-paginate` 即可解决

遗憾的是我并没有解决，因为我直接 clone 的 Github 上的代码，没有通过 `jekyll new XXX` 创建 blog，因此项目没有去 Running bundle install ，直观上可以发现项目中没有 `Gemfile` 文件和 `Gemfile.lock` 文件。最终的解决方式是：
```
安装 jekyll-paginate：gem install jekyll-paginate
生成 Gemfile 文件，并添加到 myblog 根目录
在 Gemfile 文件中添加一行 gem "jekyll-paginate"
在 myblog 根目录下运行命令 ：bundle install
```
**2.问题**
>
Deprecation: You appear to have pagination turned on,
    but you haven't included the `jekyll-paginate` gem.
    Ensure you have `gems: [jekyll-paginate]` in your configuration file.
          Source: /Users/chenkuntao/KarmaChen.github.io
     Destination: /Users/chenkuntao/KarmaChen.github.io/_site
Incremental build: disabled. Enable with --incremental
    Generating...
                  done in 0.228 seconds.
     Deprecation: You appear to have pagination turned on,
     but you haven't included the `jekyll-paginate` gem.
     Ensure you have `gems: [jekyll-paginate]` in your configuration file.
Auto-regeneration: enabled for '/Users/chenkuntao/KarmaChen.github.io'
Configuration file: /Users/chenkuntao/KarmaChen.github.io/_config.yml

**解决方案**：在 `_config.yml` 中添加 `gems: [jekyll-paginate]`

**3.问题**
>Dependency Error: Yikes! It looks like you don't have redcarpet or one of its
dependencies installed. In order to use Jekyll as currently configured, you'll n
eed to install this gem. The full error message from Ruby is: 'cannot load such
file -- redcarpet' If you run into trouble, you can find helpful resources at ht
tp://jekyllrb.com/help/!
  Conversion error: Jekyll::Converters::Markdown encountered an error while conv
erting '_posts/2014-12-28-first-blog.md':
redcarpet
 ERROR: YOUR SITE COULD NOT BE BUILT:
redcarpet

**解决方案**:<br>
* 安装kramdown: gem install kramdown
* 在 `_config.yml` 修改配置：
```
markdown: kramdown
kramdown:
  input: GFM               
```

**3. 问题**

执行 `jekyll serve` 后出现如下异常：
>WARN: Unresolved specs during Gem::Specification.reset:
      rb-inotify (>= 0.9.7, ~> 0.9)
WARN: Clearing out unresolved specs.
Please report a bug if this causes problems.
/var/lib/gems/2.0.0/gems/bundler-1.13.7/lib/bundler/runtime.rb:40:in `block in setup': You have already activated addressable 2.5.1, but your Gemfile requires addressable 2.5.0. Prepending `bundle exec` to your command may solve this. (Gem::LoadError)
	from /var/lib/gems/2.0.0/gems/bundler-1.13.7/lib/bundler/runtime.rb:25:in `map'
	from /var/lib/gems/2.0.0/gems/bundler-1.13.7/lib/bundler/runtime.rb:25:in `setup'
	from /var/lib/gems/2.0.0/gems/bundler-1.13.7/lib/bundler.rb:99:in `setup'
	from /var/lib/gems/2.0.0/gems/jekyll-3.3.1/lib/jekyll/plugin_manager.rb:36:in `require_from_bundler'
	from /var/lib/gems/2.0.0/gems/jekyll-3.3.1/exe/jekyll:9:in `<top (required)>'
	from /usr/local/bin/jekyll:23:in `load'
	from /usr/local/bin/jekyll:23:in `<main>'

**解决方案**:
* 方法一：使用命令 `bundle exec jekyll serve`
* 方法二：将 `Gemfile.lock` 文件中的 `addressable 2.5.0` 改为 `addressable 2.5.1`
