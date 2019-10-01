---
layout: archive
permalink: /
title:
image:
  feature: logo_horizontal_blanco.jpg
---


## [Noticias](blog)

<div class="tiles">

<!-- Video de la Ofilibre -->
<article class="tile" itemscope itemtype="http://schema.org/Article">
<iframe src='https://tv.urjc.es/iframe/5d022dedd68b14cb308b6ae5' id='pumukitiframe' frameborder='0' border='0' width='100%' height='170px' allowfullscreen></iframe>
  <h2 class="post-title" itemprop="name"><a href="https://tv.urjc.es/video/5d022dedd68b14cb308b6ae5">La OfiLibre en dos minutos</a></h2>
  <p class="post-excerpt" itemprop="description">Breve presentación en video de los objetivos de la Oficina de Conocimiento y Cultura Libre.</p>

<!--  <a href="{{ site.url }}{{ post.url }}" title="{{ post.title }}" class="post-teaser">{% if post.image.teaser %}<img src="{{ site.url }}/images/{{ post.image.teaser }}" alt="teaser" itemprop="image">
    {% else %}<img src="{{ site.url }}/images/{{ site.teaser }}" alt="teaser" itemprop="image">{% endif %}</a>
  {% if post.date %}<p class="entry-date date published"><time datetime="{{ post.date | date: "%Y-%m-%d" }}" itemprop="datePublished">{{ post.date | date: "%B %d, %Y" }}</time></p>{% endif %}
  <h2 class="post-title" itemprop="name"><a href="{{ site.url }}{{ post.url }}">{{ post.title }}</a></h2>
  <p class="post-excerpt" itemprop="description">{{ post.excerpt | strip_html | truncate: 160 }}</p>
-->
</article><!-- /.tile -->

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
