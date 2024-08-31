

### 💙**Excepciones en lugar de codigos de error**

Conviene encerrar todo nuestro codigo que puede fallar dentro de un try/catch y manejarlo mediante el mismo que logear los errores al momento en que suceden, el flujo normal queda mucho mas claro.

DON´T

```
public void restoreDatabase(){
    if (state() == 'OK'){
        realizoUnaAccion();
        if (state() == 'OK'){
            Logger.log("Todo salio bien");
        }
        else {
            Logger.log("Error");
        }
    }
    else {
        Logger.log("Error");
    }
}
```

DO

```
public void restoreDatabase(){
    try {
        realizoUnaAccion(); // Throws excepcionUna
        realizoOtraAccion(); // Throws excepcionDos
        Logger.log('Todo Salio Bien')
    }
    catch (excepcionUna e){
        Logger.log("Ocurrio un error");
    }
    catch (excepcionDos e){
        Logger.log("Ocurrio un error");
    }
}
```

Tambien seria ideal separar las funciones try/catch en una sola funcion, asi tenemos los caminos por separado. Cada funcion puede tratar con un solo nivel de abstraccion.

```
public void restoreDatabase(){
    try {
        realizoTodasLasAcciones(); // Throws excepcionUna y excepcionDos
    }
    catch (excepcionUna e){
        Logger.log("Ocurrio un error");
    }
    catch (excepcionDos e){
        Logger.log("Ocurrio un error");
    }
}
```

### 💙**Comentarios**

Se nos enseña mucho que debemos comentar mucho nuestro codigo. El buen codigo deberia poder leerse con casi ningun comentario.

- Solo agregar comentarios cuando es estrictamente necesario
- Los comentarios se desactualizan muy rapidamente ya que el codigo cambia mucho, son muy complicados de mantener, por eso no son aconsejables
- Si el codigo tiene demasiados comentarios es porque no se entiende (Refactorizacion necesaria) o porque los comentarios son demasiado obvios (eliminarlos en ese caso)
- Los comentarios de Javadocs en una API publica si son comentarios utiles.

DON´T

```
// --- En este caso el comentario esta ya que la condicion del if no es lo suficientemente explicativa por si misma ---

// Check if password is secure
if (password != null && password.length > 9 && !password.contains(username)){}
// --- Comentario generado automaticamente por javadoc, no nos suma nada, es innecesario ---

/*
* @param title The title of the book
* @param descripcion The description of the book
* 
*/
public void addBook(String title, String description){}

// --- NUNCA dejar codigo comentado pensando que lo vamos a usar en un futuro, para eso usamos control de versiones ---

//public void oldWayOfAddingBook(Request request){}
```

DO

```
// --- Reemplazamos la condicion confusa por una funcion que resume lo que hace ---

if(isSecure(password))
// --- Se explican cosas que suman al codigo, por ejemplo, que busca matchear un regex que no se puede ver a simple vista ---
// Matches Date in format 17-09-2022 18:05:15
Pattern.matches(regexDate, date);

// --- Se dejan cosas en TODO para un futuro ---
// TODO expect this method to change when..
public void login (User user) {}
```

### 💙**Formato Codigo**

Hay dos tipos de formato:

- Vertical: Densidad, distancia y orden
- Horizontal: Densidad e Indentación

Todos los miembros de un equipo deben escribir bajo las mismas reglas (Se recomienda el Styleguide de Google)

### 💙**Densidad**

Numero de lineas que hay en un bloque de codigo en concreto. Deben haber lineas en blanco que separen conceptos.

### 💙Densidad Horizontal

Mismo concepto pero de manera horizontal, entre caracteres

DON´T

```
public Integer tripleSum(Integer a, Integer b, Integer c){
   if (a==null||b==null||c==null){
       return null;
   }
   return a+b+c;
}
```

DO

```
public Integer tripleSum(Integer a, Integer b, Integer c) {
   if (a == null || b == null|| c == null){
       return null;
   }
   return a + b + c;
}
```

### 💙**Orden**

El orden de las funciones es para facilitar la lectura. En el ejemplo, primero ponemos la funcion `a()` que a su vez, consume a `b()` y `c()`, haciendo que la lectura sea ordenada.

```
public void a(){
    b();
    c();
}public void b(){}public void c(){}
```

### 💙**Distancias**

Se deben agrupar los conceptos relacionados y separar los conceptos que son diferentes con lineas en blanco, siempre y cuando sean necesarios.

DON´T

```
public String getUserConfig(){
    Session session = getSession();    User currentUser = session.get(username);    String configString = null;    if (currentUser.isValid()){
        Config config = currentUser.getConfig();
    }    return configString;
}
```

DO

```
public String getUserConfig(){
    Session session = getSession();
    User currentUser = session.get(username);    String configString = null;
    if (currentUser.isValid()){
        Config config = currentUser.getConfig();
    }    return configString;
}
```

### 💙**Indentación**

- Se recomiendan dos o cuatro espacios de indentacion
- El maximo recomendado de caracteres por linea es de 100 caracteres, si no, se tendrá que partir la linea
- Se deben seguir siempre las mismas reglas

DON´T

```
public Integer tripleSum(Integer a, Integer b, Integer c){
if (a==null||b==null||c==null){
return null;
}
return a+b+c;
}
```

## 💙**Gestión de errores**

- Se debe buscar un codigo limpio pero tambien robusto en si mismo
- La gestion de errores puede ensuciar el codigo, hay que prestarle especial atención
- Se deben devolver excepciones en lugar de códigos de error
- Se deben usar excepciones unchecked
- No devolver ni pasar `null`

### 💙**Excepciones en lugar de códigos de error**

Usando excepciones no hay porque tratar el error en el momento. Nos conviene separar la gestion de errores del codigo con el flujo correcto.

```jsx
public void sendShutDown() {
    try {
        tryToShutDown();
    }
    catch (DeviceShutDownError e) {
        logger.log(e);
    }
}

private void tryToShutDown() throws DeviceShutDownError {
    DeviceHandle handle = getHandle(DEV1); // Throws DeviceShutDownError
    // ...
    sendShutDown(handle);
}
```

### 💙**Unchecked Exceptions**

- Son excepciones que heredan de `RuntimeException`, como `NullPointerException`, muy común en Java, y en lenguajes compilados
- Se pueden tratar con try-catch pero no es necesario al 100%
- Las excepciones checked son las excepciones normales que heredan de `Exception`, si no son tratadas, el programa directamente no compila, ensucia mucho el codigo.
- Se recomienda usar checked Excepcions si se trata de una libreria critica donde se quiera obligar a quien usa la liberia a manejar estas excepciones.

```jsx
public class Driver {
    public void startDriving() {
        try {
            startCar();
        }
        catch (CarCrashException e) {
            logger.log(e);
        }
    }
}

public class Car {
    public void start() throws BatteryDeadException, EngineStartException {
        // ...
    }
}

public class Battery {
    public void start() throws BatteryDeadException {
        // ...
    }
}

public class Engine {
    public void start() throws EngineStartException {
        // ...
    }
}
```

### 💙**No Devolver null**

- No se debe devolver null en una funcion
- Al devolver null, obligamos que se chequee ese null, es una gran fuente de bugs, por ejemplo, es recomendable devolver una lista vacia en vez de un null
- No debemos pasar null en los parametros de una funcion, obligamos a que se chequee que estos parametros no sean nulos para ejecutar correctamente.

DON'T

```jsx
public void processRequestBatch() {
  List<Request> requestList = getRequestList();
  if (requestList != null) {
    for (Request r: requestList) {
      processRequest(r);
    }
  }
}
```

DO

```jsx
public void processRequestBatch(){
  List<Request> requestList = getRequestList();
  for (Request r: requestList) {
    processRequest(r);
  }
}
```

---

# Code Smells

## 🟢 **¿Qué es un Code Smell?**

Son sintomas de que el codigo no es todo lo limpio que deberia. Hay distintos tipos de code smells como:

- En los comentarios
- En el entorno de desarrollo
- En las funciones
- Code Smells Generales
- Code Smells en Java u otros lenguajes
- En los nombres
- En los tests

Nadie se va a volver experto teniendo esta lista de memoria, nunca va a ser una lista completa de todos los problemas que puede tener el código.

---

## 🟢**Code Smells en los Test**

### 👉**Tests insuficientes**

Se deben probar todas las posibilidades de una funcion. Si no se prueban todas las condiciones, los tests son insuficientes.

### 👉**No usar una herramienta de cobertura**

Se muestran claramente las lineas que no estan siendo cubiertas por los tests. Son faciles de instalar.

### 👉**Evitar los tests triviales**

Aunque una funcionalidad parezca trivial, hacer un test igual. Es una gran fuente de bugs.

### 👉**Test ignorado o comentado**

Segun Robert C. Martin “A veces tenemos dudas sobre los detalles de una funcionalidad, porque los requisitos no están claros. Podemos expresar estas dudas con un test comentado, o con un test con `@ignore`. La opcion que elijas depende de si tu test compila o no"

Si hay dudas en los tests, hay dudas en los requisitos

### 👉**No testear condiciones limite**

Son los mayores generadores de bugs, hay que testearlos. No solo testear las condiciones normales del código.

### 👉**No buscar bugs de forma exhaustiva**

Los bugs tienden a estar cerca unos de otros. Si encontrás un bug, revisar, pueden haber más

### 👉**Los patrones de fallo son reveladores**

Si vemos que una funcion falla cuando mandamos un String con caracteres en blanco, debemos manejar esto.

### 👉**La cobertura de codigo es reveladora**

Se pueden encontrar los motivos de fallo con las lineas que no estan analizadas

### 👉**Tests lentos**

Los tests deben ser rapidos, ya que deben ser ejecutados muchas veces al dia.

---

## 🟢**Code Smells en Java**

### 👉**Listas de imports muy largas**

Si usas dos o mas clases de un paquete, importar directamente todo el paquete.

### 👉**Herencia de constante**

Se debe evitar el uso de herencia para estos mecanismos. Por ejemplo `Game` hereda de `GameConstants`. Se debe usar `GameConstants.NUMBER_OF_LEVELS` o importarlo directamente. No se debe usar herencia para crear estos mecanismos.

```jsx
public class EasyGame extends Game {
    private int currentLevel;
    public Double calculateProgress(){
        return currentLevel / NUMBER_OF_LEVELS;
    }
}

public abstract class Game implements GameConstants {
    public Double calculateProgress();
}

public interface GameConstants {
    public static final int NUMBER_OF_LEVELS = 10;
}
```

### 👉**Enums vs Constantes**

Usar enums siempre y cuando sea posible. Es mas potente en funcionalidad y claridad. Tambien nos permiten declarar funciones abstractas para cada valor, como `numberOfLevels` por nivel.

```jsx
public class Game {
	private int currentLevel;
	DifficultyLevel difficultyLevel;

	public int levelsLeft() {
		return difficultyLevel.numberOfLevels() - currentLevel;
	}
}

public enum DifficultyLevel {
	EASY {
		public int numberOfLevels() {
			return 20;
		}
	},
	MEDIUM {
		public int numberOfLevels() {
			return 30;
		}
	},
	HARD {
		public int numberOfLevels(){
			return 50;
		}
	}

	public abstract int numberOfLevels();
}
```

Se evitan los condicionales por nivel de dificultad en el ejemplo.

---

## 🟢**Code Smells en Nombres**

### 👉**Nombres en nivel de abstraccion incorrecto**

Si tenemos una clase `FtpFileDownloader` que implementa la interfaz, ya no recibiria una web url.

```jsx
public interface FileDownloader {
	File download(String webUrl);
}
```

### 👉**No usar nomenclatura estandar**

- Por ejemplo, usar la palabra `Singleton` para clases que usen ese patrón, la palabra `Controller` cuando se trata de un controlador, etc..
- Sobreescribir metodos `toString` que ya tienen las clases Java en lugar de crear un metodo propio. No reinventar la rueda

### 👉**No usar nombres largos para largos alcances**

- Se pueden usar nombres de pocos caracteres para alcances cortos, pero si el alcance es mayor, el nombre largo es necesario. El ejemplo de un alcance acotado es en el caso de `i` para un bucle.

```jsx
for (int i = 0; i < numberOfRequests; i++) {
	processRequest(requests.get(i));
}

// NOPE
public class Car {
	private int hp;
}

// SIP
public class Car {
	private int horsePower;
}
```

### 👉**Usar Codificaciones**

- Evitar los nombres con codificaciones que distraigan al lector
- Un ejemplo malo son las variables, por ejemplo `intVariable` o `strName`. No es necesario agregar el prefijo `str` si ya se sabe que se trata de un String.

### 👉**Ocultar efectos secundarios**

Si existen efectos secundarios, los nombres de las variables y funciones deben describirlo. Por ejemplo, en el caso de esta funcion `login`, se hace mas de una cosa, y no es aclarado. Se debe separar en dos funciones o cambiar el nombre (recomendable la primera)

```jsx
public Boolean login (User user) {
    User databaseUser = userRepository.findByUsername(user.getUsername());

    if (validCredentials(user, databaseUser)) {
        return true;
    }

    // Esto esta de mas
    if (checkNumberOfTries(user.getIp()) > MAX_TRIES) {
        blockUser(user);
    }

    return false;

}
```

---

## 🟢**Code Smells en los Comentarios**

Hay distintos sintomas dentro de un Code Smells. En este primer punto son:

### 👉**Informacion Inapropiada**

Todo comentario que contenga informacion que este mejor documentada en otro sistema debe ser movida.

Por ejemplo en este caso, el autor del codigo ya esta siendo registrado por Git, no es necesario que haya un comentario que lo indique, lo mismo con los TODO

DON´T

```
//@author Denisse Lemos
public class Comments {}
```

### 👉**Comentario Obsoleto**

Los comentarios deben estar actualizados al codigo actual para que no confundan al programador.

### 👉**Comentario Redundante**

Comentarios innecesarios de, por ejemplo, javadoc. Que no aportan valor al codigo

### 👉**Comentario mal redactado**

Los comentarios utiles deben estar bien redactados, sin faltas de ortografia y directo.

### 👉**Código Comentado**

Nunca dejar codigo comentado, ya que se guarda el registro de cambios en Git. El codigo que no se necesita, se borra.

---

## 🟢**Code Smells en el entorno**

### 👉**Que la compilacion requiera mas de un paso**

Se debe ser capaz de hacer un checkout del codigo fuente con un solo comando y compilarlo con otro solo comando. Si el codigo es simple y no tiene dependencias, es facil de hacer, pero a medida que va creciendo en dependencia, puede ser que se necesite compilar las dependencias por separado para luego compilar el proyecto, para esto se crearon muchas herramientas para automatizar el build del codigo. En Java el mas usado es Maven o Apache, en JS es Grunt, entre otros…

```
git clone {nuestroProyecto}
mvn install
```

### 👉**Los test requieren mas de un paso**

Los test se deben ejecutar con un unico comando facil, rapido y obvio, al igual que la compilacion. Si esto no se cumple, pueden haber altas chances de que los test no suelan correrse por el trabajo que conllevan en si mismos. La idea es ejecutarlos muy seguido.

Tambien hay herramientas de automatizacion para los tests, con comandos especificos para los tests. En maven existe `mvn test`

---

## 🟢**Code Smells en Funciones**

### 👉**Demasiados parametros / argumentos**

Las funciones mas faciles de leer son las que no reciben parametros. Mientras mas parametros, mayor es la complejidad. No se recomiendan mas de 3 parametros, y si es el caso, se recomienda refactorizar, separando esta funcion en varias funciones mas pequeñas o encapsulando los argumentos en una clase

👉**Intenciones ocultas dificiles de apreciar**

- El codigo debe ser lo mas expresivo posible
- De nada vale un codigo que ocupa poco espacio si no lo entendemos. Mejor aclarar lo mas posible antes que priorizar el ahorro de caracteres

En este ejemplo podemos ver que se ahorraron muchas letras en los nombres de las funciones, haciendo que sea muy confuso de leer

```javascript
public int otCalc(){
	return isWkn * wkRte + (int) Math.round(0.5 * wkRte * ...);
}
```

### 👉**Una función recibe parametros de salida**

Los argumentos de salida con los que se le pasan a una funcion para que esta guarde el resultado, en vez de usar un return, como en el caso del ejemplo el parametro `int* result`, que solo sirve para guardar el resultado en una variable global. Siempre se debe usar el `return`

Estos argumentos son muy usados en lenguajes como C.

```
#include <stdio.h>

void suma (int input1, int input2, int* result) {
  *result = input1 + input2;
}

int main() {
  int result;
  suma(1,2,&result);
  printf("Result: %d\n", result);

  return 0;
}
```

### 👉**Comportamiento incorrecto en los limites del codigo**

Muchos bugs del codigo estan en los limites de los condicionales o bucles de los codigos.

```java
private final List<String> thirtyOneDayMonths = Arrays.asList("January", "March", "May", "July", "August", "October", "December");

private final List<String> thirtyDayMonths = Arrays.asList("April", "June", "September", "November");

public int getNumberOfDaysInMonth(String month) {
  if (thirtyOneDayMonths.contains(month)) {
    return 31;
  }
  if (thirtyDayMonths.contains(month)) {
    return 30;
  }
  return 28;
}
```

Si un año es biciesto, el codigo seria incorrecto

### 👉**No encapsular las condiciones limite**

Las condiciones limite son los focos mas importantes de bug. Es importante encapsularlas bien y tenerlas bien definidas.

Si tenemos que definir una variable mas para que quede mas claro, lo hacemos.

```jsx
if (level + 1 == game.getMaxLevel()) {
	loadFinalBoss();
}

Integer nextLevel = level + 1;
if (nextLevel == game.getMaxLevel()) {
	loadFinalBoss();
}
```

### 👉**Metodos estaticos inapropiados**

Los metodos estaticos no operan bajo ninguna instancia. Se debe pensar si un metodo es realmente estatico antes de declararlo como tal.

En el caso del ejemplo, deberia ser una funcion de `Employee` si deseamos que sea polimorfica y calcule distinto dependiendo del tipo de Employee.

```java
public class HourlyPayCalculator {
    public static Double calculatePay(Employee employee, Double overtimeRate)
}
```

### 👉**No ser precisos**

Se debe tener precision sobre las decisiones que se tomen sobre el codigo. No ser vago con las decisiones.

- Si se va a calcular dinero, usar enteros y manejar el redondeo
- Si se va a tratar con concurrencia, asegurar que no hayan carreras criticas
- Si hay metodos que pueden lanzar excepciones, tratarlas.

### 👉**Darle mas peso a convenciones que a la estructura**

Las convenciones son importantes pero el diseño del software es mas importante. No uses MVC si no es la apropiada para tu codigo.

### 👉**Navegacion Transitiva**

- Un modulo debe saber lo menos posible sobre los demás
- Si A usa a B, y B usa a C, evitar `a.getB().getC()`, desde A hay que acceder a B para acceder a C. Y si queremos intercalar una clase D, tenemos que modificar estos llamados agregando `getD()` en el medio.
- Segun la Ley de Demeter hay que escribir codigo timido, A debe ser timido y no llamar a una funcion de C a traves de B. Debe conocer lo menos posible sobre los demás.
- Asi es como se forman las arquitecturas rigidas, si surge la necesidad de modificarla, será muy costoso.

### 👉**Que las funciones hagan mas de una cosa**

Las funciones deben hacer una sola cosa, ya que hace que sean mas legibles y faciles de testear.

### 👉**Usar condicionales negativos**

Siempre es mejor usar condicionales positivos. Se debe cambiar el nombre a lo que sea necesario

```java
if (!isNotValid) {
    // ...
}

if (isValid) {
    // ...
}
```

### 👉**Envidia del ambito de otra clase**

Cuando una clase esta interesada en demasiada info de otra clase. Es un problema de diseño. Lo ideal es que los metodos de una clase esten interesadas en su propia clase, y no tenga que usar variables ni funciones de otra clase.

En el ejemplo, `GameRankCalculator` tiene "envidia" de la clase `user`, ya que pide demasiadas cosas del mismo

```java

public class GameRankCalculator {
    public Rank calculateRank(User user) {
        Double hoursPlayed = user.getHoursPlayed();
        int gamesPlayed = user.getGamesPlayed();
        int achievements = user.getAchievements();

        Double winPercentage = gamesPlayed / user.getGamesWon();
        Double rank = hoursPlayed * gamesPlayed * achievements * winPercentage;

        return new Rank(rank);
    }
}
```

### 👉**Acomplamiento artificial**

Los elementos que no dependan entre si no deben estar acoplados. Por ejemplo, en el caso de PI dentro del ejemplo, nos conviene hacer una variable global del mismo, ya que es un valor que puede ser usado por cualquier otro elemento del codigo y no es algo exclusivo del Circulo.

```java
public class Circle {
    private static final Double PI = 3.14159265359;
    private Double radius;
    public Circle(Double radius) {
        this.radius = radius;
    }
    public Double calculateArea() {
        return PI * radius * radius;
    }
}
```

### 👉**Clases base dependen de las derivadas**

Las clases base no deben saber nada de sus derivadas. En el ejemplo, `PlaneGeometry` no deberia tener logica asociada ni a `Rectangle` ni a `Triangle` que lo heredan, deberian encargarse ellos de sus especificaciones de clase. La clase base no deberia depender de quien la hereda.

Se recomendaria aplicar `getNumberOfSides` como abstracto.

```java
public class PlaneGeometry {
    public int getNumberOfSides() {
        if (this instanceof Rectangle) {
            return 4;
        }
        if (this instanceof Triangle) {
            return 3;
        }
        return 0;
    }
}

public class Rectangle extends PlaneGeometry {}
public class Triangle extends PlaneGeometry {}

```

### 👉**Ser arbitrario**

Si la estructura del codigo es arbitraria, otros haran lo mismo sobre el, ensuciando mas el codigo. Las decisiones deben ser consistentes.


### 👉**No usar convenciones**

Es importante seguir las normas establecidas para un lenguaje de programacion en particular, como las formas de llamar a las variables, tamaños de lineas, etc..

Todo el equipo debe seguir las mismas normas, si no, el codigo no sera homogeneo

### 👉**Responsabilidad fuera de lugar**

- El codigo debe ser escrito en el lugar mas natural para un lector
- No escribir el codigo donde mas convenga, si no, donde se esperaria leer
- Esto puede ir acoplado al ejemplo de PI acoplado con Circle, cuando quedaria mas claro que exista en una clase de constantes matematicas llamada `Math`

### 👉**Funciones que no dicen lo que hacen**

Los nombres de las funciones deben ser explicativos, por ejemplo:

```
Date newDate = oldDate.add(5)
```

¿Qué estamos agregando acá? ¿Son 5 horas, minutos, segundos, dias?

### 👉**No conocer el algoritmo**

Se deben entender los algoritmos complicados, si no los entendemos, el codigo no será todo lo limpio que puede ser. Si no se entiende, se debe refactorizar.

### 👉**Pasar Flags como argumento**

Un flag, boolean, indica que la funcion hace mas de una cosa. Cada funcion debe hacer unicamente una cosa.

```
private static final Double PREMIUM_DISCOUNT_FACTOR = 0.2;
private static final Double REGULAR_DISCOUNT_FACTOR = 0.1;

public Double calculateDiscount(Item item, boolean isPremium) {
  if (isPremium) {
    return item.getPrice() * PREMIUM_DISCOUNT_FACTOR;
  }
  return item.getPrice() * REGULAR_DISCOUNT_FACTOR;
}

```

### 👉**Funciones muertas — Dead Functions**

Son funciones que no se llaman nunca. Deben eliminarse, y si llegamos a necesitarlas en un futuro, se guardan en el historial de Git.

## 🟢**Code Smells Generales**

### 👉**Mezclar muchos lenguajes en un solo fichero**

En el ejemplo podemos ver que se mezclan Java y HTML en un mismo archivo. A veces puede ser razonable (CSS + JS), pero en casos como el ejemplo, solo ensucia el codigo.

```
public String getUserProfilePage (User user) {
  return "<html> <body>...."
}
```

### 👉**Comportamiento obvio no implementado**

Por ejemplo tenemos la siguiente función:

```
Month month = MonthDate.StringToMonth(String monthName);
```

Esperamos que esta funcion pase el parametro `July` a un enum `Month.JULY`. Que haga lo mismo con `jULY` y `july`, si el codigo no hace lo que se espera en estos casos, se pierde confianza en el, y se debe estar revisando lo que hace cada linea.

👉**No usar variables explicatorias**

Crear variables intermedias hace que el codigo quede mucho mas claro. Por ejemplo, en la imagen podemos ver la primera imagen que es una funcion de una sola linea, que queda mucho mas clara en su segunda version, agregando una variable intermedia para calcular un cateto.

```javascript
public Double calcularArea(Double hipotenusa, Double cateto) {
    return Math.sqrt(hipotenusa*hipotenusa - cateto*cateto) * cateto / 2;
}

public Double calcularArea (Double hipotenusa, Double cateto) {
    Double cateto_2 = Math.sqrt(hipotenusa*hipotenusa - cateto*cateto);
    
    return cateto * cateto_2 / 2;
}
```

No se debe sacrificar claridad por menos lineas.

👉**Usar numeros magicos en lugar de constantes**

Se deben pasar numeros que no se sabe que son como en el primer caso a constantes, como sucede en su segunda refactorizacion.

```javascript
public Double getAnnualSalary() {
    return 200.0 * 0.6..
}

public Double getAnnualSalary() {
    return WORKING_DAYS * WORKING_HOURS_PER_DAY...
}
```

### 👉**Anular mecanismos de seguridad**

Deshabilitar warnings del compilador, ignorar o comentar tests fallidos, etc.. Por algo estan ahi!

### 👉**Duplicidad**

Tener codigo repetido entre funciones y clases es algo común, es una oportunidad perdida para generar una nueva abstraccion. Si tenemos duplicidad entre varias clases, puede indicar la necesidad de una nueva jerarquia.

Ademas es dificil de mantener, ya que si cambiamos una parte de ese codigo, debemos repetirlo en todos los lugares en donde se encuentra.

### 👉**Código en el nivel de abstracción incorrecto**

El método `changeRadioChannel` esta mal, ya que no todos los vehículos tienen radio. El método no debería estar en esa clase tan general como lo es `Vehicle`

```
public class Vehicle() {
  public void turnOn();
  public void turnOff();
  public Integer getLeftAutonomyInKm();
  public void changeRadioChannel(RadioCHannel selectedChannel);
}
```

### 👉**Distancia Vertical**

Las variables y funciones deben estar cerca de donde se usen

- Las variables locales deben ser declaradas antes de ser usadas
- Las funciones privadas deben estar debajo de la primera funcion que las usa
- No queremos que codigo local este puesto a muchas lineas de distancia de donde es usado ya que hace que sea complicado de leer

### 👉**Inconsistencia**

- Si haces algo de una forma, hacelo siempre de la misma forma, tanto nombre como logica
- Si, por ejemplo, se elije la palabra “delete” para eliminar algo, no cambiarlo en otro lado por “remove”
- Esto hace que nuestro codigo sea mucho mas facil de leer para otros y para uno mismo

### 👉**Demasiada Información**

Poner demasiados metodos, demasiados get/set, etc, cuando no sabemos si hace falta. Los modulos bien definidos tienen interfaces muy pequeñas que permiten hacer mucho con pocos metodos.

- Exponer solo lo necesario
- Esconde los datos, ofrece operaciones, que no todo tenga get/set si no es necesario.

### 👉**Código Muerto**

Código que no se usa debe ser eliminado. Por ejemplo:

- Funciones que no se llaman
- Condiciones en el switch/case que no se dan nunca, al igual que con el if
- try/catch con excepciones que nunca se lanzan

Se debe borrar sin miedo, Git recuerda todo.

### 👉**Basura**

Son partes que no sirven de nuestro codigo, como por ejemplo:

- Constructores sin implementacion puestos por defecto
- Getters y Setters de todas las variables privadas de una clase que puede ser que no se usen nunca
- Variables y funciones que no se usan nunca


### Acoplamiento (Evitar)

Grado en que dos modulo software estan relacionados entre si. Con un buen diseño, se crean modulos poco acoplados, haciendo que si se modifica un modulo, afecte lo menos posible al resto (Misma logica con las arquitecturas de microservicios o microfrontends)

```javascript
// -----------Módulo con acoplamiento alto-----------
const HighCouplingModule = (function() {
    // Función para calcular el área de un círculo
    function calculateCircleArea(radius) {
        return Math.PI * radius ** 2;
    }

    // Función para calcular el área de un rectángulo
    function calculateRectangleArea(length, width) {
        return length * width;
    }

    // Función para imprimir el área de una forma geométrica
    function printArea(shape, ...args) {
        let area;
        if (shape === 'circle') {
            area = calculateCircleArea(...args);
        } else if (shape === 'rectangle') {
            area = calculateRectangleArea(...args);
        } else {
            throw new Error("Shape not supported");
        }
        console.log(`The area of the ${shape} is: ${area}`);
    }

    // Exponer las funciones públicas del módulo
    return {
        printArea
    };
})();

// Uso del módulo con acoplamiento alto
HighCouplingModule.printArea('circle', 5); // Output: The area of the circle is: 78.53981633974483
HighCouplingModule.printArea('rectangle', 4, 6); // Output: The area of the rectangle is: 24

// -----------Módulo con acoplamiento bajo-----------

const LowCouplingModule = (function() {
    // Función para calcular el área de un círculo
    function calculateCircleArea(radius) {
        return Math.PI * radius ** 2;
    }

    // Función para calcular el área de un rectángulo
    function calculateRectangleArea(length, width) {
        return length * width;
    }

    // Exponer las funciones públicas del módulo
    return {
        calculateCircleArea,
        calculateRectangleArea
    };
})();

// Módulo independiente
const IndependentModule = (function() {
    // Función para imprimir el área de una forma geométrica
    function printArea(shape, ...args) {
        let area;
        if (shape === 'circle') {
            area = LowCouplingModule.calculateCircleArea(...args);
        } else if (shape === 'rectangle') {
            area = LowCouplingModule.calculateRectangleArea(...args);
        } else {
            throw new Error("Shape not supported");
        }
        console.log(`The area of the ${shape} is: ${area}`);
    }

    // Exponer las funciones públicas del módulo
    return {
        printArea
    };
})();

// Uso del módulo con acoplamiento bajo
IndependentModule.printArea('circle', 5); // Output: The area of the circle is: 78.53981633974483
IndependentModule.printArea('rectangle', 4, 6); // Output: The area of the rectangle is: 24

```



### Cohesion (Tener mucho)

Es el grado en que los elementos de un modulo estan relacionados entre si. Mientas mas alta la cohesion en el codigo, mejor

En el ejemplo la cohesion es baja, las funciones se relacionan con distintas variables. Se podria separar en 2 clases, una que sea para el manejo de niveles y otra para el manejo del jugador.

Ejemplo de alta cohesion:

```javascript
// Módulo para manejar operaciones relacionadas con formas geométricas
const GeometryModule = (function() {
    // Función para calcular el área de un círculo
    function calculateCircleArea(radius) {
        return Math.PI * radius ** 2;
    }

    // Función para calcular el perímetro de un círculo
    function calculateCirclePerimeter(radius) {
        return 2 * Math.PI * radius;
    }

    // Función para calcular el área de un rectángulo
    function calculateRectangleArea(length, width) {
        return length * width;
    }

    // Función para calcular el perímetro de un rectángulo
    function calculateRectanglePerimeter(length, width) {
        return 2 * (length + width);
    }

    // Exponer las funciones públicas del módulo
    return {
        calculateCircleArea,
        calculateCirclePerimeter,
        calculateRectangleArea,
        calculateRectanglePerimeter
    };
})();

// Uso del módulo de geometría
console.log(GeometryModule.calculateCircleArea(5)); // Output: 78.53981633974483
console.log(GeometryModule.calculateRectanglePerimeter(4, 6)); // Output: 20

---
    
// Módulo con baja cohesión
const LowCohesionModule = (function() {
    // Función para calcular el área de un círculo
    function calculateCircleArea(radius) {
        return Math.PI * radius ** 2;
    }

    // Función para convertir un número a binario
    function decimalToBinary(number) {
        return number.toString(2);
    }

    // Función para generar un número aleatorio
    function generateRandomNumber(min, max) {
        return Math.floor(Math.random() * (max - min + 1) + min);
    }

    // Función para validar un correo electrónico
    function validateEmail(email) {
        return /\S+@\S+\.\S+/.test(email);
    }

    // Exponer las funciones públicas del módulo
    return {
        calculateCircleArea,
        decimalToBinary,
        generateRandomNumber,
        validateEmail
    };
})();

// Uso del módulo de baja cohesión
console.log(LowCohesionModule.calculateCircleArea(5)); // Output: 78.53981633974483
console.log(LowCohesionModule.decimalToBinary(10)); // Output: "1010"
console.log(LowCohesionModule.generateRandomNumber(1, 100)); // Output: un número aleatorio entre 1 y 100
console.log(LowCohesionModule.validateEmail("example@example.com")); // Output: true


```

### Single Responsibility Principle

- Puede ser un nombre confuso. No quiere decir que un modulo debe hacer una sola cosa, si no que **un modulo debe tener una unica razon para cambiar**
- Un modulo debe ser responsable de solo un usuario o interesado del sistema
- Un modulo es una clase dentro de POO.

Hay ciertos sintomas que diagnostican el no cumplimiento de SRP

```javascript
// Clase con una única responsabilidad: gestionar la lógica relacionada con el manejo de usuarios
class UserManager {
    // Método para agregar un nuevo usuario a la base de datos
    addUser(user) {
        // Lógica para agregar un usuario a la base de datos
        console.log(`User added: ${user.name}`);
    }

    // Método para enviar un correo electrónico de bienvenida a un nuevo usuario
    sendWelcomeEmail(user) {
        // Lógica para enviar un correo electrónico de bienvenida al nuevo usuario
        console.log(`Welcome email sent to: ${user.email}`);
    }

    // Método para generar un informe de usuarios registrados
    generateUserReport() {
        // Lógica para generar un informe de usuarios registrados
        console.log("Generating user report...");
    }
}

// Clase que representa un usuario del sistema
class User {
    constructor(name, email) {
        this.name = name;
        this.email = email;
    }
}

// Uso de las clases
const userManager = new UserManager();

const newUser = new User("John", "john@example.com");
userManager.addUser(newUser); // Output: User added: John
userManager.sendWelcomeEmail(newUser); // Output: Welcome email sent to: john@example.com
userManager.generateUserReport(); // Output: Generating user report...

```


### Open Closed Principle

**Un artefacto de software debe estar abierto para su extension, pero cerrado para su modificacion**. Se debe poder aumentar la funcionalidad sin modificar el funcionamiento ya existente.

¿Cómo se consigue?

- Separando el Software en componentes de alta cohesion.
- Que la direccion de las dependencias sea la correcta (se vera con mas detalle mas adelante).
- Se deben proteger a los componentes de alto nivel de los cambios en los componentes de bajo nivel.

```
// Clase base para representar formas
class Shape {
    area() {
        // Método para calcular el área de la forma
        throw new Error("This method must be overridden");
    }
}

// Clase para representar un rectángulo
class Rectangle extends Shape {
    constructor(width, height) {
        super();
        this.width = width;
        this.height = height;
    }

    area() {
        // Sobrescribimos el método para calcular el área de un rectángulo
        return this.width * this.height;
    }
}

// Clase para representar un círculo
class Circle extends Shape {
    constructor(radius) {
        super();
        this.radius = radius;
    }

    area() {
        // Sobrescribimos el método para calcular el área de un círculo
        return Math.PI * this.radius ** 2;
    }
}

// Uso de las clases
const rectangle = new Rectangle(4, 5);
const circle = new Circle(3);

console.log(rectangle.area()); // Output: 20
console.log(circle.area()); // Output: 28.274333882308138

```

###  Interface Segregation Principle

El Principio de Segregación de Interfaces (ISP) establece que una clase no debe depender de interfaces que no utiliza. En otras palabras, las interfaces deben ser lo suficientemente específicas para cada cliente, evitando que los clientes dependan de métodos que no necesitan.

Supongamos que tenemos una interfaz `Worker` (Trabajador) que define diferentes acciones que un trabajador puede realizar. Sin embargo, algunos tipos de trabajadores solo necesitan implementar un subconjunto de estas acciones. Aplicaremos el ISP para dividir la interfaz `Worker` en interfaces más específicas:

```javascript
// Interfaz original para un trabajador
class Worker {
    work() {}
    eat() {}
    sleep() {}
}

// Interfaz para un trabajador que solo necesita trabajar
class WorkOnlyWorker {
    work() {}
}

// Interfaz para un trabajador que solo necesita comer
class EatOnlyWorker {
    eat() {}
}

// Interfaz para un trabajador que solo necesita dormir
class SleepOnlyWorker {
    sleep() {}
}

// Clase para un programador que solo necesita trabajar
class Programmer extends WorkOnlyWorker {
    work() {
        console.log("Programming...");
    }
}

// Clase para un camarero que solo necesita trabajar y comer
class Waiter extends WorkOnlyWorker, EatOnlyWorker {
    work() {
        console.log("Taking orders...");
    }

    eat() {
        console.log("Eating during break...");
    }
}

// Uso de las clases
const programmer = new Programmer();
const waiter = new Waiter();

programmer.work(); // Output: Programming...
waiter.work(); // Output: Taking orders...
waiter.eat(); // Output: Eating during break...

```

En este ejemplo, la interfaz `Worker` se divide en interfaces más específicas (`WorkOnlyWorker`, `EatOnlyWorker` y `SleepOnlyWorker`) que contienen solo los métodos necesarios para cada tipo de trabajador. Las clases concretas (`Programmer` y `Waiter`) implementan las interfaces relevantes para su tipo de trabajo, evitando así la dependencia de métodos que no necesitan. Esto cumple con el Principio de Segregación de Interfaces.

### Liskov Substitution Principle

El Principio de Sustitución de Liskov (LSP) establece que los objetos de un programa deberían ser reemplazables por instancias de sus subtipos sin alterar la corrección del programa. En otras palabras, si tenemos una clase base y una subclase que la extiende, deberíamos poder usar un objeto de la subclase en lugar de un objeto de la clase base sin cambiar el comportamiento del programa.

Supongamos que tenemos una clase `Rectangle` (Rectángulo) y una subclase `Square` (Cuadrado). Según el LSP, deberíamos poder usar un objeto de tipo `Square` donde se espera un objeto de tipo `Rectangle` sin alterar el comportamiento esperado del programa:

```javascript
// Clase base para representar un rectángulo
class Rectangle {
    constructor(width, height) {
        this.width = width;
        this.height = height;
    }

    setWidth(width) {
        this.width = width;
    }

    setHeight(height) {
        this.height = height;
    }

    area() {
        return this.width * this.height;
    }
}

// Subclase para representar un cuadrado
class Square extends Rectangle {
    constructor(side) {
        super(side, side);
    }

    setWidth(width) {
        this.width = width;
        this.height = width;
    }

    setHeight(height) {
        this.width = height;
        this.height = height;
    }
}

// Función para calcular el área de un rectángulo
function calculateArea(rectangle) {
    rectangle.setWidth(5);
    rectangle.setHeight(4);
    console.log("Area:", rectangle.area());
}

// Uso de las clases
const rectangle = new Rectangle(5, 4);
const square = new Square(5);

calculateArea(rectangle); // Output: Area: 20
calculateArea(square); // Output: Area: 25

```

En este ejemplo, la clase `Square` es una subclase de `Rectangle` y sobrescribe los métodos `setWidth()` y `setHeight()` para asegurarse de que siempre tenga la misma anchura y altura. Aunque `Square` es una subclase de `Rectangle`, podemos usar un objeto de tipo `Square` donde se espera un objeto de tipo `Rectangle` (como en la función `calculateArea()`) sin cambiar el comportamiento esperado del programa. Esto cumple con el Principio de Sustitución de Liskov.

