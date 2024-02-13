# Cookies 🍪

## **Que es una cookie?**
Las cookies son textos planos con poca dificultad para entender de que se tratan, guardados localmente en cada pc. Se puede saber para que sirve cada cookie con solo leerlo

1. Hacer una request a una page desde el server
2. El server responde con la pagina y las cookies
3. El browser muestra la pagina y guarda las cookies

Otras propiedades de las cookies son..

- Son simples, contienen un par de “variable” y “valor” en sí mismo, y pesan menos de 4KB, las cookies son válidas en un solo dominio, el host del dominio actual, excluyendo subdominios, aunque podemos hacer que una cookie sea válida también para los subdominios estableciendo una propiedad específica de dominio, es decir, en vez de que el dominio sea [www.facebook.com](http://www.facebook.com/), que el dominio sea facebook.com, pudiendo poner cualquier cosa además del www.
- Los sitios web suelen usar las cookies para **identificar a los usuarios**, y sus **preferencias**, también para trackear el **comportamiento en la web**, es por eso que guardan en sí mismos info del usuario y su estatus online.
- Las cookies también sirven para que un usuario entre a la misma web y no tenga que logearse una y otra vez, para lograr esto se crea una cookie única en cada navegador con las **credenciales del usuario**. Y cada vez que el usuario ingresa, el sitio checkea sí esa credencial existe, y sí no existe, la pide.
- Cuando se crea una cookie, también se crea con una fecha de expiracion bajo el label **Expires**, con una fecha y una hora en particular. Esto se hace por cuestiones de seguridad, ya que sí alguien logra “robar” una cookie de auteticacion, podria logearse con la cuenta de cierta persona, esto se evita ya que la cookie checkea sí se trata del mismo nevegador el que pretende acceder a esa cookie.

## **Que es el cookie tracking?**

Las cookies cumplen un rol importante no solo en la autenticacion sí no también en el track de tu comportamiento online, esto con un **tracking cookie**. Usualmente, muchas paginas usan herramientas de trackeo de otros lugares y no propios, lo cual hace que se pueda trackear la actividad de distintas web al mismo tiempo.

Tracking pixel: Es un pequeño pedazo de codigo que es pedido desde el dominio de la web que hace el tracking para insertar la cookie, es invisible para el usuario en la web.

```html
<img height="0" width="0" alt="" src="http://track.com"/>
```

El tracking cookie puede tener mucha informacion, como la IP y el navegador que se esta usando.

```html
TrackingID=3984720234; Ip=11.0.1.1; origin=stuff.com
```

El tracking puede servirle a muchas empresas para obtener informacion exacta acerca del perfil del usuario, y así, ofrecer publicidad, dando a muchos problemas de privacidad para los usuarios, es por eso que se pide permiso antes de guardar cookies.

## **Como creo una cookie con Javascript?**

1. Crear una funcion de Javascript

```jsx
function addCookie() {}
```

1. Le agregamos dos parametros a la funcion. **cname** 
es el nombre de la cookie, y **value** 
para el valor de la cookie

```jsx
function addCookie(cname, value) { }
```

1. Para crear la cookie debemos llamar a la funcion **document.cookie**

```jsx
function addCookie(cname, value) {
   document.cookie= cname + “=” + value + “;”
 }
```

1. Ya teniendo esta funcion creada, podemos crear una cookie por fuera de la funcion, como **username**

```jsx
function addCookie(cname, value) {
   document.cookie= cname + “=” + value + “;”
     }

addCookie(“username”,”denukennedy”);
```

1. Para ver la cookie creada, invocamos a un console log.

```jsx
function addCookie(cname, value) {
   document.cookie= cname + “=” + value + “;”
     }

addCookie(“username”,”denulemos”);
console.log(document.cookie);
```
