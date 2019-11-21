---
title: 解决github io的sitemap生成问题
tags: 杂谈
---

虽然我的博客没人访问,  只有我在上面记录一些内容, 但是看到不能正确地生成sitemap.xml,  谷歌无法进行正常收录, 我就莫名地不爽.

我曾尝试了很多地办法,  甚至想过在本地安装ruby环境, 在本地生成完sitemap.xml再push到github上面去, 但是这样太麻烦了,  不是正确而高效地处理方式.

我猜测问题的根源发生在`_config.yml`中, 但是改了N多次, 还是没有解决.

之后我就用了一个sitemap模板, 大概如下

![sitemap模板](https://i.loli.net/2019/11/04/Xk6PMAcHiblY25R.png)

但是一点都不好使, 在google console中, 一直显示我的站点地图是存在错误的.

哎, 一顿折腾之后, 算了吧, 就这样把.


今晚莫名地闲了起来, 可能是没什么计划了把,  就想解决一下这个问题.

然后google, `how to create github io sitemap`.  然后意外地点进了一个youtube地视频,  跟着视频上面修改了`_config.yml`.

关于`_config.yml`地修改如下.

```yml
# 这是我之前地
# gems: [jekyll-sitemap]


# 这是我修改之后的
gems:
      - jekyll-sitemap

```

修改了之后,  push上去.

发现, sitemap还是以前的模板样式. 

想着这个插件是不是默认不会覆盖, (这个我删了试过, 并没有自动生成), 抱着再试一试的心态, `git rm ./sitemap.xml`

然后push上去.

几秒之后, `https://yxler.cn/sitemap.xml`

我滴天.

![nice](https://i.loli.net/2019/11/04/4bTYpJHqoelVkgd.gif)

竟然好了.(我之前google过无数的方法.)

![hei](https://i.loli.net/2019/11/04/7XY4IPpwt82rBs6.png)



有一些不用着急的问题, 或许你就需要泡一泡它.

等它软了, 你也静下来了.

或许解决问题的最佳时机也就来了.