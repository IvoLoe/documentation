{% assign headerText = "Table of Contents" %}

{% if {{include.headerText}} == nil %}
{% else %}
	{% assign headerText = {{include.headerText}} %}
{% endif %}

### {{headerText}}
{:.no_toc}

- TOC
{:toc}
