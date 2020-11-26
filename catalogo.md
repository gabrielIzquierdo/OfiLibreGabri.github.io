---
layout: archive
permalink: /catalogo/
title: "Catálogo de materiales docentes de acceso abierto"
image:
  feature: logo-ofilibre-blanco.jpg
---

Este catálogo incluye materiales docentes de acceso abierto producidos por personal de la URJC. Si quieres enviarnos los materiales que hayas producido, para que sea incluidos en él, sigue las [instrucciones sobre cómo hacerlo](/blog/catalogo-materiales-libres).

<div class="tiles">
{% for ficha in site.catalogo %}
   {% include catalogo-grid.html %}
{% endfor %}

</div><!-- /.tiles -->
