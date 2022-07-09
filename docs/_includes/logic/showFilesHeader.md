{% assign searchPath = {{include.searchPath}} | downcase %}

{% assign bShowHeader = false %}

{% for file in site.static_files %}
	{% capture searchFilePath %}{{searchPath}}{{ file.name }}{% endcapture %}
	{% assign filePath = file.path | downcase %}
	{% if filePath contains searchPath %}
		{% assign bShowHeader = true %}
		{% break %}
	{% endif %}
{% endfor %}

{% if bShowHeader == true %}
### {{include.textHeader}}
{% endif %}
