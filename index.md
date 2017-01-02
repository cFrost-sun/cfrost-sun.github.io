---
layout: default
title: cFrost's Blog
---
## {{ page.title }}
Latest Blogs

{% for post in site.posts %}
* {{ post.date | date_to_string }} [{{ post.title }}]({{ site.baseurl }}{{ post.url }})
{% endfor %}
