---
title: hexo博客添加gitalk评论系统
date: 2020-09-25 17:48:32
tags:
 - hexo
 - github
 - gitalk
---

&emsp;&emsp;经过了一天的折腾，我终于为自己的博客添加上了评论系统。坦率的讲，为什么网上那么多方案我还要自己写一篇博客，那就是因为他们说的都有bug,所以我要自己总结一下。

&emsp;&emsp;我选用的是gitalk评论系统，原因是因为它可以直接在github上管理评论，不需要在别的平台注册，特别方便。下面我来详细说一下hexo博客是如何添加gitalk评论的。

&emsp;&emsp;先看一下效果：


![image](https://img-blog.csdnimg.cn/2020092516551467.gif)


# 一、配置github
&emsp;&emsp;首先进入`github`,点击右上角头像【Settings】->【Developer settings】->【OAuth Apps】->【New OAuth App】进行基本配置（**一定要细心，看清截图中的红字**）。

![image](https://img-blog.csdnimg.cn/20200925170806924.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2NvZGVMaXVndWlzaGVuZw==,size_16,color_FFFFFF,t_70)

&emsp;&emsp;上面的填写成功之后进入,我们需要的是生成的`Client ID
`和`Client Secret`

![image](https://img-blog.csdnimg.cn/2020092517103878.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2NvZGVMaXVndWlzaGVuZw==,size_16,color_FFFFFF,t_70)

# 二、博客配置

&emsp;&emsp;上面的确认无误后，我们进行配置博客，引入gitalk。

&emsp;&emsp;首先进入主题的配置文件`_config.yml`(注意是主题的配置文件，不是博客根目录下的配置文件)，添加gitalk配置(一定要仔细，缺一不可)。

```
gitalk:
  enable: true 开启gitalk评论，不需要配置
  owner: github用户名
  admin: github用户名
  repo: 博客的仓库名称(注意不是地址)
  ClientID: 上面生成的Client ID
  ClientSecret: 上面生成的Client Secret
  labels: 'gitalk' github issue 对应的issue标签（新建一个）
  distractionFreeMode: true  无干扰模式，不需要更改
```
&emsp;&emsp; 下面是我的配置：

![image](https://img-blog.csdnimg.cn/20200925172124545.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2NvZGVMaXVndWlzaGVuZw==,size_16,color_FFFFFF,t_70)


&emsp;&emsp; 上面配置完毕后，进入主题目录->【layout】->【_partial】->【post】目录，在当前目录下新建一个`gitalk.ejs`文件，写入如下代码：
```
<link rel="stylesheet" href="https://unpkg.com/gitalk/dist/gitalk.css">
<script src="https://unpkg.com/gitalk/dist/gitalk.min.js"></script>
<script src="https://priesttomb.github.io/js/md5.min.js"></script>
<script type="text/javascript">
    new Gitalk({
        clientID: '手动马赛克',
        clientSecret: '手动马赛克',
        repo: 'qisi007.github.io',
        owner: 'qisi007',
        admin: 'qisi007',
        id: md5(location.pathname),
        distractionFreeMode: true
    }).render('gitalk-container')
</script>
```

&emsp;&emsp; 里边的配置我就我说了，和上面的一摸一样，然后进入上一级的目录，路径是
主题目录->【layout】->【_partial】下的`article.ejs`文件最后面加入下边的代码：
```
<% if (theme.gitalk.enable){ %>
	<div id="gitalk-container"></div>
	<%- include post/gitalk.ejs %>
<% } %>
```
&emsp;&emsp;好了，到此为止，所有的配置就完成了。

&emsp;&emsp;执行命令`hexo d -g`打包发布，过几分钟应该能看到效果了，应该能看到效果了。

&emsp;&emsp;评论管理页面在仓库的issue里边。

![image](https://img-blog.csdnimg.cn/20200925173032555.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2NvZGVMaXVndWlzaGVuZw==,size_16,color_FFFFFF,t_70)

# 三、问题

&emsp;&emsp;大部分问题都是因为`Homepage URL`和`Authorization callback URL`这两个地址错误引起的，比如服务器错误，或者是点击登录跳转到博客主页等等。

&emsp;&emsp;还有个问题是进入博客详情页后，评论插件上面显示`Error: Validation Failed`, 这是因为文章名称经URL编码后添加到issues的label里，但是label的长度上限是50个字符，所以文章名有些长的都会生成label失败，也就没办法评论了。

&emsp;&emsp;所以上面我们用到了`md5`,文章名经URL编码后转MD5，然后再生成label,这样就不会超过长度了。

![image](https://img-blog.csdnimg.cn/20200925173930342.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2NvZGVMaXVndWlzaGVuZw==,size_16,color_FFFFFF,t_70)

# 四、写在最后

&emsp;&emsp; 我的博客主题用的是`freemind`，这是一个复古风格的主题，这个主题本身不带评论插件的，本人进行了二次封装，修改了一些样式和bug,并上传到了自己的`github`中，欢迎star。  [**传送门**](https://github.com/qisi007/hexo-theme-freemind.386.second)


个人博客主页: [传送](https://www.liuguisheng.vip)
开源项目（react-admin-plus）: [传送](https://github.com/qisi007/react-admin-plus)