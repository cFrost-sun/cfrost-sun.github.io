---
layout: default
title: cFrost's Blog
---
## {{ page.title }}
Latest Blogs

{% for post in site.posts %}
* {{ post.date | date_to_string }} [{{ post.title }}]({{ site.baseurl }}{{ post.url }})
{% endfor %}

@2017 cFrost

Published with [GitHub Pages](https://pages.github.com/)
