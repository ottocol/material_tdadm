# Ejercicios de introducción a iOS

## Foundation

Vamos a gestionar una hipotética agenda de contactos para el móvil. Cada contacto tendrá inicialmente un nombre y uno o varios teléfonos. El nombre será una cadena y los teléfonos un array mutable de cadenas (cada teléfono se almacena como una cadena). Para simplificar, la aplicación no tendrá interfaz gráfico, vamos a crear solo el modelo de datos. 

Crea un nuevo proyecto en Xcode de tipo "single view application". Como nombre ponle "AgendaSimple". Desmarca la casilla de "use core Data", no es necesaria. 

### Representar un contacto (1.5 puntos)

Cada contacto será una instancia de la clase `Contacto`. Crea dicha clase, que debe tener:

- propiedades: el nombre del contacto y los teléfonos.
- inicializador:  `initConNombre:yTelefono:`, con dos parámetros, el nombre del nuevo contacto y un teléfono (el inicializador no admite varios teléfonos) 
- un método: `nuevoTelefono:` al que se le pasa un teléfono a añadir al contacto. Para simplificar, no es necesario comprobar si el teléfono ya estaba.

En el `didFinishLaunchingWithOptions` del `AppDelegate.m` del proyecto incluye unas cuantas líneas que prueben el funcionamiento de la clase, algo como:

```objectivec
Contacto *c;

[c initConNombre:@"Pepe" yTelefono:@"666112233"];
[c nuevoTelefono:@"626113322"];
//Imprimimos el nombre y los teléfonos
NSlog(@"%@", c.nombre)
for (NSString *tel in c.telefonos) {
    NSLog(@"%@", tel);
}
```

### Representar la agenda (1.5 puntos)

Crea una clase `Agenda` que represente la agenda con todos los contactos. Supondremos para simplificar que el nombre de cada contacto es único. Eso nos permite almacenar la agenda como un diccionario, cuya clave es el nombre del contacto y cuyo valor es el objeto `Contacto` en sí.

La clase `Agenda` debe tener

- propiedades: no es necesario que tenga ninguna. Podéis definir el diccionario con todos los contactos como una variable de instancia
- Un inicializador `init` que cree la agenda vacía
- métodos:
    + `nuevoContacto:` que añade un contacto a la agenda. Para simplificar, no es necesario comprobar si ya está.
    + `mostrarAgenda` que imprime en pantalla los nombres de los contactos y para cada uno sus teléfonos

Para comprobar que el código funciona, incluye unas cuantas líneas de código en el `didFinishLaunchingWithOptions` del `AppDelegate.m` que creen una agenda vacía, añadan un par de contactos y muestren el contenido de la agenda.


## UAdivino (total: 2 puntos)

Vamos a **modificar la aplicación UAdivino**. En lugar de mostrar solo el texto de la respuesta, también vamos a mostrar una imagen que la “ilustre”. Tendremos dos imágenes: una para cuando la respuesta es positiva y otra para cuando es negativa.

### Parte 1: modificación del modelo (1 punto)
- Primero habrá que modificar el modelo para que en lugar de almacenar simplemente los textos de las respuestas para cada una se almacene el texto y además si la respuesta es positiva/negativa. **Crearemos la clase Respuesta**
- Despúes **añadirle a la clase Respuesta un constructor** `initWithTexto:andTipo:` para poderle pasar el texto y si es positiva/negativa. El constructor se usaría de este modo:

```objectivec
Respuesta *r1 = [[Respuesta alloc] initWithTexto:@"NOOO!!" andTipo:NO];
```

- Para  instanciar el `NSMutableArray` de respuestas lo más simple es usar el mismo método que hasta ahora, el método de clase `arrayWithObjects`, pero ahora le pasaremos instancias de la clase `Respuesta` en lugar de cadenas de Foundation.

### Parte 2: interfaz gráfico (1 punto)

- Crear una “Image view” en el interfaz de usuario, usando la “Object Library” (panel inferior derecho de Xcode)
- Crear un **outlet** que asocie esta “image view” con una propiedad en el `ViewController.h`. Podéis llamar a esta propiedad como queráis, por ejemplo “imagen”.
- Guarda las imágenes para la respuesta [positiva](img/si.png)/[negativa](img/no.png) en tu disco y añádelas al `images.xcassets` (puedes usar tus propias imágenes si lo prefieres). Dales un nombre corto para referirnos luego a ellas en el código.
- En el *controller*, dentro del método `botonPulsado`, además de mostrar la respuesta en la etiqueta, hay que actualizar la “image view”. Recordar que se puede cargar una imagen con `imageNamed:`. Lo que tenemos que cambiar de la “image view” es la propiedad `image`

```objectivec
//suponiendo que el outlet que se refiere a la "image view" se llama "imagen"
self.imagen.image = [UIImage imageNamed:@"nombre_de_imagen"]
```
