---
layout: post
title: "Analytics, tags and comments"
date: 2015-08-01 16:25:06
description: Here you'll find how to setup analytics, tags and comments for your blog
tags:
 - jekyll
 - analytics
 - tags
 - comments
---

## Analytics
 
#### [Google Analytics](http://www.google.com/analytics/)

To enable Google Analytics create an account [here](https://analytics.google.com). Then add your tracking id in `config.xml`, it should look something like `UA-********-1`
 
#### [Yandex Metrica](http://metrica.yandex.com)
 
To enable Yandex Metrica you need to register, create a 'counter' and then copy-paste it's code in `/_includes/yandex-metrika.html` file.

## Tags

Using tags in Jekyll is a bit tricky topic, I used this approach: [charliepark.org/tags-in-jekyll/](http://charliepark.org/tags-in-jekyll/). You can read technical details there. To simply use tags provide list of tags in post header:

{% highlight bash %}
tags:
 - jekyll
 - analytics
 - tags
 - comments
{% endhighlight bash %}

And then before pushing your changes to github copy generated folder `/_site/tag` in your blog's root folder, since github pages will not generate it automatically.

## Comments

To enable [Disqus](http://disqus.com) register on the site and then just put your name in `_config.xml`. Comments could be switched on and off on per post basis, just put `comments: true` to enable them.