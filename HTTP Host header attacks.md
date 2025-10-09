
Param Miner Extension -> https://portswigger.net/bappstore/17d2949a985c4b7ca092728dba871943

#### Algunas cosas a probar para identificar la vulnerabilidad: 

Inyectar más de un Host Header. 
```
GET /example HTTP/1.1
Host: example.com
Host: <payload>
```

Suministrar una URL absoluta
```
GET https://example.com/ HTTP/1.1
Host: <payload>
```

Agregar salto de línea (line wrapping)
```
GET /example HTTP/1.1
	Host: <payload>
Host: example.com
```

Inyectar headers que puedan anular el header Host
```bash
X-Forwarded-Host: <payload> 


X-Forwarded-Host #Indica el host original que envio el cliente antes de pasar por un componente intermedio

X-Host #Algunos proxies lo usan para indicar/forzar el host hacia el backend
X-Forwarded-Server #Indica el nombre del servidor/proxy que reenvía la petición.
X-HTTP-Host-Override #Permite sobreescribir el valor del Host que vera el backend
Forwarded #Es una manera de enviar un unico header lo que antes se hacía con varios de X-Forwarded-*
```


