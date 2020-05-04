---
---
<h1>Basic concepts</h1>

{% assign basic_sections = site.basic_concepts | sort: "order" | where: "lang", page.lang %}
{% for doc in basic_sections %}
<div class="grid-container fluid">
  <div class="grid-x">
    <h2 id="{{ doc.title | slugify }}">{{ doc.title }}</h2>
  </div>
  <div class="section-wrapper">
    {{ doc.content }}
  </div>
</div>
<hr />
{% endfor %}

<h1>Advanced concepts</h1>

{% assign advanced_sections = site.advanced_concepts | sort: "order" | where: "lang", page.lang %}
{% for doc in advanced_sections %}
<div class="grid-container">
  <div class="grid-x">
    <h2 id="{{ doc.title | slugify }}">{{ doc.title }}</h2>
  </div>
  <div class="grid-x grid-margin-x">
    {{ doc.content }}
  </div>
</div>
{% endfor %}
