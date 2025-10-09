
-  Enviar múltiples request en simultaneo, puede ser trigger race conditions o con grupos en burp repeater
- Turbo intruder
- 
## Lab: Limit overrun race conditions

![[Pasted image 20251008131333.png]]

## Lab: Bypassing rate limits via race conditions

![[Pasted image 20251008142846.png]]

![[Pasted image 20251008142821.png]]

## Lab: Multi-endpoint race conditions

![[Pasted image 20251008150214.png]]

![[Pasted image 20251008150221.png]]

## Lab: Single-endpoint race conditions

Enviar el grupo muchas veces
![[Pasted image 20251008152302.png]]
![[Pasted image 20251008152320.png]]


![[Pasted image 20251008152255.png]]

## Lab: Exploiting time-sensitive vulnerabilities

Debido a un bloqueo que permite solo enviar 1 petición por id de session, se debe enviar la petición con un id de sesion distinto. 

Debido a que el hash generaba el token en base a un timestamp y no incluia en ese hash el nombre de usuario podían tener dos usuarios un mismo token. 

![[Pasted image 20251008154113.png]]

![[Pasted image 20251008154122.png]]

![[Pasted image 20251008154140.png]]

simplemente en el link cambia el user por carlos.