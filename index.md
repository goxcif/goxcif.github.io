{% for post in site.posts %}
  {% assign count = post.content | size | divided_by: 3000 %}
  {% assign dashes = "-" %}
  {% for i in (1..count) %} {% assign dashes = dashes | append: "-" %} {% endfor %}
  [{{ post.title }}]({{ post.url }}) {{ dashes }}
{% endfor %}
