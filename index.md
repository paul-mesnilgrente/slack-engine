---
---
Table of Content
================

Basic concepts
--------------

{% for doc in site.basic_concepts %}
  <h3>
    <a href="#{{ doc.title | slugify }}">{{ doc.title }}</a>
  </h3>
{% endfor %}

Advanced concepts
-----------------

{% for doc in site.advanced_concepts %}
  <h3>
    <a href="#{{ doc.title | slugify }}">{{ doc.title }}</a>
  </h3>
{% endfor %}

<hr />

Basic concepts
==============

{% for doc in site.basic_concepts %}
  <h2 id="{{ doc.title | slugify }}">{{ doc.title }}</h2>

  {{ doc.content }}
{% endfor %}

Advanced concepts
=================

{% for doc in site.advanced_concepts %}
  <h2 id="{{ doc.title | slugify }}">{{ doc.title }}</h2>

  {{ doc.content }}
{% endfor %}
