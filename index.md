---
---
<h1>Basic concepts</h1>

{% for doc in site.basic_concepts %}
<div class="grid-container">
  <div class="grid-x">
    <h2 id="{{ doc.title | slugify }}">{{ doc.title }}</h2>
  </div>
  <div class="grid-x grid-margin-x">

    {{ doc.content }}
  </div>
</div>
{% endfor %}

<h1>Advanced concepts</h1>

{% for doc in site.advanced_concepts %}
<div class="grid-container">
  <div class="grid-x">
    <h2 id="{{ doc.title | slugify }}">{{ doc.title }}</h2>
  </div>
  <div class="grid-x grid-margin-x">

    {{ doc.content }}
  </div>
</div>
{% endfor %}
