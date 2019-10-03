---
layout: archive
permalink: /pres/
title: "Presentaciones de la OfiLibre"
image:
  feature: logo-ofilibre-blanco.jpg
---

{{ title }}

<div class="tiles">
{% for pres in site.pres %}
  {% assign coll = site.collections | where: "label", "pres" | first %}
  {% assign dir = pres.url | split: "/" | last | replace: '.html', '' %}
	{% include pres-grid.html %}
{% endfor %}
</div><!-- /.tiles -->

