---
layout: default
author: kryraj
---

¡Buenas! Bienvenidos a mí pagina de Github (principalmente hacking, de un par de ramas etc) el cual serviría para dar a conocer un poco lo que hago. Y como lo hago. 
kry

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
