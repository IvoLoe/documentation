{% if page.urlDocumentation == nil %}
{% elsif page.urlDocumentation == "" %}
{% else %}
The documentation can be found can be found at the following link  
>[{{page.urlDocumentation}}]({{page.urlDocumentation}})
{% endif %}