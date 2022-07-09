---
layout: default
name: interfaces
title: Standard Interfaces
---

[comment]: # (list interfaces by categories, see _data/categoriesInterfaces.yml )

{% for group in site.data.categoriesInterfaces %}
	{% assign bShowHeader = true %}
	{% assign interfaces = site.interfaces | where_exp:"item", "item.group contains group.name" %}
	{% for interface in interfaces %}
			{% if bShowHeader == true %}
### {{group.title}}
				{% assign bShowHeader = false %}
			{% endif %}
>[{{interface.title}}]({{interface.url}})
	{% endfor %}

	{% assign webservices = site.webservices | where_exp:"item", "item.group contains group.name" %}
	{% for webservice in webservices %}
			{% if bShowHeader == true %}
### {{group.title}}
				{% assign bShowHeader = false %}
			{% endif %}
>[{{webservice.title}}]({{webservice.url}})
	{% endfor %}	
{% endfor %}


[comment]: # (list interfaces without category)
{% assign bShowHeader = true %}
{% assign Empty = "(none)" %}

{% for interface in site.interfaces %}
	{% if interface.group != nil %}
	{% else %}
		{% if bShowHeader == true %}
### Other
			{% assign bShowHeader = false %}
		{% endif %}
>[{{interface.title}}]({{interface.url}})
	{% endif %}
{% endfor %}
	
{% for webservice in site.webservices %}
	{% if webservice.group != nil %}
	{% else %}
		{% if bShowHeader == true %}
### Other
			{% assign bShowHeader = false %}
		{% endif %}
>[{{webservice.title}}]({{webservice.url}})
	{% endif %}
{% endfor %}
