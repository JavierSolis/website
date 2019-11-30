---
title: Focus y campos de texto
prev:
  title: Crea y da estilo a un campo de texto
  path: /docs/cookbook/forms/text-input
next:
  title: Manejar los cambios de un campo de texto
  path: /docs/cookbook/forms/text-field-changes
---

Cuando se selecciona un campo de texto y se acepta la entrada, se dice que tiene "focus." 
En general, los usuarios pueden hacer focus a los campos de texto al tocarlos, y los 
desarrolladores pueden hacer focus a los campos de texto usando las herramientas descritas en esta receta. 

Administrar el focus, es una herramienta fundamental para crear formularios con un flujo 
intuitivo. Por ejemplo, supongamos que tienes una pantalla de búsqueda con un campo de 
texto. Cuando el usuario navega hacia la pantalla de búsqueda, puedes hacer focus al campo 
de texto del término de búsqueda. Esto le permite al usuario comenzar a escribir tan pronto 
como la pantalla esté visible, sin necesidad de tocar manualmente en el campo de texto.

En esta receta, aprenderás cómo hacer focus a un campo de texto tan pronto como sea 
visible, así como también a hacer focus a un campo de texto cuando se pulsa un botón.

## Focus a un campo de texto tan pronto como sea visible

Para hacer focus a un campo de texto tan pronto como sea visible, podemos usar la 
propiedad de `autofocus` .

<!-- skip -->
```dart
TextField(
  autofocus: true,
);
```

Para obtener más información sobre el manejo de entradas y la creación de campos de texto, por favor consulta la 
[Sección de formularios del cookbook](/docs/cookbook#formularios).

## Focus a un campo de texto cuando se pulsa un botón

En lugar de enfocar inmediatamente un campo de texto específico, es posible que necesites 
hacer focus a un campo de texto en un momento posterior. En el mundo real, es 
posible que también se necesite hacer focus a un campo de texto específico en respuesta a una llamada de una api o a un error de validación.
En este ejemplo, verás cómo hacer focus a un campo de texto 
después de que el usuario pulsa un botón usando los siguientes pasos: 

  1. Crea un `FocusNode`
  2. Pasa el `FocusNode` a un `TextField`
  3. Dale Focus al `TextField` cuando pulses un botón

### 1. Crea un `FocusNode`

Primero, crea un [`FocusNode`]({{site.api}}/flutter/widgets/FocusNode-class.html).
Usa el `FocusNode` para identificar un `TextField` específico en el "focus tree" de 
Flutter. Esto te permite hacer focus al `TextField` en los siguientes pasos.

Como los nodos focus son objetos de larga vida, administra el ciclo de vida 
usando una clase `State` . Para hacer esto, crea la instancia de `FocusNode` dentro del 
método `initState` de una clase `State`, y límpia este dentro del 
método `dispose`. 

<!-- skip -->
```dart
// Define un widget de formulario personalizado
class MyCustomForm extends StatefulWidget {
  @override
  _MyCustomFormState createState() => _MyCustomFormState();
}

// Define una clase State correspondiente. Esta clase contendrá los datos
// relacionados con el formulario.
class _MyCustomFormState extends State<MyCustomForm> {
  // Define el focus node. Para administrar el ciclo de vida, crea el FocusNode en
  // el método initState, y límpialo en el método dispose
  FocusNode myFocusNode;

  @override
  void initState() {
    super.initState();

    myFocusNode = FocusNode();
  }

  @override
  void dispose() {
    // Limpia el nodo focus cuando se haga dispose al formulario
    myFocusNode.dispose();
        
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    // Rellena esto en el siguiente paso.
  }
}
```

### 2. Pasa el `FocusNode` a un `TextField`

Ahora que tenemos nuestro `FocusNode`, podemos pasarlo a un `TextField` específico 
en el método `build` . 

<!-- skip -->
```dart
class _MyCustomFormState extends State<MyCustomForm> {
  // Código para crear el nodo Focus...

  @override
  Widget build(BuildContext context) {
    return TextField(
      focusNode: myFocusNode,
    );
  }
}
```

### 3. Dale Focus al `TextField` cuando pulses un botón

Finalmente, haz focus al campo de texto cuando el usuario pulsa un botón de acción 
flotante. Utilizaremos el método [`requestFocus`]({{site.api}}/flutter/widgets/FocusScopeNode/requestFocus.html) 
para lograr esta tarea.

<!-- skip -->
```dart
FloatingActionButton(
  // Cuando el botón es pulsado, pide a Flutter que haga focus sobre nuestro
  // campo de texto usando myFocusNode.
  onPressed: () => FocusScope.of(context).requestFocus(myFocusNode),
);
```

## Ejemplo completo

```dart
import 'package:flutter/material.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Text Field Focus',
      home: MyCustomForm(),
    );
  }
}

// Define un widget de formulario personalizado
class MyCustomForm extends StatefulWidget {
  @override
  _MyCustomFormState createState() => _MyCustomFormState();
}

// Define una clase State correspondiente. Esta clase contendrá los datos
// relacionados con el formulario.
class _MyCustomFormState extends State<MyCustomForm> {
  // Define el focus node. Para administrar el ciclo de vida, crea el FocusNode en
  // el método initState, y límpialo en el método dispose
  FocusNode myFocusNode;

  @override
  void initState() {
    super.initState();

    myFocusNode = FocusNode();
  }

  @override
  void dispose() {
    // Limpia el nodo focus cuando se haga dispose al formulario
    myFocusNode.dispose();

    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Text Field Focus'),
      ),
      body: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          children: [
            // El primer campo de texto se enfocará tan pronto como se inicie la aplicación
            TextField(
              autofocus: true,
            ),
            // El segundo campo de texto se enfocará cuando un usuario pulse el
            // FloatingActionButton
            TextField(
              focusNode: myFocusNode,
            ),
          ],
        ),
      ),
      floatingActionButton: FloatingActionButton(
        // Cuando el botón es pulsado, pide a Flutter que haga focus sobre nuestro
        // campo de texto usando myFocusNode.
        onPressed: () => FocusScope.of(context).requestFocus(myFocusNode),
        tooltip: 'Focus Second Text Field',
        child: Icon(Icons.edit),
      ), // Esta coma final hace que el auto-formatting sea más agradable para los métodos build.
    );
  }
}
```

![Text Field Focus Demo](/images/cookbook/focus.gif){:.site-mobile-screenshot}