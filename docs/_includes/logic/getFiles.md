{% assign searchPath = {{include.searchPath}} | downcase %}

{% for file in site.static_files %}

	{% capture searchFilePath %}{{searchPath}}{{ file.name | downcase}}{% endcapture %}
	{% assign filePath = file.path | downcase %}

	{% if filePath contains searchFilePath %}

[{{ file.name }}]( {{file.path}} )
	{% endif %}
{% endfor %}
