---
layout: archive
permalink: /
title: "Últimas notas"
image:
  feature: logo_horizontal_blanco.jpg
---



<div class="tiles">
{% for post in site.posts %}
	{% include post-grid.html %}
{% endfor %}
</div><!-- /.tiles -->


<br style="clear: both;">

## [Guías de la OfiLibre](guias)

<div class="tiles">

{% assign guias = site.guias | sample: 10 %}
{% for guia in guias %}
  {% assign coll = site.collections | where: "label", "guias" | first %}
  {% assign dir = guia.url | split: "/" | last | replace: '.html', '' %}
	{% include guia-grid.html %}
{% endfor %}
</div><!-- /.tiles -->

<br style="clear: both;">

## [Fichas](fichas)

<div class="tiles">
{% assign fichas = site.fichas | sample: 10 %}
{% for ficha in fichas %}
  {% assign coll = site.collections | where: "label", "fichas" | first %}
  {% assign dir = ficha.url | split: "/" | last | replace: '.html', '' %}
	{% include ficha-grid.html %}
{% endfor %}
</div><!-- /.tiles -->
