---
layout: archive
permalink: /catalogo/
title: "Catálogo de materiales docentes de acceso abierto"
image:
  feature: logo-ofilibre-blanco.jpg
---

<div class="tiles">
{% for ficha in site.catalogo %}
   {% include catalogo-grid.html %}
{% endfor %}

</div><!-- /.tiles -->
