---
layout: default
author: kryraj
---

¡Buenas! Bienvenidos a mi página de GitHub, la cual trata sobre mi camino y conocimientos en la ciberseguridad.  

--kry

{% assign posts = site.posts%}

<div class="box">
	<div class ="list">
		{%- for post in posts -%}
		<a class="link post" href="{{ post.url | relative_url }}">
			<h3>{{ post.title | escape }}</h3>
		</a>
		{%- endfor -%}
	</div>
</div>
