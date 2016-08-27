---
layout: post
title: "Share buttons"
date: 2016-05-15 16:25:06
description: Built-in share buttons!
share: true
---

# Share buttons

This theme comes with built-in share buttons. You can see them in the bottom f this post.
To turn them on in the header of your post add:

{% highlight yml %}
share: true
{% endhighlight yml %}

If you want to disable some of them - go to **_config.yml**:

{% highlight yml%}
share:
  facebook: true
  twitter: true
  gplus: true
  linkedin: true
  pinterest: true
  email: true
{% endhighlight yml%}

# Add new buttons

To add new buttons:

1. add icon name in **_config.yml**;
2. add section in **_includes/share.html**;
3. add styles in **css/theme.css**.