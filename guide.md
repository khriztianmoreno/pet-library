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

Para comenzar a enviar consultas GraphQL, iremos a http://localhost:4000/. Cuando vayamos a esta ruta, aparecerá una herramienta llamada GraphQL Playground en el navegador.

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
