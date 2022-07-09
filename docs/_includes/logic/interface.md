{% if page.collection == "interfaces" %}

[comment]: # ( get main path for content )
	{% assign interfaceName = page.name %}
	{% capture mainPath %}/content/{{ page.collection }}/{{ interfaceName }}/{% endcapture %}

{:.no_toc}

[comment]: # ( show disclaimer content and table of contents)
{% include disclaimerContent.md %}
{% include logic/tableOfContents.md %}

[comment]: # ( list all content files - general and version specific )
{% assign fileMainList = site.documents | where_exp:"item", "item.path contains interfaceNameName" | where_exp:"item", "item.path contains '_includes/content/interfaces'" | sort: "name" %}

{% for group in site.data.orderSections %}
	{% assign headerType = group.headerType %}
		{% assign bShowHeader = true %}
		{% capture mainFileName %}_includes/content/interfaces/{{interfaceName}}/{{group.name}}.md{% endcapture %}
		{% assign mainFile = site.documents | find_exp:"item", "item.path == mainFileName" %}
		{% if mainFile != nil %}
{{headerType}} {{group.title}}				
{% include {{mainFileName | remove:'_includes/'}}	%}
		{% endif %}
{% endfor %}

[comment]: # (show schema files)
{% capture searchPath %}/assets/files/schema/{{interfaceName}}/{{ currentVersion}}/{% endcapture %}
{% include logic/showFilesHeader.md searchPath = searchPath textHeader = "Schema" %}
{% include logic/getFiles.md searchPath = searchPath %}

[comment]: # (show samples)
{% capture searchPath %}/assets/files/samples/{{interfaceName}}/{% endcapture %}
{% include logic/showFilesHeader.md searchPath = searchPath textHeader = "Samples" %}
{% include logic/getFiles.md searchPath = searchPath %}

[comment]: # (show documentation)
{% capture searchPath %}/assets/files/documentation/{{interfaceName}}/{% endcapture %}
{% include logic/showFilesHeader.md searchPath = searchPath textHeader = "Documentation Download" %}
{% include logic/getFiles.md searchPath = searchPath %}

{% endif %}
