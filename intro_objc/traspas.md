#Tecnologías para el desarrollo de aplicaciones en dispositivos móviles
##Sesiones 1 y 2: hola iOS
##Introducción al lenguaje Objective-C

---

##Puntos a tratar

- Un poco de historia
- Características básicas

---

##Un poco de historia

- Creado en 1980 por Brad Cox y Tom Love, inspirado en Smalltalk <!-- .element: class="fragment" -->
- Adoptado por NeXT como lenguaje de desarrollo en 1988 <!-- .element: class="fragment" -->
- En 1996 Apple compra NeXT y usa el S.O. NexTSTEP como base para OS X <!-- .element: class="fragment" -->
- En 2006 se presenta Objective-C 2.0, una importante modernización de la sintaxis<!-- .element: class="fragment" -->
- En 2014 Apple presenta Swift como alternativa a Objective-C <!-- .element: class="fragment" -->

---

## Características básicas

Es una **extensión de C** orientada a **objetos**, aunque **su sintaxis no se parece a la de C++**


```objectivec
@interface Baraja : NSObject
@property NSMutableArray *cartas;
@property(readonly) int numCartas;

- (id) init;
- (void) barajar;
- (Carta *) repartirCarta;
@end
```

```objectivec
@implementation Baraja
- (id) init {
    //implementación del método 
    //...
}
...
@end
```

---

Al ser una extensión de C, todo **el código C es también Objective-C válido**. Las instrucciones de control de flujo (`if`, `switch`, `while`, `do...while`,`for`...), y operadores lógicos y aritméticos son los mismos que en C


---


Las variables que contienen objetos son **referencias**, al estilo Java, pero debemos explicitarlo (con el `*`)
        
```objectivec
//Esto es un puntero, y se nota por el *
NSString *cadena = @"hola";
```

```java
//Esto también es un puntero, aunque no se note en nada
String cadena = "hola";
```

---

Como en Java, hay una dualidad entre **tipos primitivos** y **objetos**. Los tipos primitivos se pasan por valor, los objetos por referencia

```objectivec
//Esto no es un puntero
int i = 1;
//Los arrays si
NSMutableArray *a = ...  //Luego veremos cómo se inicializan
NSMutableArray *b = ...
//También veremos por qué todas las cadenas llevan una '@' delante
a[0] = @"hola";
//Al asignar dos objetos, estamos haciendo que apunten a la misma dirección
//ya que son referencias
b = a;
a[0] = @"adios";  //ahora b[0] también vale @"adios"
```

---

**No usa la sintaxis OO clásica** de `objeto.metodo(parametro)` sino otra mucho más atípica: `[objeto metodo: parametro]`


```objectivec
Carta *carta; //Una carta de la baraja
Mano *carta;  //Un conjunto de cartas

//Si esto fuera Java , sería algo así como mano.insertarCarta(carta)
[mano insertarCarta:carta];
```

 

---

**Los parámetros tienen nombre**,que en realidad es parte del nombre del método. Esto es más evidente cuando tenemos más de un parámetro

```objectivec
UIAlertView *alert = [[UIAlertView alloc]
                        initWithTitle:titulo
                        message:mensaje
                        delegate:self
                        cancelButtonTitle:@"OK"
                        otherButtonTitles: nil];
[alert show];
```

El nombre del método anterior es en realidad `initWithTitle:message:delegate:cancelButtonTitle:otherButtonTitles`




---

A diferencia de C++, en el que `new` reserva memoria y llama al constructor, en ObjectiveC los dos pasos están separados (`alloc` + `init`)

```objectivec
NSArray *a = [[NSArray alloc] init];
```

---

## Un inicializador típico

Normalmente no es necesario sobreescribir `alloc`, pero sí `init`

```objectivec
- (id)init {
    self = [super init];
    if (self) {
      //Aquí inicializaríamos las propiedades y/o variables de instancia
      ...
    }
    return self;
}
```

---


Una referencia para la que no se ha llamado a `alloc`+`init` es `nil` (el equivalente a `null` de Java) 

```objectivec
NSArray *a;
//Se me ha olvidado el alloc+init. La siguiente condición será cierta
if (a==nil)
    NSLog(@"¡¡¡a es nil!!!");
```


---

Curiosamente, llamar a un método desde `nil` no tiene efecto, ni bueno ni malo (no hace nada)

```objectivec
NSArray *a; 
//Se me ha olvidado el alloc+init
[a insertObject:@"hola" atIndex:1];
```

Nuestro código no hará nada, pero el *runtime* nos tiene en tan poca consideración que ni siquiera se molesta en abortar el programa :)



---

Objective-C es mucho más **dinámico** que C\++. Por ejemplo, se puede decidir el método a llamar en tiempo de ejecución.

```objectivec
SEL selector = NSSelectorFromString(@"action1");
if ([myFoo respondsToSelector:selector]) {
    [myFoo performSelector:selector];
}
```

---

Además de métodos, los objetos pueden tener **variables de instancia**

```objectivec
@implementation Persona : NSObject
int edad;
...
@end
```

---

En la documentación de Apple se recomienda el uso de **propiedades**, en lugar de variables de instancia

```objectivec
@interface Persona : NSObject
@property int edad;
...
@end
```

- Por cada propiedad hay una variable de instancia (por defecto con el mismo nombre precedido de `_`, en el ejemplo `_edad`) 
- Las propiedades pueden tener *atributos* (por ejemplo, solo lectura)

```objectivec
@property(readonly) int edad;
```

---

- Para cada propiedad se genera automáticamente un *getter* y un *setter* (solo *getter* si es `readonly`)

```objectivec
Persona *p = [[Persona alloc] init];
//Siguiendo las convenciones típicas, el setter es setXXX
[p setEdad:18];
//El getter se llama simplemente como la propiedad
NSLog(@"%d", [p edad]);
//Se puede usar también notación "."
p.edad = 18;
NSLog(@"%d", p.edad);
```

---

La **interfaz** (archivo `.h`) está físicamente separada de la **implementación** (`.m`)

```objectivec
//Archivo Baraja.h
@interface Baraja : NSObject
  //Propiedades
  ...
  //Métodos
  - (id) init;
  - (void) barajar;
  - (Carta *) repartirCarta;
@end
```

```objectivec
//Archivo Baraja.m
@implementation Baraja
- (id) init {
    //implementación del método 
    //...
}
...
@end
```


---


Para referenciar/incluir otras clases se usan `#import`, muy parecidos a los `#include` de C. 

```objectivec
//Archivo Baraja.m
#import "Carta.h"

@implementation Baraja
- (id) init {
    //implementación del método 
    //...
}
...
@end
```

---

**Propiedades privadas**: sección especial de `@interface` dentro del `.m`


```objectivec
@interface Prueba ()
@property NSMutableArray *cartas;
@end
@implementation Baraja
    //...
@end
```


---

La **gestión de memoria es automática**, se hace `alloc` pero no hace falta el equivalente al `delete` de C++

Técnicamente, la gestión de memoria no se hace al estilo Java, con "recolección de basura", sino con otra técnica denominada "cuenta de referencias", que ya veremos.

---


# ¿...alguna pregunta?