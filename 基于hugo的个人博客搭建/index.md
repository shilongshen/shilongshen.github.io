# 基于hugo的个人博客搭建




#### 本文是基于ubuntu16.04进行搭建



## 下载安装hugo

~~~
下载地址：https://github.com/gohugoio/hugo/releases/download/v0.56.0/hugo_0.56.0_Linux-64bit.deb
安装：sudo dpkg -i hugo_0.56.0_Linux-64bit.deb
~~~

注意如果直接使用

~~~
sudo apt install hugo
~~~

进行安装，安装的版本可能过低，导致一些主题不能够使用。要尽可能下载新版本！

安装完成后可以使用

~~~
hugo version
~~~

来检查是否安装成功。

## 生成站点

~~~
hugo new site /path
~~~

/path站点表示路径，我这里设为了‘myblog’ , myblog中的结构

- archetypes  config.toml  content  data  layouts  static  themes

## 创建文章

~~~
hugo new post/about.md
~~~

这是会在content/post文件夹下生成一个about.md文件

## 安装皮肤

~~~
cd theme
git clone https://github.com/vaga/hugo-theme-m10c.git
~~~

hugo提供的皮肤可以在这里[找到](https://www.gohugo.org/theme/)

## 运行hugo

~~~
//切换到根目录下
hugo server --theme=LoveIt --buildDrafts
~~~

这是你可以打开http://localhost:1313/ 在本地查看博客.

## 部署

这里尝试在github部署我的博客。

首先在github上新建一个仓库

![](https://gitee.com/shilongshen/image-bad/raw/master/img/20201115090850.png)

**注意仓库的名字为‘github的名字'+github.io** 

(由于我之前已经建立过了，所以这里提示已经建立过了)



然后执行

~~~
hugo --theme=LoveIt --baseUrl="https://shilongshen.github.io/" --buildDrafts
~~~

注意命令行中没有server。

此时将会在'myblog'下生成一个'public'文件夹,即为我们的Git仓库

~~~
cd public
git init
git add .
git remote add origin https://github.com/shilongshen/shilongshen.github.io.git
git commit -m 'first blog'
git push -u origin master
~~~

随后我们就可以通过	https://shilongshen.github.io/ 访问我们的博客.


## 文章的更新

~~~
//切换到根目录下
//创建一篇文章,或者是修改一篇文章
hugo new post/**.md

//本地启动博客，检查内容，此步骤可以忽略
hugo server --theme=LoveIt --buildDrafts

//确认无误后在远端进行更新
hugo --theme=LoveIt --baseUrl="https://shilongshen.github.io/" --buildDrafts  #这条命令会更新public文件夹，然后将public的内容推上仓库即可
cd public
git add .
git commit -m 'change'
git push -u origin master
~~~


