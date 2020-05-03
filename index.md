---
---

<h1>Basic concepts</h1>

{% for doc in site.basic_concepts %}
  <h2 id="{{ doc.title | slugify }}">{{ doc.title }}</h2>

  {{ doc.content }}
{% endfor %}

<h1>Advanced concepts</h1>

{% for doc in site.advanced_concepts %}
  <h2 id="{{ doc.title | slugify }}">{{ doc.title }}</h2>

  {{ doc.content }}
{% endfor %}
