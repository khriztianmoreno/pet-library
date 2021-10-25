# GraphQL Query Language
GraphQL está ganando terreno como una de las formas más populares de crear una API. Independientemente de la implementación de GraphQL que elija, usará `QL` en GraphQL, el lenguaje de consulta, para consultar datos, cambiar datos con mutaciones y escuchar cambios de datos con suscripciones.

Necesita conocer el lenguaje de consulta independientemente de la implementación del lado del servidor. En este curso, aprenderemos el lenguaje de consulta GraphQL enviando una variedad de operaciones GraphQL a una API existente.

Para empezar, aprenderemos a escribir consultas para obtener todos los datos necesarios para una aplicación en una sola respuesta. A medida que avanza el curso, usaremos mutaciones para agregar y cambiar datos. Para terminar, investigaremos las suscripciones a GraphQL y los datos en tiempo real.

Después del curso, estará listo para comunicarse con una API GraphQL independientemente de la implementación del lado del servidor mediante el lenguaje de consulta GraphQL.

### Qué aprenderás:
- Consulta, mutación y suscripción
- GraphiQL playground
- Schemas
- Fragmentos
- Variables
- Tipos de entrada, uniones, interfaces y payloads de retorno
- Consultas introspectivas

## Enviar una consulta con GraphQL Playground
GraphQL Playground es un IDE para interactuar con una API GraphQL. Las API GraphQL tienen un único endpoint. Las consultas se utilizan para solicitar datos específicos de ese endpoint. En esta lección, enviaremos una consulta para obtener el número total de mascotas registradas en la Biblioteca de mascotas.

Para comenzar a enviar consultas GraphQL, iremos a https://pet-library.moonhighway.com/. Cuando vayamos a esta ruta, aparecerá una herramienta llamada GraphQL Playground en el navegador.

`GraphQL Playground` es un IDE en el navegador que le permite enviar consultas a los endpoint GraphQL. El endpoint de la biblioteca de mascotas está aquí, en el centro de la pantalla. Con GraphQL, solo hay un endpoint, por lo que necesito especificar qué datos quiero escribiendo una consulta.

Escribiré la consulta en el lado izquierdo de la pantalla comenzando con la palabra clave `query`. Voy a pedir `totalPets`. ¿Cuántas mascotas hay en la biblioteca de mascotas?

```graphql
query {
  totalPets
}
```
Cuando hago clic en reproducir, los datos que solicité me serán devueltos como JSON. Observe que la forma de la consulta coincide exactamente con la forma de la respuesta. Todos los campos son iguales.

```graphql
{
  "data": {
    "totalPets": 25
  }
}
```

Enviamos nuestra primera consulta usando GraphQL Playground. A la izquierda, escribí una consulta para describir qué datos quiero obtener de la API de la biblioteca de mascotas. Luego hago clic en reproducir, que envía una solicitud http, la solicitud de publicación a nuestro punto final GraphQL. Recupero los datos como JSON.


## Consultar una lista de objetos con GraphQL
Ahora que entendemos cómo escribir una consulta simple para verificar un valor total, vamos a escribir una consulta para devolver una lista de objetos favoritos. En el camino, aprenderemos un poco más sobre el lenguaje de consulta GraphQL, abordando vocabulario como conjuntos de selección y campos

Agreguemos un poco a nuestra consulta y solicitemos algunos datos sobre las mascotas que están disponibles en la biblioteca de mascotas. Si quisiera una lista de todas nuestras mascotas, consultaré el campo allPets. Abriré nuestras llaves para seleccionar el nombre y el peso de cada una de estas mascotas, y luego haré clic en reproducir.

```
query {
  allPets {
    name
    weight
  }
  totalPets
}
```

Vera que `allPets` devuelve una matriz de mascotas con el nombre y el peso de cada una de ellas.

```
{
  "data": {
    "allPets": [
      {
        "name": "Biscuit",
        "weight": 10.2
      },
      {
        "name": "Jungle",
        "weight": 9.7
      }
    ]
  }
}
```

Además, si colapso el campo allPets, veremos que totalPets también se envía en la consulta y también obtenemos esos datos.

Si echamos un vistazo más de cerca a la consulta, todo lo envuelto con llaves se llama `conjunto de selección`. Cada dato que solicitamos se llama campo. También puedo agregar comentarios a la consulta usando el símbolo de almohadilla o el hashtag.

Entonces, si tuviera que usar esto en uno de los campos, veremos que el nombre ahora se elimina de la consulta y no se devuelve.

## Consultar un tipo de enumeración con GraphQL

En GraphQL Query Language, una enumeración o tipo de enumeración es una lista restringida de valores para un campo en particular. Consultaremos un campo de enumeración, categoría, en esta lección para descubrir las diferentes categorías de mascotas. Esta lección también analiza el esquema GraphQL para la API.

Hemos enviado la consulta `allPets`, por lo que tenemos información sobre la mascota, su nombre y su peso. Ahora, sé que Biscuit y Jungle son gatos, porque sé que son gatos, pero es posible que no los haya memorizado todos.

Queremos saber a qué categoría pertenece la mascota. Cuando usamos GraphQL Playground, podemos presionar control-space. Esto mostrará todos los diferentes campos que están disponibles en esta consulta.

Sigamos adelante y agreguemos una `category` a nuestra consulta. Cuando hago clic en reproducir, deberíamos ver que se devuelve la `category`.

```
query {
  allPets {
    name
    weight
    category
  }
  totalPets
}
```

Ahora, las `categories` parecen cadenas, pero todas parecen bastante similares. GATO, PERRO, MANTARRAYA y CONEJO. GraphQL es un lenguaje de consulta para su API, pero también es un sistema de tipos para su API.

La especificación GraphQL describe un lenguaje de definición de esquema, que usaremos para definir todas las diferentes consultas y todos los diferentes tipos que están disponibles en esta API.

Si hace clic en la pestaña de esquema, puede ver este esquema.

`allPets` devuelve una lista de mascotas y puedo acceder a todos los campos diferentes de este tipo de mascota desplazándome hacia abajo. Aquí, dice que la categoría devuelve una opción llamada petCategory. `petCategory` es un tipo de enumeración que representa una lista restringida de opciones para este campo. Gato, perro, conejo y raya.

Aquí hay otro consejo que puede utilizar al explorar API con GraphQL Playground. Puede colocar el cursor sobre uno de estos nombres de campo y presionar comando. Esto le permitirá hacer clic en ese campo y lo llevará directamente a la definición de ese campo en el esquema.

Podemos hacer eso para el peso, comando-clic, comando-clic para el nombre, pero también podemos hacerlo para allPets y para totalPets.

