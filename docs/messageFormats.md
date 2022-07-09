---
layout: default
name: messageFormats
title: Standard Message Formats
---

[comment]: # (list all messageFormats )
{% for messageFormat in site.messageFormats %}
[{{ messageFormat.title }}]({{messageFormat.url}})
{% endfor %}
