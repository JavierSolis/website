---
title: Crear y dar estilo a un campo de texto
prev:
  title: Construir un formulario con validación
  path: /docs/cookbook/forms/validation
next:
  title: Centrar el foco en un Text Field
  path: /docs/cookbook/forms/focus
---

Los campos de texto permiten a los usuarios escribir texto en una app. 
Estos se usan para crear formularios, 
enviar mensajes, crear experiencias de búsqueda, y más. 
En esta receta, explora cómo crear y dar estilo a campos de texto.

Flutter proporciona dos campos de texto: 
[`TextField`]({{site.api}}/flutter/material/TextField-class.html) 
y [`TextFormField`]({{site.api}}/flutter/material/TextFormField-class.html).

## `TextField`

[`TextField`]({{site.api}}/flutter/material/TextField-class.html)
es el widget de entrada de texto más comúnmente utilizado.

Por defecto, un `TextField` está decorado con un subrayado. 
Podemos agregar una etiqueta, un icono, un 
texto de ayuda en línea, y un texto de error al suministrar un
[`InputDecoration`]({{site.api}}/flutter/material/InputDecoration-class.html) 
como la propiedad [`decoration`]({{site.api}}/flutter/material/TextField/decoration.html) del 
`TextField`. Para eliminar por completo la decoración (incluido el subrayado y el espacio reservado 
para la etiqueta), configure la `decoration` como nula 
explícitamente.

<!-- skip -->
```dart
TextField(
  decoration: InputDecoration(
    border: InputBorder.none,
    hintText: 'Enter a search term'
  ),
);
```

## `TextFormField`

[`TextFormField`]({{site.api}}/flutter/material/TextFormField-class.html)
envuelve un `TextField` y lo integra con el 
[`Form`]({{site.api}}/flutter/widgets/Form-class.html) adjunto. 
Esto proporciona una funcionalidad adicional, como la validación y la integración con 
otros widgets 
[`FormField`]({{site.api}}/flutter/widgets/FormField-class.html).

<!-- skip -->
```dart
TextFormField(
  decoration: InputDecoration(
    labelText: 'Enter your username'
  ),
);
```

Para más información sobre la validación de entrada, mira la receta 
[Construyendo un formulario con validaciones](/docs/cookbook/forms/validation/).