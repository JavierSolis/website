---
title: Actualizar el UI basándose en la orientación
prev:
  title: Exportar fuentes desde un paquete
  path: /docs/cookbook/design/package-fonts
next:
  title: Usar Themes para compartir colores y esitlos de fuente
  path: /docs/cookbook/design/themes
---

En ciertos casos, 
quieres actualizar el diseño de una aplicación cuando el usuario 
rota su pantalla de modo portrait a modo landscape. Por ejemplo, 
la app podría mostrar un elemento tras otro en modo portrait, 
pero colocar esos mismos elementos uno al lado del otro en modo landscape.

En Flutter, puedes construir diferentes diseños dependiendo de una
[`Orientation`]({{site.api}}/flutter/widgets/Orientation-class.html) dada.
En este ejemplo, crearemos una lista que muestre 2 columnas 
en modo portrait y 3 columnas en modo landscape usando los 
siguientes pasos:

  1. Construye un `GridView` con 2 columnas
  2. Usa un `OrientationBuilder` para cambiar el número de columnas


## 1. Construye un `GridView` con 2 columnas

Primero, crea una lista de elementos para trabajar. 
En lugar de utilizar una lista normal, 
queremos una lista que muestre los elementos en un grid. 
Por ahora, crearemos un grid con 2 columnas.

<!-- skip -->
```dart
GridView.count(
  // Una lista con 2 columnss
  crossAxisCount: 2,
  // ...
);
```

Para obtener más información sobre cómo trabajar con `GridViews`, mira la receta 
[Creando una lista en un grid](/docs/cookbook/lists/grid-lists/).

## 2. Usa un `OrientationBuilder` para cambiar el número de columnas

Para determinar la `Orientation` actual de la app, 
usa el widget 
[`OrientationBuilder`]({{site.api}}/flutter/widgets/OrientationBuilder-class.html). 
El `OrientationBuilder` calcula la `Orientation` actual comparando 
la anchura y la altura disponibles para el widget padre, 
y la reconstruye cuando el tamaño del padre cambia.

Usando `Orientation`, podemos construir una lista que muestre 2 columnas en 
modo portrait o 3 columnas en modo landscape.

<!-- skip -->
```dart
OrientationBuilder(
  builder: (context, orientation) {
    return GridView.count(
      // Crea una grid con 2 columnas en modo portrait 
      // o 3 columnas en modo landscape.
      crossAxisCount: orientation == Orientation.portrait ? 2 : 3,
    );
  },
);
```
{{site.alert.note}}
Nota: Si estás interesado en la orientación de la pantalla, más que 
en la cantidad de espacio disponible para el padre, por favor usa 
`MediaQuery.of(context).orientation` en vez de un Widget `OrientationBuilder`.
{{site.alert.end}}

## Ejemplo completo

```dart
import 'package:flutter/material.dart';

void main() {
  runApp(MyApp());
}

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    final appTitle = 'Orientation Demo';

    return MaterialApp(
      title: appTitle,
      home: OrientationList(
        title: appTitle,
      ),
    );
  }
}

class OrientationList extends StatelessWidget {
  final String title;

  OrientationList({Key key, this.title}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text(title)),
      body: OrientationBuilder(
        builder: (context, orientation) {
          return GridView.count(
            // Crea una grid con 2 columnas en modo portrait o 3 columnas en
            // modo landscape.
            crossAxisCount: orientation == Orientation.portrait ? 2 : 3,
            // Genera 100 widgets que muestran su índice en la Lista
            children: List.generate(100, (index) {
              return Center(
                child: Text(
                  'Item $index',
                  style: Theme.of(context).textTheme.headline,
                ),
              );
            }),
          );
        },
      ),
    );
  }
}
```

![Orientation Demo](/images/cookbook/orientation.gif){:.site-mobile-screenshot}
