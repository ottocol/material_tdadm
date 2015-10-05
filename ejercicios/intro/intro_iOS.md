## Ejercicios de introducción a iOS

Vamos a **modificar la aplicación UADivino**. En lugar de mostrar solo el texto de la respuesta, también vamos a mostrar una imagen que la “ilustre”. Tendremos dos imágenes: una para cuando la respuesta es positiva y otra para cuando es negativa.

#### Parte 1: modificación del modelo
- Primero habrá que modificar el modelo para que en lugar de almacenar simplemente los textos de las respuestas para cada una se almacene el texto y además si la respuesta es positiva/negativa. **Crearemos la clase Respuesta**
- Despúes **añadirle a la clase Respuesta un constructor** `initWithTexto:andTipo:` para poderle pasar el texto y si es positiva/negativa. El constructor se usaría de este modo:

```objectivec
	Respuesta *r1 = [[Respuesta alloc] initWithTexto:@"NOOO!!" andTipo:NO];
```

- Para  instanciar el `NSMutableArray` de respuestas lo más simple es usar el mismo método que hasta ahora, el método de clase `arrayWithObjects`, pero ahora le pasaremos instancias de la clase `Respuesta` en lugar de cadenas de Foundation.

#### Parte 2: interfaz gráfico
- Crear una “Image view” en el interfaz de usuario, usando la “Object Library” (panel inferior derecho de Xcode)
- Crear un **outlet** que asocie esta “image view” con una propiedad en el `ViewController.h`. Podéis llamar a esta propiedad como queráis, por ejemplo “imagen”.
- Guarda las imágenes para la respuesta [positiva](img/si.png)/[negativa](img/no.png) en tu disco y añádelas al `images.xcassets` (puedes usar tus propias imágenes si lo prefieres). Dales un nombre corto para referirnos luego a ellas en el código.
- En el *controller*, dentro del método `botonPulsado`, además de mostrar la respuesta en la etiqueta, hay que actualizar la “image view”. Recordar que se puede cargar una imagen con `imageNamed:`. Lo que tenemos que cambiar de la “image view” es la propiedad `image`

```objectivec
//suponiendo que el outlet que se refiere a la "image view" se llama "imagen"
self.imagen.image = [UIImage imageNamed:@"nombre_de_imagen"]
```
