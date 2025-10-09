
##### Lab: Excessive trust in client-side controls 
 
Comprar producto por el precio que queramos: 
```
POST /cart HTTP/2
Host: 0a25002d0359bdf0829101ea00890081.web-security-academy.net

productId=1&redir=PRODUCT&quantity=1&price=1
```

##### Lab: High-level logic vulnerability

![[Pasted image 20251004224104.png]]

##### Lab: Low-level logic flaw

![[Pasted image 20251004230825.png]]

![[Pasted image 20251004231338.png]]

##### Lab: Inconsistent handling of exceptional input

Crear un correo con esta longitud: 
```
ertertebbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbb@dontwannacry.com.exploit-0a4900c504273abc878214bb01ec0078.exploit-server.net
```

Acceder a `/admin`

##### Lab: Inconsistent security controls

![[Pasted image 20251005133447.png]]

Se despliega el admin panel. 

##### Lab: Weak isolation on dual-use endpoint

Cuando cambiamos la contraseña, ponemos el nombre de usuario del victima y omitimos el parámetro de `current-password`

Request original: 
```
csrf=1C0IUOGtIP872QjxEKuGJVgCbCx9qSgL&username=wiener&current-password=peter&new-password-1=test&new-password-2=test
```

Request maliciosa: 
```
csrf=1C0IUOGtIP872QjxEKuGJVgCbCx9qSgL&username=administrator&new-password-1=test&new-password-2=test
```

##### Lab: Insufficient workflow validation

Vemos lo que pasa cuando se compra un producto correctamente, y tomanos la URL. 

Luego antes de que se genere la verificación real de la cantidad de dinero que tenemos, lanzamos la petición que confirmaba la compra. Haciendo así que podamos comprar cualquier producto, sin importar la cantidad de dinero que tengamos. 

##### Lab: Authentication bypass via flawed state machine

![[Pasted image 20251005140804.png]]
Luego navegar a la pagina / y tendremos el rol de administrador.  

##### Lab: Flawed enforcement of business rules

![[Pasted image 20251005142722.png]]
Cuando uno se registraba, permitía usar el código de new customer nuevamente, si nos seguíamos registrando varias veces podíamos usar el mismo código. Ya que tomaba como si fuéramos clientes nuevos. 

![[Pasted image 20251005142835.png]]

##### Lab: Infinite money logic flaw

![[Pasted image 20251005212533.png]]![[Pasted image 20251005212545.png]]
![[Pasted image 20251005212558.png]]
![[Pasted image 20251005212613.png]]
##### Authentication bypass via encryption oracle

#Pendiente -> cuando entiendo padding oracle attack 



