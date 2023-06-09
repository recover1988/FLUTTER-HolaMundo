# hello_world_app

A new Flutter project.

## Getting Started

This project is a starting point for a Flutter application.

A few resources to get you started if this is your first Flutter project:

- [Lab: Write your first Flutter app](https://docs.flutter.dev/get-started/codelab)
- [Cookbook: Useful Flutter samples](https://docs.flutter.dev/cookbook)

For help getting started with Flutter development, view the
[online documentation](https://docs.flutter.dev/), which offers tutorials,
samples, guidance on mobile development, and a full API reference.

## Widget

Los widgets de Flutter son construidos usando un moderno framework que toma inspiración de React. La idea central es que construyas tu UI de widgets. Los Widgets describen cómo debería ser su vista, dada su configuración y estado actuales. Cuando el estado de un widget cambia, el widget reconstruye su descripción, que el framework difiere de la descripción anterior para determinar los cambios mínimos necesarios en el árbol de renderizado subyacente para la transición de un estado al siguiente.

## runApp()

La aplicación mínima de Flutter simplemente llama a la función runApp() con un widget.

```
import 'package:flutter/material.dart';

void main() {
  runApp(
    Center(
      child: Text(
        'Hello, world!',
        textDirection: TextDirection.ltr,
      ),
    ),
  );
}
```

La función runApp() toma el Widget dado y lo convierte en la raíz del árbol de widgets. En este ejemplo, el árbol de widgets consiste de dos widgets, el widget Center y su hijo, el widget Text.El framework obliga al widget raíz a cubrir la pantalla, lo que significa que el texto “Hello, world!” termina centrado en la pantalla. La dirección del texto debe especificarse en esta instancia; cuando se usa el widget MaterialApp, este lo hace por ti, como se demuestra más adelante.

Al escribir una aplicación, comúnmente creará nuevos widgets que son subclases de, ya sea StatelessWidget o StatefulWidget, dependiendo de si su widget gestiona algún estado. El trabajo principal de un widget es implementar una función de construcción build, que describe el widget en términos de otros widgets de nivel inferior. El framework construye estos widgets en orden, hasta que el proceso toca fondo en widgets que representan el RenderObject subyacente, el cual calcula y describe la geometría del widget.

## Scaffold

```
import 'package:flutter/material.dart';

void main(List<String> args) {
  runApp(MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});


  @override
  Widget build(BuildContext context) {
    return const MaterialApp(
      debugShowCheckedModeBanner: false,
      home: Scaffold(
        body: Center(child: Text('Hola Mundo')),
      )
    );
  }
}
```

El widget Scaffold toma un número de widgets diferentes como argumentos con nombre, cada uno de los cuales se coloca en el layout del Scaffold en el lugar apropiado. Del mismo modo, el widget `body` nos permite pasarle widgets para el `Center` y los actions de su widget title. Este patrón se repite a lo largo de todo el framework y es algo que podría considerar al diseñar sus propios widgets.
El 'debugShowCheckedModeBanner' en false desactiva el banner que aparece de modo debbug.

# Estructura de directorios

Nosotros tenemos que crear nuestro propio file_system del proyecto, para aplicar algun tipo de arquitectura o estructura de directorios para manejar cada una de las carpetas que tengamos.
Usar principios de arqutectura limpia para poder mantener y ampliar la aplicacion.

- presentation: cualquier cosas que sea widgets personalizados.
- screens: un widget que cubre toda la pantalla y estas llevan sus propios `Scaffold`.

## Crear Screen

Podemos crear un widget de tipo screen y importarlo en la funcion main para que se renderize.

```
class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return const MaterialApp(
        debugShowCheckedModeBanner: false,
        home: CounterScreen() // CounterScreem
        );
  }
}
```

## Widgets

```
import 'package:flutter/material.dart';

class CounterScreen extends StatelessWidget {
  const CounterScreen({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: const [
            Text(
              '10',
              style: TextStyle(fontSize: 160, fontWeight: FontWeight.w100),
            ),
            Text(
              'cantidad de clicks',
              style: TextStyle(
                fontSize: 25,
              ),
            )
          ],
        ),
      ),
      floatingActionButton: FloatingActionButton(
        onPressed: () {},
        child: const Icon(Icons.plus_one),
      ),
    );
  }
}
```

Los widgtes tipo `Text` aceptan ademas del `String`, otra propiedad llamada `style` deonde podemos usar la clase `TextStyle` la cual tiene diferentes metodos para manejar las propiedades de los textos, como el tamaño, grosor, etc.

El widget `Scaffold` tiene diferentes widgets que significan una posicion en la pantalla como el `body`, que a su vez puede aceptar widgets.

- Column y Row son widgets que modifican la posicion de los widgets, como el flex en css de column y row. Y se puede alinear con el `mainAxisAlignment` que acepta una clase de tipo `MainAxisAlignment` que funciona como un enum con diferentes propiedades que se pueden aplicar como el center, etc.

- El `Scaffold`tambien acepta un `floatingActionButton` que a su vez tiene una clase ` FloatingActionButton``, que no es mas que un boton el cual tiene su metodo  `onPressed`que requiere una funcion y un`child` que requiere un Icon para mostrar.

## Material Design 3

Para usarlo tenemos que ir a la propiedad `theme` del `MaterialApp`

```
theme:
     ThemeData(
        useMaterial3: true,
        colorSchemeSeed: Colors.deepPurple
    ),
```

Al usar el `colorSchemeSeed` se nos genera una paleta de colores alrededor del color que elegimos.
Podemos usar colores llamando al constructor `Color` y entre sus metodos elegir alguna forma para pasar color com ARGB u otros. o podemos ir a `Colors` y elegir entre los colores predefinidos que tiene flutter.

## Cambiar el estado de la Aplicacion

Para cambiar de StatelessWidget a StatefulWidget apretamos el comando `Ctrl + .` sobre la extension, se recomienda no tener muchas widgets con estados.

```
class CounterScreen extends StatefulWidget {
  const CounterScreen({super.key});

  @override
  State<CounterScreen> createState() => _CounterScreenState();
}

class _CounterScreenState extends State<CounterScreen> {
..............
..............
..............
..............
}
```

# F2 sobre la palabra para renombrar todas

## AppBar y Acciones

La AppBar puede tener un leading o actions que esperan un widget que puede ser los columns o rows y asi poder usar otroa widgets dentro como botones.

```
 appBar: AppBar(
          title: const Text('Counter Functions'),
          actions: [
            IconButton(
              icon: const Icon(Icons.refresh_rounded),
              onPressed: () {
                setState(() {
                  clickCounter = 0;
                });
              },
            ),
          ],
          centerTitle: true,
        ),
```

El floatingActionButton espera un widget en este caso le ponemos un column para poder tener mas botones.

```
        floatingActionButton:
            Column(mainAxisAlignment: MainAxisAlignment.end, children: [
          FloatingActionButton(
            shape: const StadiumBorder(),
            onPressed: () {
              clickCounter += 1;
              setState(() {});
            },
            child: const Icon(Icons.plus_one),
          ),
          const SizedBox(
            height: 10,
          ),
          FloatingActionButton(
            shape: const StadiumBorder(),
            onPressed: () {
              clickCounter -= 1;
              setState(() {});
            },
            child: const Icon(Icons.exposure_minus_1_outlined),
          ),
```

## Widget Personalizado

Podemos hacer extraer el widget presionando `Ctrl + .` y eligiendo la opcion.
Tenemos que especificar el tipo que recibe la clase.

```
class CustomButton extends StatelessWidget {
  final IconData icon;
  final VoidCallback? onPressed;

  const CustomButton({
    super.key,
    required this.icon,
    this.onPressed,
  });

  @override
  Widget build(BuildContext context) {
    return FloatingActionButton(
      shape: const StadiumBorder(),
      onPressed: onPressed,
      child: Icon(icon),
    );
  }
}

```

Y asi podemos usar esta nueva widget pasandole las prop que necesitamos.

```
 floatingActionButton:
            Column(mainAxisAlignment: MainAxisAlignment.end, children: [
          CustomButton(
            icon: Icons.refresh_rounded,
            onPressed: () {
              clickCounter = 0;
              setState(() {});
            },
          ),
          const SizedBox(
            height: 10,
          ),
          CustomButton(
            icon: Icons.exposure_minus_1_outlined,
            onPressed: () {
              if (clickCounter == 0) return;
              clickCounter--;
              setState(() {});
            },
          ),
          const SizedBox(
            height: 10,
          ),
          CustomButton(
            icon: Icons.plus_one_outlined,
            onPressed: () {
              clickCounter++;
              setState(() {});
            },
          ),
        ]));
```

El `SizedBox` es un widget que seria una caja vacia con el tamaño que necesitemos para espaciar otros widgets.
