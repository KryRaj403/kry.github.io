---
layout: default
author: kryraj
---

¡Buenas! Bienvenidos a mi página de GitHub, la cual trata sobre mi camino y conocimientos en la ciberseguridad. 
Mi objetivo en este espacio es dar un poco de respaldo a lo que aprendo y lo que voy probando. Espero que alguien leyendo esto pueda quitarle alguna piedra del camino.

[Sobre mí](kry.github.io/about/)


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
