{% if page.collection == "messageFormats" %}

[comment]: # ( get main path for content )
	{% assign messageFormatName = page.name %}
	{% capture mainPath %}/content/{{ page.collection }}/{{ messageFormatName }}/{% endcapture %}

{% assign versions = site.messageFormatVersions | where_exp:"item", "item.url contains messageFormatName" | sort: "sortId" | reverse %}

[comment]: # ( get version path for content )
	{% for version in versions %}
		{% assign currentVersion = version %}
		{% break %}
	{% endfor %}
	{% if currentVersion != nil %}
		{% capture currentVersionTitle %}{{currentVersion.title}}{% endcapture %}
		{% capture versionPath %}{{mainPath}}{{currentVersion}}/{% endcapture %}
	{% endif %}
{% else %}

[comment]: # ( get main path for content )
	{% for messageFormat in site.messageFormats %}
		{% if page.url contains messageFormat.name %}
			{% assign messageFormatName = messageFormat.name %}
			{% break %}
		{% endif %}
	{% endfor %}
	{% capture mainPath %}/content/messageformats/{{ messageFormatName }}/{% endcapture %}
	{% assign currentVersion = page %}
	{% assign currentVersionTitle = page.title %}
	{% assign versions = site.messageFormatVersions | where_exp:"item", "item.url contains messageFormatName" | sort: "sortId" | reverse %}

[comment]: # ( get version path for content )
	{% capture versionPath %}/content/messageformats{{page.url | remove: "/messageFormatVersions" | remove: ".html" }}/{% endcapture %}
{% endif %}


[comment]: # ( show only content if version is available )
{% if currentVersionTitle != nil %}

### Version: {{currentVersionTitle|upcase}}
{:.no_toc}

{% include disclaimerContent.md %}
{% include logic/tableOfContents.md %}

[comment]: # ( list all content files - general and version specific )
{% assign fileMainList = site.documents | where_exp:"item", "item.path contains messageFormatName" | where_exp:"item", "item.path contains '_includes/content/messageFormats'" | sort: "name" %}
{% assign fileVersionList = fileMainList | where_exp:"item", "item.path contains currentVersionTitle" | sort: "path" %}

{% for group in site.data.orderSections %}
	{% assign headerType = group.headerType %}
	{% if group.name == "SchemaInfo" %}
		{% capture mainFileName %} _includes/content/messageFormats/{{messageFormatName}}/{{group.name}}.md {% endcapture %}
		
		{% assign mainFile = site.documents | find_exp:"item", "item.path == mainFileName" %}
		{% if mainFile != nil %}
{{headerType}} {{group.title}}
{% include {{mainFileName | remove:'_includes/'}} %}
[{{currentVersion.linkSchema}}]({{currentVersion.linkSchema}})

		{% endif %}
	{% else %}
			{% assign bShowHeader = true %}
			{% capture mainFileName %}_includes/content/messageFormats/{{messageFormatName}}/{{group.name}}.md{% endcapture %}
			{% capture versionFileName %}_includes/content/messageFormats/{{messageFormatName}}/{{currentVersionTitle}}/{{group.name}}.md{% endcapture %}
			{% assign mainFile = site.documents | find_exp:"item", "item.path == mainFileName" %}
			{% assign versionFile = site.documents | find_exp:"item", "item.path == versionFileName" %}
			{% if mainFile != nil %}
				{% if bShowHeader %}
{{headerType}} {{group.title}}				
					{% assign bShowHeader = false %}						
				{% endif %}		
{% include {{mainFileName | remove:'_includes/'}}	%}

			{% endif %}
			
			{% if versionFile != nil %}
				{% if bShowHeader %}
{{headerType}} {{group.title}}					
					{% assign bShowHeader = false %}						
				{% endif %}			
{% include {{versionFileName | remove:'_includes/'}}	%}

	{% endif %}	
	{% endif %}	
{% endfor %}


[comment]: # (show schema files )
{% capture searchPath %}/assets/files/schema/{{messageFormatName}}/{% endcapture %}
{% include logic/showFilesHeader.md searchPath = searchPath textHeader = "Schema" %}
{% include logic/getFiles.md searchPath = searchPath %}

{% capture searchPath %}/assets/files/schema/{{messageFormatName}}/{{ currentVersion}}/{% endcapture %}
{% include logic/getFiles.md searchPath = searchPath %}

[comment]: # (show samples )
{% capture searchPath %}/assets/files/samples/{{messageFormatName}}/{% endcapture %}
{% include logic/showFilesHeader.md searchPath = searchPath textHeader = "Samples" %}
{% include logic/getFiles.md searchPath = searchPath %}

{% capture searchPath %}/assets/files/samples/{{messageFormatName}}/{{ currentVersionTitle}}/{% endcapture %}
{% include logic/getFiles.md searchPath = searchPath %}

[comment]: # (show documentation for download)
{% capture searchPath %}/assets/files/documentation/{{messageFormatName}}/{% endcapture %}
{% include logic/showFilesHeader.md searchPath = searchPath textHeader = "Documentation Download" %}
{% include logic/getFiles.md searchPath = searchPath %}

{% capture searchPath %}/assets/files/documentation/{{messageFormatName}}/{{ currentVersionTitle}}/{% endcapture %}
{% include logic/getFiles.md searchPath = searchPath %}


[comment]: # (list versions with changes info - if available)
### Change history
<table>
<th>Version</th><th>Schema</th><th>Comment</th>
	{% for version in versions %}
<tr>
		{% capture changeInfoPath %}/content/messageFormats/{{messageFormatName}}/{{version.title}}/Changes{% endcapture %}
		{% assign changeInfoFile = site.documents | find_exp:"item", "item.path contains changeInfoPath" %}		
			{% assign versionUrl = version.url %}	
			{% assign sMessageFormat = version.messageFormat %}
			{% capture sMessageFormatVersion %}{{ sMessageFormat }}/{{ version.messageFormatVersion }}{% endcapture %}
<td><a href="{{version.url}}">{{version.title}}</a></td>
<td><a href="{{version.linkSchema}}">{{version.linkSchema}}</a></td>
				{% if changeInfoFile != nil %}
					{% capture my_include %}{% include {{changeInfoPath}}.md %}{% endcapture %}
<td>{{ my_include | markdownify }}</td>			
				{% endif %}
</tr>
	{% endfor %}
</table>

{% endif %}
