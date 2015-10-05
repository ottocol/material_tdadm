# Introducción a Objective-C

Este lenguaje es una extensión de C, por lo que podremos utilizar en cualquier lugar código C estándar, aunque normalmente utilizaremos los elementos equivalentes definidos en Objective-C para así mantener una coherencia con la API Cocoa, que está definida en dicho lenguaje.

Vamos a ver los elementos básicos que aporta Objective-C sobre los elementos del lenguaje con los que contamos en lenguaje C estándar. También es posible combinar Objective-C y C++, dando lugar a Objective-C++, aunque esto será menos común.

##Tipos de datos

###Tipos de datos básicos
A parte de los tipos de datos básicos que conocemos de C (`char`, `short`, `int`,
`long`, `float`, `double`, `unsigned int`, etc), en Cocoa se definen
algunos tipos numéricos equivalentes a ellos que encontraremos frecuentemente en dicha API:

- `NSInteger` (`int` o `long`)
- `NSUInteger` (`unsigned int` o `unsigned long`)
- `CGFloat` (`float` o `double`)

Podemos asignar sin problemas estos tipos de Cocoa a sus tipos C estándar equivalentes, y al contrario.

Contamos también con el tipo booleano definido como `BOOL`, y que puede tomar como valores
las constantes `YES` (1) y `NO` (0):

```objc
BOOL activo = YES;
```

###Enumeraciones

Las enumeraciones resultan útiles cuando una variable puede tomar su valor de un conjunto limitado de
posibles opciones. Dentro de la API de Cocoa es habitual encontrar enumeraciones, y se definen de la misma
forma que en C estándar:

```objc
typedef enum {
  UATipoAsignaturaOptativa,
  UATipoAsignaturaObligatoria,
  UATipoAsignaturaTroncal
} UATipoAsignatura;
```

A cada elemento de la enumeración se le asigna un valor entero, empezando desde `0`, y de forma
incremental siguiendo el orden en el que está definida la enumeración. Podemos también especificar de forma
manual el número asignado a cada elemento.

```objc
typedef enum {
  UATipoAsignaturaOptativa = 0,
  UATipoAsignaturaObligatoria = 1,
  UATipoAsignaturaTroncal = 2
} UATipoAsignatura;
```

###Estructuras

También encontramos y utilizamos estructuras de C estándar dentro de la API de Cocoa.

```objc
struct CGPoint {
  CGFloat x;
  CGFloat y;
};
typedef struct CGPoint CGPoint;
```
Muchas veces encontramos librerías de funciones para inicializarlas o realizar operaciones con ellas.

```objc
CGPoint punto = CGPointMake(x,y);
```
###Cadenas

En C normalmente definimos una cadena entre comillas, por ejemplo `"cadena"`. Con esto estamos
definiendo un <em>array</em> de caracteres terminado en `null`. Esto es lo que se conoce como
cadena C, pero en Objective-C normalmente no utilizaremos dicha representación. Las cadenas en Objective-C
se representarán mediante la clase `NSString`, y los literales de este tipo en el código se
definirán anteponiendo el símbolo `@` a la cadena:

```objc
NSString* cadena = @"cadena";
```

> Todas las variables para acceder a objetos de Objective-C tendrán que definirse como punteros, y por lo tanto en la declaración de la variable tendremos que poner el símbolo `*`.
Los únicos tipos de datos que no se definirán como punteros serán los tipos básicos y las estructuras.

Más adelante veremos las operaciones que podemos realizar con la clase `NSString`, entre ellas el convertir una cadena C estándar a un `NSString`.

####Objetos

Las cadenas en Objective-C son una clase concreta de objetos, a diferencia de las cadenas C estándar, pero las hemos visto como un caso especial por la forma en la que podemos definir en el código literales de ese tipo.

En la API de Objective-C encontramos una extensa librería de clases, y normalmente deberemos también crear clases propias en nuestras aplicaciones. Para referenciar una instancia de una clase siempre lo haremos mediante
un puntero.

```objc
MiClase* objeto;
```

Sin embargo, como veremos más adelante, la forma de trabajar con dicho puntero diferirá mucho de la forma en la
que se hacía en C, ya que Objective-C se encarga de ocultar toda esa complejidad subyacente y nos ofrece
una forma sencilla de manipular los objetos, más parecida a la forma con la que trabajamos con la API de Java.

No obstante, podemos utilizar punteros al estilo de C. Por ejemplo, podemos crearnos punteros de tipos básicos
o de estructuras, pero lo habitual será trabajar con objetos de Objective-C.

Otra forma de hacer referencia a un objeto de Objective-C es mediante el tipo `id`. Cuando tengamos una variable de este tipo podremos utilizarla para referenciar cualquier objeto, independientemente de la clase a la que pertenezca. El compilador no realizará ningún control sobre los métodos a los que llamemos, por lo que deberemos llevar cuidado al utilizarlo para no obtener ningún error en tiempo de ejecución.

```objc
MiClase* mc = // Inicializa MiClase;
MiOtraClase* moc = // Inicializa MiOtraClase;
...

id referencia = nil;

referencia = mc;
referencia = moc;
```

Como vemos, `id` puede referenciar instancias de dos clases distintas sin que exista ninguna relación entre ellas. Hay que destacar también que las referencias a objetos con `id` no llevan el símbolo
`*`.

También observamos que para indicar un puntero de objeto a nulo utilizamos `nil`. También contamos con `NULL`, y ambos se resuelven de la misma forma, pero conceptualmente se aplican a casos distintos. En caso de `nil`, nos referimos a un puntero nulo a un objeto de Objective-C, mientras que utilizamos
`NULL` para otros tipos de punteros.


##Directivas

También podemos incluir en el código una serie de directivas de preprocesamiento que resultarán de utilidad y que encontraremos habitualmente en aplicaciones iOS.


###La directiva `#import`

Con esta directiva podemos importar ficheros de cabecera de librerías que utilicemos en nuestro código. Se diferencia de `#include` en que con `#import` se evita que un mismo fichero sea incluido más de una vez cuando encontremos inclusiones recursivas.

Encontramos dos versiones de esta directiva: `#import&lt;...&gt;` e `#import "..."`. La primera de ellas buscará los ficheros en la ruta de inclusión del compilador, por lo que utilizaremos esta forma cuando estemos incluyendo las librerías de Cocoa. Con la segunda, se incluirá también nuestro propio
directorio de fuentes, por lo que se utilizará para importar el código de nuestra propia aplicación.

###La directiva `#define`

Este directiva toma un nombre (símbolo) y un valor, y sustituye en el código todas las ocurrencias de dicho nombre por el valor indicado antes de realizar la compilación. Como valor podremos poner cualquier expresión, ya que la sustitución se hace como preprocesamiento antes de compilar.

También podemos definir símbolos mediante parámetros del compilador. Por ejemplo, si nos fijamos en las <em>Build Settings</em> del proyecto, en el apartado <em>Preprocessing</em> vemos que para la configuración
<em>Debug</em> se le pasa un símbolo `DEBUG` con valor `1`.

Tenemos también las directivas `#ifdef` (y `#ifndef`) que nos permiten incluir un bloque de código en la compilación sólo si un determinado símbolo está definido (o no). Por ejemplo, podemos hacer que sólo se escriban logs si estamos en la configuración <em>Debug</em> de la siguiente forma:

```objc
#ifdef DEBUG
  NSLog(@"Texto del log");
#endif
```

Los nombres de las constantes (para ser más exactos macros) definidas de esta forma normalmente se utilizan letras mayúsculas separando las distintas palabras con el carácter subrayado `'_'` (`UPPER_CASE_UNDERSCORE`).

###La directiva `#pragma mark`

Se trata de una directiva muy utilizada en los ficheros de fuentes de Objective-C, ya que es reconocida y utilizada por Xcode para organizar nuestro código en secciones. Con dicha directiva marcamos el principio de cada sección de código y le damos un nombre. Con la barra de navegación de Xcode podemos saltar directamente a cualquier de las secciones:

```objc
#pragma mark Constructores

// Código de los constructores

#pragma mark Eventos del ciclo de vida

// Código de los manejadores de eventos

#pragma mark Fuente de datos

// Métodos para la obtención de datos

#pragma mark Gestión de la memoria

// Código de gestión de memoria
```

##Modificadores

Tenemos disponibles también modificadores que podemos utilizar en la declaración de las variables.

###Modificador `const`

Indica que el valor de una variable no va a poder se modificado. Se le debe asignar un valor en la declaración, y este valor no podrá cambiar posteriormente. Se diferencia de `#define` en que en este caso tenemos una variable en tiempo de compilación, y no una sustitución en preprocesamiento.
En general, no es recomendable utilizar `#define` para definir las constantes. En su lugar utilizaremos normalmente `enum` o `const`.

Hay que llevar cuidado con el lugar en el que se declara `const` cuando se trate de punteros. Siempre afecta al elemento que tenga inmediatamente a la izquierda, excepto en el caso en el que esté al principio, que afectará al elemento de la derecha:

```objc
// Puntero variable a objeto NSString constante (MAL)
const NSString * UATitulo = @"Menu";

// Equivalente al anterior (MAL)
NSString const * UATitulo = @"Menu";

// Puntero constante a objeto NSString (BIEN)
NSString * const UATitulo = @"Menu";
```

En los dos primeros casos, estamos definiendo un puntero a un objeto de tipo `const NSString`. Por lo tanto, nos dará un error si intentamos utilizarlo en cualquier lugar en el que necesitemos tener un puntero a `NSString`, ya que el compilador los considera tipos distintos. Además, no es necesario hacer que `NSString` sea constante. Para ello en la API de Cocoa veremos que existen
versiones mutables e inmutables de un gran número de objetos. Bastará con utilizar una cadena inmutable.

Las constantes se escribirán en <em>UpperCamelCase</em>, utilizando el prefijo de nuestra librería en caso de que sean globales.

###Modificador `static`

Nos permite indicar que una variable se instancie sólo una vez. Por ejemplo en el siguiente código:

```objc
static NSString *cadena = @"Hola";
NSLog(cadena);
cadena = @"Adios";
```

La primera vez que se ejecute, la variable `cadena` se instanciará y se inicializará con el valor `@"Hola"`, que será lo que se escriba como <em>log</em>, y tras ello modificaremos su valor a `@"Adios"`. En las sucesivas ejecuciones, como la variable ya estaba instanciada, no se volverá a instanciar ni a inicilizar, por lo que al ejecutar el código simplemente escribirá `Adios`.

Como veremos más adelante, este modificador será de gran utilidad para implementar el patrón <em>singleton</em>.

El comportamiento de `static` difiere según si la variable sobre la que se aplica tiene ámbito local o global. En el ámbito local, tal como hemos visto, nos permite tener una variable local que sólo se instancie una vez a lo largo de todas las llamadas que se hagan al método. En caso de aplicarlo a una
variable global, indica que dicha variable sólo será accesible desde dentro del fichero en el que esté definida. Esto resultará de utilidad por ejemplo para definir constantes privadas que sólo vayan a utilizarse dentro de un determinado fichero `.m`. Al comienzo de dicho ficheros podríamos declararlas de la siguiente forma:

```objc
static NSString * const titulo = @"Menu";
```

Si no incluimos `static`, si en otro fichero se definiese otro símbolo global con el mismo nombre, obtendríamos un error en la fase de <em>linkado</em>, ya que existirían dos símbolos con el mismo nombre dentro del mismo ámbito.

###Modificador `extern`

Si en el ejemplo anterior quisiéramos definir una constante global, no bastaría con declarar la constante sin el modificador `static` en el fichero `.m`:

```objc
NSString * const titulo = @"Menu";
```

Si sólo hacemos esto, aunque el símbolo sea accesible en el ámbito global, el compilador no va a ser capaz de encontrarlo ya que no hemos puesto ninguna declaración en los ficheros de cabecera incluidos. Por lo tanto, además de la definición anterior, tendremos que declarar dicha constante en algún fichero
de cabecera con el modificador `extern`, para que el compilador sepa que dicho símbolo existe en el ámbito global.

Para concluir, listamos los tres posibles ámbitos en los que podemos definir cada símbolo:

- **Global**: Se declaran fuera de cualquier método para que el símbolo se guarde de forma global.
Para que el compilador sepa que dicho símbolo existe, se deben declarar en los ficheros de cabecera con
`extern`. Sólo se instancian una vez.
- **Fichero**: Se declaran fuera de cualquier método con modificador `static`. Sólo
se podrá acceder a ella desde dentro del fichero en el que se ha definido, por lo que no deberemos declararlas
en ningún fichero de cabecera que vaya a ser importado por otras unidades. Sólo se instancian una vez.
- **Local**: Se declaran dentro de un bloque de código, como puede ser una función, un método,
o cualquier estructura que contengan los anteriores, y sólo será accesible dentro de dicho bloque. Por defecto
se instanciarán cada vez que entremos en dicho bloque, excepto si se declaran con el modificador `static`,
caso en el que se instanciarán y se inicializarán sólo la primera vez que se entre.

##Paso de mensajes

Como hemos comentado anteriormente, Objective-C es una extensión de C para hacerlo orientado a objetos, como es también el caso de C++. Una de las mayores diferencias entre ambos radica en la forma en la se ejecutan los
métodos de los objetos. En Objective-C los métodos siempre se ejecutan de forma dinámica, es decir, el método a ejecutar no se determina en tiempo de compilación, sino en tiempo de ejecución. Por eso hablamos de
<em>paso de mensajes</em>, en lugar de <em>invocar</em> un método. La forma en la que se pasan los mensajes también resulta bastante peculiar y probablemente es lo que primero nos llame la atención cuando veamos código Objective-C:

```objc
NSString* cadena = @"cadena-de-prueba";
NSUInteger tam = [cadena length];
```

Podemos observar que para pasar un mensaje a un objeto, ponemos entre corchetes `[...]` la referencia al objeto, y a continuación el nombre del método que queramos ejecutar.

Los métodos pueden tomar parámetros de entrada. En este caso cada parámetro tendrá un nombre, y tras poner el nombre del parámetro pondremos `:` seguido de el valor que queramos pasarle:

```objc
NSString* result = [cadena stringByReplacingOccurrencesOfString: @"-"
                 withString: @" "];
```

Podemos observar que estamos llamando al método `stringByReplacingOccurrencesOfString:withString:` de nuestro objeto de tipo `NSString`, para reemplazar los guiones por espacios.

Es importante remarcar que el nombre de un método comprende el de todos sus parámetros, por ejemplo, el método
anterior se identificaría mediante `stringByReplacingOccurrencesOfString:withString:`. Esto es lo que se
conoce como un <em>selector</em>, y nos permite identificar los mensajes que se le van a pasar a un objeto.

No podemos sobrecargar los métodos, es decir, no puede haber dos métodos que correspondan a un mismo <em>selector</em>
pero con distinto tipo de parámetros. Sin embargo, si que podemos crear varias versiones de un método con
distinto número o nombres de parámetros. Por ejemplo, también existe el método
`stringByReplacingOccurrencesOfString:withString:options:range:` que añade dos parámetros adicionales al
anterior.

Es posible llamar a métodos inexistentes sin que el compilador nos lo impida, como mucho obtendremos un
<em>warning</em>:

```objc
NSString* cadena = @"cadena-de-prueba";
[cadena metodoInexistente]; // Produce warning, pero compila
```

En el caso anterior, como `NSString` no tiene definido ningún método que se llame `metodoInexistente`, el compilador nos dará un <em>warning</em>, pero la aplicación compilará, y tendremos un error en tiempo de ejecución. Concretamente saltará una excepción que nos indicará que se ha enviado un mensaje a un selector inexistente.

En el caso en que nuestra variables fuese de tipo `id`, ni siquiera obtendríamos ningún <em>warning en la compilación</em>. En este tipo de variables cualquier mensaje se considera válido:

```objc
id cadena = @"cadena-de-prueba";
[cadena metodoInexistente]; // Solo da error de ejecucion
```

Por este motivo es por el que hablamos de paso de mensajes en lugar de llamadas a métodos. Realmente lo que hace nuestro código es enviar un mensaje al objeto, sin saber si el método existe o no.

##Creación e inicialización

Para instanciar una clase deberemos pasarle el mensaje `alloc` a la clase de la cual queramos crear la instancia. Por ejemplo:

```objc
id instancia = [NSString alloc];
```

Con esto crearemos una instancia de la clase `NSString`, reservando la memoria necesaria para alojarla, pero antes de poder utilizarla
deberemos inicializarla con alguno de sus métodos inicializadores. Los inicializadores comienzan todos por `init`, y normalmente encontraremos definidos varios con distintos parámetros.

```objc
NSString *cadVacia = [[NSString alloc] init];
NNString *cadFormato = [[NSString alloc] initWithFormat: @"Numero %d", 5];
```

Como podemos ver, podemos anidar el paso de mensajes siempre que el resultado obtenido de pasar un mensaje sea un objeto al que podemos pasarle otro. Normalmente siempre encontraremos anidadas las llamadas a `alloc` e `init`, ya que son los dos pasos que siempre se deben dar para construir el objeto.
Destacamos que `alloc` es un método de clase, que normalmente no será necesario redefinir, y que todas las clases heredarán de `NSObject` (en Objective-C las clases también son objetos y los métodos de clase se heredan), mientras que `init` es un método de instancia (se pasa el mensaje a la
instancia creada con `alloc`), que nosotros normalmente definiremos en nuestras propias clases (de `NSObject` sólo se hereda un método `init` sin parámetros).

Sin embargo, en las clases normalmente encontramos una serie de métodos alternativos para instanciarlas y construirlas. Son los llamados métodos factoría, y en este caso todos ellos son métodos de clase. Suele haber
un método factoría equivalente a cada uno de los métodos `init` definidos, y nos van a permitir instanciar e inicializar la clase directamente pasando un único mensaje. En lugar de `init`, comienzan con el nombre del objeto que están construyendo:

```objc
NSString *cadVacia = [NSString string];
NNString *cadFormato = [NSString stringWithFormat: @"Numero %d", 5];
```

Suelen crearse para facilitar la tarea al desarrollador. Estas dos formas de instanciar clases tienen una diferencia importante en cuando a la gestión de la memoria, que veremos más adelante.

##Algunas clases básicas de Cocoa Touch

Vamos a empezar viendo algunos ejemplos de clases básicas de la API Cocoa Touch que necesitaremos para implementar nuestras aplicaciones. En estas primeras sesiones comenzaremos con el <em>framework Foundation</em>, donde tenemos la librería de clases de propósito general, y que normalmente encontraremos con el prefijo `NS`.

###Objetos

`NSObject` es la clase de la que normalmente heredarán todas las demás clases, por lo que sus métodos estarán disponibles en casi en todas las clases que utilicemos. Vamos a ver los principales métodos de esta clase.

####Inicialización e instanciación

Ya conocemos algunos métodos de este grupo (`alloc` e `init`), pero podemos encontrar alguno más:

- `+ initialize`: Es un método de clase, que se llama cuando la clase se carga por primera vez,
antes de que cualquier instancia haya sido cargada. Nos puede servir para inicializar variables estáticas.
- `+ new`: Este método lo único que hace es llamar a `alloc` y posteriormente a `init`,
para realizar las dos operaciones con un único mensaje. No se recomienda su uso.
- `+ allocWithZone: (NSZone*)`: Reserva memoria para el objeto en la zona especificada. Si pasamos
`nil` como parámetro lo aloja en la zona por defecto. El método `alloc` visto anteriormente
realmente llama a `allocWithZone: nil`, por lo que si queremos cambiar la forma en la que se
instancia el objeto, con sobrescribir `allocWithZone` sería suficiente (esto se puede utilizar por
ejemplo al implementar el patrón <em>singleton</em>, para evitar que el objeto se instancie más de una vez).
Utilizar zonas adicionales puede permitirnos optimizar los accesos memoria (para poner juntos en memoria objetos
que vayan a utilizarse de forma conjunta), aunque normalmente lo más eficiente será utilizar la zona creada por defecto.
- `- copy` / `- mutableCopy`: Son métodos de instancia que se encargan de crear una nueva
instancia del objeto copiando el estado de la instancia actual. No todos los objetos son copiables, a continuación veremos más detalles sobre la copia
de objetos.

####Objetos mutables e inmutables

En la API de Cocoa encontramos varios objetos que se encuentran disponibles en dos versiones: **mutable** e **inmutable**. Uno de estos objetos son por ejemplo las cadenas (`NSString` y `NSMutableString`).

Los objetos **inmutables** son objetos de los cuales no podemos cambiar su estado interno, y que una vez instanciados e inicializados, sus variables de instancia no cambiarán de valor. Por ejemplo, una vez instanciada una cadena de tipo `NSString`, no podremos modificar sus caracteres.

Por otro lado, los **mutables** son aquellos cuyo estado si puede ser modificado. Este es el caso de `NSMutableString`, que además de todos los métodos que ya tenía la clase `NSString`, incorpora también métodos como `appendString:` o `replaceCharactersInRange:withString:` que nos permiten modificar el contenido de la cadena.

####Copia de objetos

La clase `NSObject` incorpora el método `copy` que se encarga de crear una copia de nuestro objeto. Sin embargo, no todos los objetos son copiables. Sólo se podrán copiar aquellos objetos que adopten el protocolo `NSCopying` (lo podremos comprobar en la documentación de la clase). Gran parte de los objetos de la API de Cocoa Touch implementan `NSCopying`, y por lo tanto son copiables.

Los objetos que existan en las modalidades mutable e inmutable pueden adoptar también el protocolo `NSMutableCopy`, que nos indica que podemos utilizar también el método `mutableCopy`, para poder crear una copia mutable del objeto.En estos objetos `copy` realizará la copia inmutable, mientras
que `mutableCopy` creará una copia mutable.

Por ejemplo, en el caso de las cadenas tenemos las dos opciones. Tanto si nuestra cadena original es mutable como inmutable, podremos obtener una copia de ella en cualquiera de estas modalidades:

```objc
// La cadena original es inmmutable
NSString *cadena = @"Mi cadena";

NSString *copiaInmutable = [cadena copy];
NSMutableString *copiaMutable = [cadena mutableCopy];

// La cadena original es mutable
NSMutableString *cadenaMutable = [NSMutableString stringWithCapacity: 32];

NSString *copiaInmutable = [cadenaMutable copy];
NSMutableString *copiaMutable = [cadenaMutable mutableCopy];
```

####Información de la instancia

Al igual que ocurría en Java, `NSObject` implementa una serie de métodos que nos darán información sobre los objetos, y que nosotros podemos sobrescribir en nuestra clases para personalizar dicha información. Estos métodos son:

- `isEqual`: Comprueba si dos instancias de nuestra clase son iguales internamente, y nos devuelve un <em>booleano</em> (`YES` o `NO`).
- `description`: Nos da una descripción de nuestro objeto en forma de cadena de texto (`NSString`).
- `hash`: Genera un código <em>hash</em> a partir de nuestro objeto para indexarlo en tablas. Si hemos redefinido `isEqual`, deberíamos también redefinir `hash` para que dos objetos iguales generen siempre el mismo <em>hash</em>.

###Cadenas

La clase `NSString` es la clase con la que representamos las cadenas en Objective-C, y hemos visto cómo utilizarla en varios de los ejemplos anteriores. Vamos ahora a ver algunos elementos básicos de esta clase.

####Literales de tipo cadena

La forma más sencilla de inicializar una cadena es utilizar un literal de tipo `@"cadena"`, que inicializa un objeto de tipo `NSString*`, y que no debemos confundir con las cadenas de C estándar que se definen como `"cadena"` (sin la `@`) y que corresponden al tipo `char*`.

####Cadenas C estándar y Objective-C

Como hemos comentado, las cadenas `@"cadena"` y `"cadena"` son tipos totalmente distintos, por lo que no podemos utilizarlas en las mismas situaciones. Las clases de Cocoa Touch siempre trabajarán con
`NSString`, por lo que si tenemos cadenas C estándar tendremos que convertirlas previamente. Para ello, en la clase `NSString` contamos con métodos inicializadores que crear la cadena Objective-C a partir de
una cadena C:

```objc
- (id)initWithCString:(const char *)nullTerminatedCString
encoding:(NSStringEncoding)encoding
- (id)initWithUTF8String:(const char *)bytes
```

El primero de ellos inicializa la cadena Objective-C a partir de una cadena C y de la codificación que se esté utilizando en dicha cadena. Dado que la codificación más común es UTF-8, tenemos un segundo método que la inicializa considerando directamente esta codificación. También encontramos métodos de factoría equivalentes:

```objc
- (id)stringWithCString:(const char *)nullTerminatedCString
encoding:(NSStringEncoding)encoding
- (id)stringWithUTF8String:(const char *)bytes
```

De la misma forma, puede ocurrir que tengamos una cadena en Objective-C y que necesitemos utilizarla en alguna función C estándar. En la clase `NSString` tenemos métodos para obtener la cadena como cadena C estándar:

```objc
NSString *cadena = @"Cadena";
const char *cadenaC = [cadena UTF8String];
const char *cadenaC = [cadena cStringWithEncoding: NSASCIIStringEncoding];
```

####Inicialización con formato

Podemos dar formato a las cadenas de forma muy parecida al `printf` de C estándar. Para ello contamos
con el inicializador `initWithFormat`:

```objc
NSString *cadena = [[NSString alloc]
initWithFormat: @"Duracion: %d horas", horas];
```

A los códigos de formato que ya conocemos de `printf` en C, hay que añadir `%@` que nos permite imprimir objetos Objective-C. Para imprimir un objeto utilizará su método `description` (o `descriptionWithLocale` si está implementado). Deberemos utilizar dicho código para imprimir cualquier objeto Objective-C, incluido `NSString`:

```objc
NSString *nombre = @"Pepe";
NSUInteger edad = 20;

NSString *cadena =
[NSString stringWithFormat: @"Nombre: %@ (edad %d)", nombre, edad];
```

> Nunca se debe usar el código `%s` con una cadena Objective-C
(`NSString`). Dicho código espera recibir un <em>array</em> de caracteres acabado en `NULL`, por lo que si pasamos un puntero a objeto obtendremos resultados inesperados. Para imprimir una cadena Objective-C siempre utilizaremos `%@`.

Los mismos atributos de formato se podrán utilizar en la función `NSLog` que nos permite escribir <em>logs</em> en la consola para depurar aplicaciones

```objc
NSLog(@"i = %d, obj = %@", i, obj);
```

> Los <em>logs</em> pueden resultarnos muy útiles para depurar la aplicación, pero
debemos llevar cuidado de eliminarlos en la <em>release</em>, ya que reducen drásticamente el rendimiento
de la aplicación, y tampoco resulta adecuado que el usuario final pueda visualizarlos. Podemos ayudarnos
de las macros (`#define`, `#ifdef`) para poder activarlos o desactivarlos de forma
sencilla según la configuración utilizada.

Los <em>logs</em> aparecerán en el panel de depuración ubicado en la parte inferior de la pantalla
principal del entorno. Podemos abrirlo y cerrarlo mediante en botón correspondiente en la esquina superior
izquierda de la pantalla. Normalmente cuando ejecutemos la aplicación y ésta escriba <em>logs</em>, dicho
panel se mostrará automáticamente.

####Localización de cadenas

Las cadenas se puede externalizar en un fichero que por defecto
se llamará `Localizable.strings`, del que podremos tener varias versiones, una para cada localización
soportada. Vamos a ver ahora cómo leer estas cadenas localizadas. Para leerlas contamos con la función
`NSLocalizedString(clave, comentario)` que nos devuelve la cadena como `NSString`:

```objc
NSString *cadenaLocalizada = NSLocalizedString(@"Titulo", @"Mobile UA");
```

Este método nos devolverá el valor asociado a la clave proporcionada según lo especificado en el fichero
`Localizable.strings` para el <em>locale</em> actual. Si no se encuentra la clave, nos devolverá
lo que hayamos especificado como comentario.

Existen versiones alternativas del método que no utilizan el fichero por defecto, sino que toman
como parámetro el fichero del que sacar las cadenas.

Las cadenas de `Localizable.strings` pueden también contener códigos de formato. En estos casos
puede ser interesante numerar los parámetros, ya que puede que en diferentes idiomas el orden sea distinto:

```objc
// es.lproj
"CadenaFecha" = "Fecha: %1$2d / %2$2d / %3$4d";

// en.lproj
"CadenaFecha" = "Date: %2$2d / %1$2d / %3$4d";
```

Podemos utilizar `NSLocalizedString` para obtener la cadena con la plantilla del formato:

```objc
NSString *cadena = [NSString stringWithFormat:
NSLocalizedString(@"CadenaFecha", "Date: %2$2d / %1$2d / %3$4d"),
dia, mes, anyo ];
```

####Conversión de números

La conversión entre cadenas y los diferentes tipos numéricos en Objective-C también se realiza con la
clase `NSString`. La representación de un número en forma de cadena se puede realizar con el método
`stringWithFormat` visto anteriormente, permitiendo dar al número el formato que nos interese.

La conversión inversa se puede realizar mediante una serie de métodos de la clase `NSString`:

```objc
NSInteger entero = [cadenaInt integerValue];
BOOL booleano = [cadenaBool boolValue];
float flotante = [cadenaFloat floatValue];
```

####Comparación de cadenas

Las cadenas son punteros a objetos, por lo que si queremos comparar si dos cadenas son iguales nunca deberemos utilizar el operador `==`, ya que esto sólo comprobará si los dos punteros apuntan a la misma dirección de memoria. Para comparar si dos cadenas contienen los mismos caracteres podemos utilizar el método `isEqual`, al igual que para comparar cualquier otro tipo de objeto Objective-C, pero si sabemos que los dos objetos son cadenas es más sencillo utilizar el método `isEqualToString`.

```objc
if([cadena isEqualToString: otraCadena]) { ... }
```

También podemos comparar dos cadenas según el orden alfabético, con `compare`. Nos devolverá un valor de la enumeración `NSComparisonResult` (`NSOrderedAscending`, `NSOrderedSame`, o `NSOrderedDescending`).

```objc
NSComparisonResult resultado = [cadena compare: otraCadena];

switch(resultado) {
	case NSOrderedAscending:
		...
		break;
	case NSOrderedSame:
		...
		break;
	case NSOrderedDescending:
		...
		break;
}
```

Tenemos también el método `caseInsensitiveCompare` para que realice la comparación ignorando mayúsculas y minúsculas.

Otros métodos nos permiten comprobar si una cadena tiene un determinado prefijo o sufijo (`hasPrefix`, `hasSuffix`), u obtener la longitud de una cadena (`length`).

###Fechas

Otro tipo de datos que normalmente necesitaremos tratar en nuestras aplicaciones son las fechas. En Objective-C las fechas se encapsulan en `NSDate`. Muchos de los métodos de dicha clase toman como parámetro datos del tipo `NSTimeInterval`, que equivale a `double`, y corresponde al tiempo en segundos (con una precisión de submilisegundos).

La forma más rápida de crear un objeto fecha es utilizar su inicializador (o factoría) sin parámetros, con lo que se creará un objeto representando la fecha actual.

```objc
NSDate *fechaActual = [NSDate date];
```

También podemos crear la fecha especificando el número de segundos desde la fecha de referencia (1 de enero de 1970 a las 0:00).

```objc
NSDate *fecha = [NSDate dateWithTimeIntervalSince1970: segundos];
```

De la misma forma que en el caso de las cadenas, podemos comparar fechas con el método `compare`, que también nos devolverá un valor de la enumeración `NSComparisonResult`.

####Componentes de la fecha

El objeto `NSDate` representa una fecha simplemente mediante el número de segundos transcurridos desde
la fecha de referencia. Sin embargo, muchas veces será necesario obtener los distintos componentes de la fecha
de forma independiente (día, mes, año, hora, minutos, segundos, etc). Estos componentes se encapsulan como
propiedades de la clase `NSDateComponents`.

Para poder obtener los componentes de una fecha, o crear una fecha a partir de sus componentes, necesitamos un objeto calendario `NSCalendar`:

```objc
NSDate *fecha = [NSDate date];
NSCalendar *calendario = [NSCalendar currentCalendar];
NSDateComponents *componentes = [currentCalendar
    components:(NSDayCalendarUnit | NSMonthCalendarUnit |
                NSYearCalendarUnit)
      fromDate:fecha];

NSInteger dia = [componentes day];
NSInteger mes = [componentes month];
NSInteger anyo = [componentes year];
```

Con el método de clase `currentCalendar` obtenemos una instancia del calendario correspondiente al usuario actual. Con el método `components:fromDate:` de dicho calendario podemos extraer los componentes indicados de la fecha (objeto `NSDate`) que proporcionemos. Los componentes se especifican mediante una máscara que se puede crear a partir de los elementos de la enumeración `NSCalendarUnit`, y son devueltos mediante un objeto de tipo `NSDateComponents` que incluirá los componentes solicitados como campos.

También se puede hacer al contrario, crear un objeto `NSDateComponents` con los campos
que queramos para la fecha, y a partir de él obtener un objeto `NSDate`:

```objc
NSDateComponents *componentes = [[NSDateComponents alloc] init];
[componentes setDay: dia];
[componentes setMonth: mes];
[componentes setYear: anyo];

NSDate *fecha = [calendario dateFromComponents: componentes];
[componentes release];
```

Con el calendario también podremos hacer operaciones con fechas a partir de sus componentes. Por ejemplo,
podemos sumar valores a cada componentes con `dateByAddingComponents:toDate:`, u obtener la diferencia
entre dos fechas componente a componente con `components:fromDate:toDate:options:`.

####Formato de fechas

Podemos dar formato a las fecha con `NSDateFormatter`. La forma más sencilla es utilizar alguno de los
estilos predefinidos en la enumeración `NSDateFormatterStyle` (`NSDateFormatterNoStyle`,
`NSDateFormatterShortStyle`, `NSDateFormatterMediumStyle`, `NSDateFormatterLongStyle`,
`NSDateFormatterFullStyle`):

```objc
NSDateFormatter formato = [[NSDateFormatter alloc] init];

[formato setTimeStyle: NSDateFormatterNoStyle];
[formato setDateStyle: NSDateFormatterFullStyle];

NSString *cadena = [formato stringFromDate: fecha];
[formato release];
```

Podemos también especificar un formato propio mediante un patrón con `setDateFormat`:

```objc
[formato setDateFormat: @"dd/MM/yyyy HH:mm"];```

También podremos utilizar nuestro objeto de formato para <em>parsear</em> fechas con `dateFromString:`

```objc
NSDate *fecha = [formato dateFromString: @"20/06/2012 14:00"];
```


###Errores y excepciones

En Objective-C podemos tratar los errores mediante excepciones de forma similar a Java. Para capturar
una excepción podemos utilizar la siguiente estructura:

```objc
@try
// Codigo
@catch(NSException *ex) {
    // Codigo tratamiento excepcion
}
@catch(id obj) {
    // Codigo tratamiento excepcion
}
@finally {
    // Codigo de finalización
}
```

Una primera diferencia que encontramos con Java es que se puede lanzar cualquier objeto (por ejemplo, en el segundo
`catch` vamos que captura `id`), aunque se recomienda utilizar siempre `NSException`
(o una subclase de ésta). Otra diferencia es que en Objective-C no suele ser
habitual heredar de `NSException` para crear nuestros propios tipos de excepciones. Cuando se produzca una
excepción en el código del bloque `try` saltará al primer `catch` cuyo tipo sea compatible con el del objeto lanzado. El bloque `finally` siempre se ejecutará, tanto si ha lanzado la excepción como si no, por lo que será el lugar idóneo para introducir el código de finalización (por ejemplo, liberar referencias a objetos).

Podremos lanzar cualquier objeto con `@throw`:

```objc
@throw [[[NSException alloc] initWithName: @"Error"
   reason: @"Descripcion del error"
   userInfo: nil] autorelease];
 ```

Aunque tenemos disponible este mecanismo para tratar los errores, en Objective-C suele ser más común pasar un parámetro de tipo `NSError` a los métodos que puedan producir algún error. En caso de que se produzca, en dicho objeto tendremos la descripción del error producido:

```objc
NSError *error;
NSString *contenido = [NSString
               stringWithContentsOfFile: @"texto.txt"
                               encoding: NSASCIIStringEncoding
                                  error: &amp;error];
```

Este tipo de métodos reciben como parámetro la dirección del puntero, es decir, `(NSError **)`, por lo que no es necesario que inicialicemos el objeto `NSError` nosotros. Si no nos interesa controlar los errores producidos, podemos pasar el valor `nil` en el parámetro `error`. Los errores
llevan principalmente un código (`code`) y un dominio (`domain`). Los código se definen como constantes en Cocoa Touch (podemos consultar la documentación de `NSError` para consultarlos). También incorpora mensajes que podríamos utilizar para mostrar el motivo del error y sus posibles soluciones:

```objc
NSString *motivo = [error localizedFailureReason];
```

En Objective-C no hay ninguna forma de crear excepciones equivalentes a las excepciones de tipo <em>checked</em> en Java (es decir, que los métodos estén obligados a capturarlas o a declarar que pueden lanzarlas). Por este motivo,
aquellos métodos en los que en Java utilizaríamos excepciones <em>checked</em> en Objective-C no será recomendable utilizar excepciones, sino incorporar un parámetro `NSError` para así dejar claro en la interfaz que la operación podría fallar. Sin embargo, las excepciones si que serán útiles si queremos tratar posibles fallos inesperados del <em>runtime</em> (las que serían equivalentes a las excepciones <em>unchecked</em> en Java).

> Como hemos comentado, mientras escribimos código podemos ver en el panel de
utilidades ayuda rápida sobre el elemento sobre el que se encuentre el cursor en el editor. Tenemos también otros
atajos para acceder a la ayuda. Si hacemos <em>option(alt)-click</em> sobre un elemento del código abriremos un
cuadro con su documentación, a partir del cual podremos acceder a su documentación completa en <em>Organizer</em>.
Por otro lado, si hacemos <em>cmd-click</em> sobre un elemento del código, nos llevará al lugar en el que ese
elemento fue definido. Esto puede ser bastante útil para acceder de forma rápida a la declaración de clases y de
tipos.
