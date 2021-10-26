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

## Enviar una consulta GraphQL anidada
Además de devolver listas o valores escalares, es posible devolver objetos GraphQL desde campos. En esta lección, analizaremos más de cerca el tipo de foto y cómo se puede utilizar para almacenar tipos de datos más complejos.

Me gustaría ajustar esta consulta para incluir una foto. Quiero devolver una foto de cada una de estas mascotas. Veamos si hay algo en el esquema que nos ayude con eso.

Voy a buscar una foto usando este cuadro de búsqueda en la parte superior. Puedo seleccionar una foto. Parece que la foto es un objeto. El tipo de foto tiene campos para imagen de tamaño completo e imagen de tamaño miniatura, los cuales son cadenas. Cada mascota tendría una foto.

Si consulto una foto, me da este mensaje de error que dice: "La foto de campo de la foto tipo debe tener una selección de subcampos".

```
query {
  allPets {
    name
    weight
    category
    photo
  }
  totalPets
}
```

Debido a que la foto es un objeto, necesitaré agregar otro conjunto de selección aquí.

```
query {
  allPets {
    name
    weight
    category
    photo {
      thumb
    }
  }
  totalPets
}
```

Esto nos permite tener cierta flexibilidad cuando enviamos una consulta. Una foto puede tener más de un campo asociado. Luego, podemos solicitar solo los campos que queramos con nuestra consulta GraphQL.

```
query {
  allPets {
    name
    weight
    category
    photo {
      thumb
      full
    }
  }
  totalPets
}
```

## Filtrar el resultado de una consulta GraphQL mediante argumentos

Algunas consultas GraphQL tendrán argumentos. Los argumentos se pueden utilizar para enviar opciones adicionales sobre el conjunto de datos que desea recibir. Se pueden utilizar para especificar preferencias de clasificación, limitar el número de registros devueltos o filtrar datos. En esta lección, filtraremos una lista de mascotas por estado.

Hasta ahora, hemos obtenido datos sobre todas las mascotas de la biblioteca. Tenemos una lista de nuestras mascotas. Tenemos mascotas totales. Total pets nos dice que hay 25 mascotas que forman parte de nuestra biblioteca, pero es posible que desee filtrar esta lista para ver solo las mascotas que están disponibles o solo las mascotas que están retiradas.

Para hacer esto, voy a agregar un argumento GraphQL. Hay un argumento en la consulta de totalPets llamado status. Esto incluirá disponibles o retirados. Si agregamos `AVAILABLE`, veremos que hay 20 mascotas disponibles.

```
query {
  totalPets (status: AVAILABLE)
  allPets {
    name
    weight
    category
    photo {
      thumb
      full
    }
  }
}
```

Si cambiamos esto a `CHECKEDOUT`, veremos que el total de mascotas retiradas es cinco.

```
query {
  totalPets (status: CHECKEDOUT)
  allPets {
    name
    weight
    category
    photo {
      thumb
      full
    }
  }
}
```

Si miramos la consulta totalPets en el esquema, veremos que tiene este argumento de estado opcional.

El valor que necesito enviar es para `PetStatus`. PetStatus es una enumeración, ya sea disponible o extraída. Ahora, como vimos antes, totalPets funcionará sin un filtro, pero si proporciono un filtro de estado, esto filtrará la lista según el valor que proporciono para PetStatus.

## Cambio de nombre de campos con alias GraphQL

Al escribir consultas GraphQL, es posible que desee consultar el mismo campo con diferentes argumentos. En esta lección, mostraremos el problema de nombrar las colisiones en las consultas GraphQL y cómo se pueden resolver con alias.

Nuestra consulta nos dice cuántas mascotas están registradas, pero también quiero ver cuántas mascotas están disponibles. Agregaré la consulta totalPets a la línea dos y agregaré el estado Disponible como argumento.

```
query {
  totalPets (status: AVAILABLE)
  totalPets (status: CHECKEDOUT)
  allPets {
    name
    weight
    category
    photo {
      thumb
      full
    }
  }
}
```

Si intentamos presionar reproducir en esto, veremos algunos errores devueltos.

Nos permite saber que los campos totalPets entran en conflicto, porque tienen argumentos diferentes, y recomienda que usemos alias diferentes en estos campos.

Si me desplazo un poco más hacia abajo, veremos dónde está sucediendo esto, línea dos, columna tres, y línea tres, columna tres. También vemos este pequeño toque de rojo, que nos hace saber que hay algún tipo de problema.

Tenemos una colisión de nombres aquí. Lo que tendremos que hacer es anteponer ambas consultas con un alias. Puedo elegir un nuevo nombre para este campo. Lo llamaré disponible y agregaré dos puntos. Luego agregaré checkOut con dos puntos delante de eso.

```
query {
  available: totalPets (status: AVAILABLE)
  checkedOut: totalPets (status: CHECKEDOUT)
  allPets {
    name
    weight
    category
    photo {
      thumb
      full
    }
  }
}
```

Puedo presionar play, y ahora veo que disponible y checkOut se devuelven. La consulta se realizó correctamente y he cambiado el nombre de estos campos en nuestra respuesta JSON.

Esto significa que también podría tomar totalPets sin ningún filtro.

```
query {
  available: totalPets (status: AVAILABLE)
  checkedOut: totalPets (status: CHECKEDOUT)
  total: totalPets
  allPets {
    name
    weight
    category
    photo {
      thumb
      full
    }
  }
}
```

## Usar variables para filtrar el resultado de una consulta con GraphQL
>Si desea pasar argumentos dinámicos a una consulta GraphQL, puede hacerlo con variables de consulta GraphQL. En esta lección, reemplazaremos valores estáticos o en línea con valores dinámicos y pasaremos datos como JSON a la consulta desde el panel Variables de consulta.

Comenzaremos abriendo una nueva pestaña en GraphQL Playground haciendo clic en el signo más, y usaremos esta nueva pestaña para escribir una nueva consulta. Usaremos nuestra consulta allPets de antes, pero esta vez, queremos pasar algunos argumentos opcionales.

```
query {
  allPets()
}
```

Si echamos un vistazo a todas las mascotas en el esquema, veremos que la categoría y el estado son filtros potenciales para esta consulta. Voy a agregar una categoría: PERRO. Solo queremos ver los perros, y solo queremos ver los perros que están DISPONIBLES.

```
query {
  allPets(category: DOG status: AVAILABLE) {
  }
}
```

Ahora, puedo hacer una selección de identificación, nombre, estado y categoría, y solo veré los perros que están disponibles.

```
query {
  allPets(category: DOG status: AVAILABLE) {
    id
    name
    status
    category
  }
}
```

En este momento, estos valores se pasan en línea como cadenas con la consulta, pero también tenemos la opción de proporcionar valores como valores dinámicos.

Supongamos que desea crear una interfaz de usuario para esto. Es posible que tenga algunos filtros desplegables. Necesitaría algún tipo de mecanismo para devolver la entrada dinámica del usuario. Para pasar valores dinámicos con una consulta GraphQL, usaremos variables.

Lo primero que quiero hacer es configurar estas variables en la línea uno. Usaremos el signo de dólar y la categoría, y luego definiremos qué tipo de categoría es, que es `petCategory`. Luego proporcionaremos el estado, que es `petStatus`.

```
query ($category: PetCategory $status: PetStatus){
  allPets(category: DOG status: AVAILABLE) {
    id
    name
    status
    category
  }
}
```

A continuación, proporcionaremos estas variables a nuestra consulta allPets usando el signo de dólar y el nombre del argumento.

```
query ($category: PetCategory $status: PetStatus){
  allPets(category: $category status: $status) {
    id
    name
    status
    category
  }
}
```

### Query variables panel
```json
{
  "category": "DOG",
  "status": "AVAILABLE"
}
```
Proporcionaré la categoría y el estado para encontrar esos perros disponibles. Estos valores se pasarán luego, y nuestra respuesta debería reflejar eso.

Esto será dinámico, así que puedo cambiarlo a gato. Esto me mostrará todos los gatos disponibles.

```JSON
{
  "category": "CAT",
  "status": "AVAILABLE"
}
```
El lenguaje de consulta GraphQL también nos brinda una forma de pasar valores predeterminados para estas variables. Por ejemplo, si establezco el valor predeterminado con un signo igual a `STINGRAY` para `PetCategory` y hago clic en reproducir, no se utilizará el valor predeterminado. Todavía vamos a extraer esos valores de las variables de consulta.

```
query ($category: PetCategory=STINGRAY $status: PetStatus){
  allPets(category: $category status: $status) {
    id
    name
    status
    category
  }
}
```

Sin embargo, si elimino eso y no paso el valor de gato, usará el valor predeterminado. Si no se proporciona ese valor, usaremos el predeterminado.


## Consultar datos conectados con el lenguaje de consulta GraphQL
> Una de las características más útiles de una consulta GraphQL es que puede recopilar datos sobre varios tipos en una solicitud. Acá veremos cómo usar campos anidados para recopilar datos sobre el tipo de cliente y el tipo de mascota.

Hasta ahora, hemos enviado consultas para el tipo de mascota, y si buscamos en el esquema para mascota, deberíamos ver todos los campos disponibles. Ahora, hay otro tipo principal que forma parte de esta API y se llama `costumer`.

El verdadero poder de GraphQL comienza a aparecer cuando comenzamos a hablar sobre la conexión de puntos de datos. Escribamos algunas consultas que relacionen el tipo de mascota con el tipo de cliente. La consulta que enviaremos es `petById`.

Esto va a tomar id como argumento. Esto va a devolver Biscuit.

```
query {
  petById(id: "S-1") {
    name
  }
}
```
Hay otro campo sobre mascotas llamado `inCareOf` (a cargo de). `inCareOf` devolverá al cliente que haya retirado esta mascota.

```
query {
  petById(id: "S-1") {
    name
    inCareOf {
      name
      username
    }
  }
}
```

Para trazar la línea de cliente a mascota, usaremos la consulta `allCustomers`. `allCustomers` devolverá una lista de clientes, por lo que podemos pedir su nombre, su nombre de usuario y también obtendremos sus mascotas actuales.

```
query {
  allCustomers {
     name
     username
     currentPets {
       name
     }
  }
}
```

`currentPets` devolverá una lista de las mascotas que hayan retirado actualmente. Esa conexión se realiza en el campo `currentPets`. `allCustomers` devuelve una lista de clientes para cada uno de esos objetos de cliente que van a tener una lista de mascotas actuales que están retiradas.

Esto podría ser una matriz vacía o podría devolver una lista de objetos personalizados. Para volver al revés, `inCareOf` va a conectar una mascota con un cliente. En el campo inCareOf, devolveremos al cliente que ha retirado esta mascota.

## Crear nombres de operaciones para consultas GraphQL
> Si hay varias consultas GraphQL en el mismo documento de consulta, debe darle a la consulta un nombre de operación. En esta lección, mostraremos cómo solucionar errores de operaciones anónimos con nombres de operaciones.

En este momento, en el lado izquierdo de nuestra pantalla, tenemos una gran consulta. Está recopilando una gran cantidad de datos sobre los `clientes` y luego sobre las `mascotas`. GraphQL nos permitirá hacer esto. Podemos obtener información sobre varios tipos en una consulta, pero quiero dividir esto en dos consultas separadas, una de las cuales será para `mascotas`. El otro será para `clientes`.

Tan pronto como divida esto en dos consultas separadas, nos encontraremos con un problema. Cuando hago clic en reproducir, hay dos consultas sin nombre.

```
uery {
  availablePets: totalPets(status: AVAILABLE)
  checkedOutPets: totalPets(status: CHECKEDOUT)
  dogs: allPets(category: DOG, status: AVAILABLE) {
    name
    weight
    status
    category
    photo {
      full
      thumb
    }
  }
}

query {
  totalCustomers
  allCustomers {
    name
    username
    dateCreated
    checkoutHistory {
      pet {
        name
      }
      checkInDate
      checkOutDate
    }
  }
}

```

<img src="https://res.cloudinary.com/dg3gyk0gu/image/upload/v1563555708/transcript-images/create-operation-names-for-graphql-queries-query-error.png">

Si hago clic en el segundo de estos, dice: "Esta operación anónima debe ser la única operación definida". Si hay más de una consulta en su documento de consulta, deberá darles un nombre a ambas.

En este momento, estas son consultas anónimas. Piense en ellos como funciones anónimas. Necesitamos darles un nombre. Llamaré al primero, "PetPage". Llamaré al segundo, "CustomerPage".

```
uery PetPage {
  availablePets: totalPets(status: AVAILABLE)
  checkedOutPets: totalPets(status: CHECKEDOUT)
  dogs: allPets(category: DOG, status: AVAILABLE) {
    name
    weight
    status
    category
    photo {
      full
      thumb
    }
  }
}

query CustomerPage {
  totalCustomers
  allCustomers {
    name
    username
    dateCreated
    checkoutHistory {
      pet {
        name
      }
      checkInDate
      checkOutDate
    }
  }
}
```

Ahora, si selecciono el botón de reproducción, veremos el menú desplegable. También veremos el nombre de la operación, por lo que puedo ejecutar estas consultas una a la vez.

<img src="https://res.cloudinary.com/dg3gyk0gu/image/upload/v1563555709/transcript-images/create-operation-names-for-graphql-queries-query-drop-down.png">

Solo para recapitular, siempre que tenga más de una consulta dentro de un documento de consulta, debe darle un nombre de operación. El nombre de la operación puede ser como quieras llamarlo, pero convencionalmente se escribe con `mayúscula`.

## Mutaciones: Utilice un tipo de entrada para crear una cuenta con una mutación GraphQL
> Las mutaciones son otro tipo de operación GraphQL que son similares a las consultas, pero se usan cuando necesitas cambiar cualquier dato en el backend. En esta lección, enviaremos mutaciones para registrar nuevos usuarios.

Para cambiar datos con GraphQL, usamos una mutación. Estos se nombran igual que las consultas y el esquema. Dentro del esquema, tenemos una mutación llamada `createAccount`.

Escribamos esa `mutation`.

Vamos a utilizar la palabra clave de `mutation`. Vamos a utilizar el nombre de la mutación `createAccount`. Parece que toma algo llamado `input`, que es la entrada `createAccount`.

```
mutation {
  createAccount
}
```

Si nos desplazamos un poco hacia abajo y hacemos clic en la entrada, veremos que `createAccount` es en realidad un wrap alrededor del nombre, el nombre de usuario y la contraseña.

Cada vez que creo una cuenta, tendré que proporcionar esas cosas.

Aquí es donde `input` es útil. En lugar de enviar todas estas variables una a la vez, puedo envolverlas en la entrada y luego puedo enviarlas como una sola cosa.

```
mutation ($input: CreateAccountInput!) {
  createAccount(input: $input) {}
}
```

Usaré el panel de variables de consulta para pasar estas variables, pero esta vez vamos a poner todo en esa clave de entrada. Vamos a anidar el objeto aquí con nombre, nombre de usuario y contraseña. Ahora que tengo estos valores de entrada definidos, necesito devolver algo de esta mutación.

### Query variables panel
```
{
  "input": {
    "name": "Eve Porcello",
    "username": "ep123",
    "password": "pass"
  }
}
```

Lo que devuelve esta mutación es un objeto de cliente. Esto nos dará acceso a todos los campos del cliente. Cada vez que creo mi cuenta, se repetirán los detalles de la cuenta que proporcioné en la entrada. Le pediremos nombre de usuario y nombre.

Cuando enviemos esta mutación, enviaremos todos los valores del objeto de entrada. Vamos a recuperar el nombre de usuario y el nombre del cliente que se acaba de crear.

`CreateAccount` toma una entrada llamada `createAccountInput`. Pasamos las variables en el panel de variables de consulta, y luego la mutación devuelve un objeto de cliente para que pueda acceder a los valores que acabo de proporcionar.

<img src="https://res.cloudinary.com/dg3gyk0gu/image/upload/v1563555710/transcript-images/egghead-use-an-input-type-to-create-an-account-with-a-graphql-mutation-account-info-returned.png">

## Autenticar a un usuario con una mutación GraphQL
> Las mutaciones le brindan la capacidad de invocar funciones de backend desde el cliente. En esta lección, usaremos una mutación para autenticar a un usuario con su nombre de usuario y contraseña. Los usuarios autorizados recibirán un token que se puede utilizar para identificar al usuario actual en operaciones futuras.

Ahora que tenemos una cuenta, podemos iniciar sesión. Veamos nuestra lista de mutaciones. Deberíamos ver que hay una mutación de `logIn`. Voy a seguir adelante y escribir eso aquí en nuestro documento de consulta. Usaremos `logIn` con la I mayúscula. Usaremos nuestro `username`, nuestra `password`.

```
mutation {
  logIn(username: "ep123" password: "pass") {

  }
}
```

Lo que se devuelve de la mutación `logIn` es un tipo llamado payload `logIn`. Este es un objeto personalizado que devuelve el cliente, todos los detalles del cliente y el token de usuario. Usaremos el token de usuario para validar que el usuario está autorizado.

<img src="https://res.cloudinary.com/dg3gyk0gu/image/upload/v1563555709/transcript-images/authenticate-a-user-with-a-graphql-mutation-log-in-schema.png">

Cuando enviemos la mutación de inicio de sesión, tendremos acceso a todos los detalles del cliente. Toma su `name`. Vamos a agarrar el `token`.

```
mutation {
  logIn(username: "ep123" password: "pass") {
    customer {
      name
    }
    token
  }
}
```

Sigamos adelante y presionemos reproducir. Vemos el nombre de nuestro cliente, que es el nombre que proporcioné cuando creé mi cuenta. También veo mi token.

<img src="https://res.cloudinary.com/dg3gyk0gu/image/upload/v1563555709/transcript-images/authenticate-a-user-with-a-graphql-mutation-customer-info.png">

Vamos a colocar esto en otro panel aquí en la parte inferior llamado encabezados HTTP.

Ahora, esto es fácil de confundir con las variables de consulta. Nos aseguraremos de estar en la sección de encabezado HTTP y agregaremos la clave de autorización. Agregaremos `Bearer`. Pegaremos este token

### HTTP Headers panel
```
{
  "Authorization": "Bearer your-token-here"
}
```

Una vez que proporcione este token en los encabezados HTTP, podré enviar consultas que son solo para usos autorizados. Ahora la consulta que voy a enviar aquí se llama "me". Me va a dar información sobre mí, el usuario actualmente autenticado.

### GraphQL playground
```
query {
  me {

  }
}
```
La consulta `Me` devuelve los detalles del cliente de cualquier persona que haya iniciado sesión. Aquí consultaré el campo de nombre. Voy a agregar un nombre de operación, porque tengo dos operaciones diferentes aquí en mi documento de consulta. Llamaré a query `Me`, y llamaré a la mutación `LogIn`.

```
query Me {
  me {
   name
  }
}

mutation LogIn {
```

Ahora, puedo enviar esta consulta y debería ver todos los detalles por mí mismo, porque soy un usuario que inició sesión. Desde que inicié sesión, podré registrarme y retirar mascotas.

## Envíe un checkOut Mutation con GraphQL como usuario autorizado
> Una vez que haya iniciado sesión, el usuario podrá ver mascotas con una mutación checkOut. En esta lección, veremos cómo enviar una mutación en función de los datos actuales.

El siguiente paso que quiero dar es buscar una mascota. La mutación de pago requiere una identificación de una mascota para verificar. Necesito mirar las mascotas que están disponibles actualmente y encontrar una mascota para revisar.

Enviaré la consulta de `allPets` con un filtro de estado `Available`. Agarraré la `id`, el `name` y la `category`.

```
query {
  allPets(status: AVAILABLE) {
    id
    name
    category
  }
}
```

Permítanme desplazarme un poco hacia abajo para averiguar a quién quiero ver.

Para hacer el `checkout`, tendré que enviar el id de una mascota disponible. En este punto, puedo agregar una mutación más a nuestro documento de consulta. Nuestra mutación se llamará pago. Le daremos a la mutación un nombre de operación de Checkout, y usaremos la mutación `checkOut`.

Pasaremos la identificación de `S-2` y queremos obtener detalles sobre la mascota, así que tomaremos su nombre. También obtengamos algunos detalles sobre el cliente. El cliente seré yo, pero también tomaré mi nombre.

```
mutation Checkout {
  checkOut(id: "S-2") {
    pet {
      name
    }
    customer {
      name
    }
  }
}
query Me {
  me {
    name
  }
}
```

Cuando envío la mutación de Checkout, debería ver que la mascota ha sido revisada y veo al cliente que ha revisado esa mascota.

<img src="https://res.cloudinary.com/dg3gyk0gu/image/upload/v1563555709/transcript-images/send-a-checkout-mutation-with-graphql-as-an-authorized-user-checkout-mutation.png">

Si observamos la mutación de pago, vemos que devuelve el payload de pago. El payload de pago nos proporciona el objeto del cliente completo, el objeto de mascota completo y la fecha de pago. Estos objetos de respuesta personalizados que podemos devolver para mutaciones son geniales. Podemos obtener los campos de los clientes, los detalles de las mascotas y la fecha de pago, todos agrupados en un solo objeto.

## Cambiar el estado de registro con una mutación GraphQL
>Siguiendo con la mutación checkOut del punto anterior, veremos más de cerca cómo registrar una mascota usando una identificación de mascota. Luego, veremos la consulta de todos los clientes y descubriremos el historial de pagos de mascotas de todos los clientes.

Nos divertimos mucho con la mascota, pero ahora es el momento de volver a registrarla. Tenemos otra mutación con nombre en el esquema llamada `CheckIn`, y esto debería incluir la identificación de la mascota que queremos registrar.

Recuerde, necesitamos tener presente nuestro token de autorización para hacer esto. Revisemos a la misma mascota que revisamos, `S-2`. La mutación `checkIn` devuelve un objeto llamado `checkout`. Averigüemos qué es eso.

`Checkout` devuelve una mascota, una fecha de salida, una fecha de entrada y si la mascota llega tarde o no. Hay un montón de campos de metadatos diferentes en ese tipo. Veamos el nombre de la mascota, obtengamos el `checkOutDate`, obtengamos el `checkInDate` y si la mascota llega tarde o no.

```
mutation CheckIn {
  checkIn(id: "S-2") {
    pet {
      name
    }
    checkOutDate
    checkInDate
    late
  }
}
mutation Checkout {
  checkOut(id: "S-2") {
    pet {
      name
    }
    customer {
      name
    }
  }
}
```

Cuando hago clic en reproducir en esto, deberíamos ver todos esos campos diferentes para nuestra mascota.

<img src="https://res.cloudinary.com/dg3gyk0gu/image/upload/v1563555708/transcript-images/change-check-in-status-with-a-graphql-mutation-check-in-mutation.png">

Ahora que hemos revisado y registrado una mascota, podemos ver estos datos en esta consulta llamada `allCustomers`. Podemos consultar el nombre de usuario, que debería devolvernos todos los nombres de usuario

```
query {
  allCustomers {
    username
  }
}
```

<img src="https://res.cloudinary.com/dg3gyk0gu/image/upload/v1563555709/transcript-images/change-check-in-status-with-a-graphql-mutation-usernames-returned.png">

Vemos mi nombre de usuario aquí en la parte inferior. Entonces también puedo agregar `checkoutHistory`. Ahora, `checkoutHistory` devolverá una lista de pagos. Ese objeto de pago que vimos antes debería tener todos los detalles de la mascota.

```
query {
  allCustomers {
    username
    checkoutHistory {
      pet {
        id
        name
      }
    }
  }
}
```

Vemos los detalles de la mascota de otras personas, y vemos Switchblade aquí hacia la parte inferior. Para cada uno de ellos, podemos encontrar el nombre de la mascota y el ID.

## Reutilizar conjuntos de selección GraphQL con fragmentos
> Los fragmentos son conjuntos de selección que se pueden utilizar en varias consultas. Le permiten refactorizar conjuntos de selección redundantes y son esenciales al consultar uniones o tipos de interfaz. En esta lección, mejoraremos nuestra lógica de consulta creando un fragmento para el conjunto de selección de actividades.

Comencemos agregando una consulta para `allPets`, y queremos filtrar esto por `category`. Buscaremos todos los conejos y también filtraremos por estado para encontrar los conejos DISPONIBLES. A continuación, queremos seleccionar algunos campos.

```
query {
  allPets(category: RABBIT, status: AVAILABLE) {}
}
```

Tomaremos el nombre, el peso y la categoría, y deberíamos ver todos estos conejos disponibles. También queremos apoderarnos de su estatus.

```
query {
  allPets(category: RABBIT, status: AVAILABLE) {
    name
    weight
    category
    status
  }
}
```
Si tan solo hubiera una forma en el lenguaje de consulta GraphQL para hacer que todos estos conjuntos de selección fueran un poco más reutilizables.

La buena noticia es que la hay. Voy a crear un fragmento llamado `PetDetails`. Piense en un fragmento de envoltura de varios campos. `PetDetails` va a ser un fragmento de cierto tipo. Esto será para `Pet`.

```
fragment PetDetails on Pet {

}
```

Luego, cortaremos y pegaremos todos estos campos en el fragmento, y usaremos la sintaxis de extensión, `...`, para insertar todos los PetDetails en esta consulta.

```
query {
  allPets(category: RABBIT, status: AVAILABLE) {
    ...PetDetails
  }
}

fragment PetDetails on Pet {
  name
  weight
  category
  status
}
```

Podemos enviar la consulta y ver que todos los campos todavía se envían con la consulta.

<img src="https://res.cloudinary.com/dg3gyk0gu/image/upload/v1563555709/transcript-images/reuse-graphql-selection-sets-with-fragments-query-still-working.png">

Ahora, también podemos ajustar el fragmento para agregar campos adicionales. Si quisiera tomar la foto y la miniatura, podemos ver que se devuelve la miniatura de la foto.

```
fragment PetDetails on Pet {
  name
  weight
  category
  status
  photo {
    thumb
  }
}
```

<img src="https://res.cloudinary.com/dg3gyk0gu/image/upload/v1563555708/transcript-images/reuse-graphql-selection-sets-with-fragments-photo-thumbnail.png">

Agreguemos un poco a esta consulta y agreguemos `petByID`.

Vamos a buscar la mascota `C-1`, y aquí, podemos reutilizar `PetDetails` dentro de la consulta para no tener que volver a escribirlos.

```
query {
  petById(id: "C-1") {
    ...PetDetails

  }
  allPets(category: RABBIT, status: AVAILABLE) {
    ...PetDetails
  }
}
```

Ahí vamos, conseguimos todos esos campos para esa mascota. También puede agregar campos adicionales junto con su fragmento.

```
query {
  petById(id: "C-1") {
    ...PetDetails
    inCareOf {
      name
      username
```
Deberíamos ver a la persona que ha revisado a esta mascota. Como nos divertimos mucho con los fragmentos, podemos crear un fragmento para `CustomerDetails`. Vamos a especificar que estos campos provienen del tipo de `cliente` y podemos agregar el nombre y el nombre de usuario de `inCareOf`.

```
query {
  petById(id: "C-1") {
    ...PetDetails
    inCareOf {
      ...CustomerDetails
    }
  }
  allPets(category: RABBIT, status: AVAILABLE) {
    ...PetDetails
  }
}

fragment CustomerDetails on Customer {
  name
  username
}
```

<img src="https://res.cloudinary.com/dg3gyk0gu/image/upload/v1563555708/transcript-images/reuse-graphql-selection-sets-with-fragments-all-fields-returned.png">

Vemos que se devuelven todos esos campos. Creamos un fragmento llamado `PetDetails`. Esto tomará todos estos campos y los incluirá en la consulta.

Luego tenemos otro para `CustomerDetails`. Todos los campos que queremos se insertarán en la consulta cuando usemos esa sintaxis de fragmento, ..., y luego el nombre del fragmento.

## Explore las consultas GraphQL refactorizadas
> En esta lección, veremos una biblioteca de mascotas refactorizada que incluye una variedad de nuevas consultas que tienen como objetivo minimizar el uso de argumentos y las colisiones de nombres que requieren alias.

La biblioteca de mascotas acaba de recibir algunos fondos, algo de dinero de capital riesgo, así que vamos a abrir nuestro navegador y pasar a la nueva versión de la aplicación. Vamos a ir a https://funded-pet-library.moonhighway.com.

Notarás nuestro nuevo punto final aquí en el centro de la pantalla. Con un presupuesto mayor, vienen más ingenieros y algunas mejoras en nuestra API, una de las cuales es que tenemos algunas consultas más específicas que pueden ser más fáciles de rastrear.

Escribamos nuestra consulta para `totalPets`.

```
query {
  totalPets
}
```

Veremos 25, pero tenemos estas nuevas consultas aquí, `availablePets`. También hemos `chequeadoOutPet`, y podemos acceder a esos valores sin tener que usar ningún filtro o enviar ningún argumento.

```
query {
  totalPets
  availablePets
  checkedOutPets
}
```

También tenemos otra consulta aquí llamada `customersWithPets`.

```
query {
  customersWithPets {

  }
  totalPets
  availablePets
  checkedOutPets
}
```

Ahora, si miramos esto en el esquema, veremos que esta consulta devolverá una lista de todos los clientes que actualmente tienen mascotas registradas.

Este refactor nos da acceso a los mismos datos, pero no tenemos que usar tantos argumentos, y hemos movido gran parte de la lógica del filtrado, ordenando al servidor en lugar de tener que manejar esto en el area de juego.

```
query {
  customersWithPets {
    username
    name
  }
  totalPets
  availablePets
  checkedOutPets
}
```

<img src="https://res.cloudinary.com/dg3gyk0gu/image/upload/v1563555709/transcript-images/explore-refactored-graphql-queries-query-data.png">


## Consultar tipos de interfaz GraphQL en GraphQL Playground
> Las interfaces son similares a las Uniones en el sentido de que proporcionan un mecanismo para tratar diferentes tipos de datos. Sin embargo, una interfaz es más adecuada para tipos de datos que incluyen muchos de los mismos campos. En esta lección, consultaremos diferentes tipos de mascotas.

Si sumamos la consulta de `allPets` para la identificación y el nombre, veremos la identificación y el nombre devueltos, tal como esperamos, pero en la biblioteca de mascotas, estas relaciones de datos están diseñadas de manera bastante diferente.

Abramos la consulta `AllPets` en nuestro esquema y veremos que la mascota ya no es un tipo, sino que es una interfaz.

Una interfaz es un tipo abstracto que incluye un conjunto de campos. Estos campos deben usarse al crear nuevas instancias de esa interfaz. Tenemos una interfaz llamada `Pet`. Esta es la clase base. Tiene ciertos campos.

Tenemos varias implementaciones diferentes de esa interfaz. Recuerde, nuestro numerador de antes que tenía `Cat`, `Dog`, `Rabbit` y `Stingray`, ahora son implementaciones de esta interfaz.

Si nos desplazamos hacia abajo hasta `Cat`, veremos que `Cat` es un tipo separado que implementa la interfaz `Pet`. Tiene todos los campos diferentes que forman parte de esa interfaz, pero luego podemos extender el Gato para que tenga un par de diferentes, por lo que la `cantidad de sueño` y la `curiosidad` ahora están disponibles en ese Gato.

Hagamos clic en `Stingray`. Ese es otro tipo. También tenemos un par de campos adicionales definidos en eso. Lo mismo con el conejo y el perro. `allPets` devuelve una lista de mascotas, pero todas estas mascotas son de diferentes tipos, diferentes implementaciones de la interfaz de mascotas.

Si quiero ver cuál es cuál, puedo consultar el campo `__typename`, esto me dará los datos sobre qué tipo de mascota estamos consultando.


```
query {
  allPets {
    __typename
    id
    name
  }
}
```

La forma en que escribimos una consulta para una interfaz también es un poco diferente.

Recuerda esos campos extra que teníamos en el gato. Puedo usar un fragmento en línea, `... on Cat`, y luego puedo consultar ese campo. Siempre que haya un gato en la respuesta, veremos un valor `sleepAmount` para el gato. Para todos los demás tipos, no veremos ese campo adicional.

```
query {
  allPets {
    __typename
    id
    name
    ... on Cat {
      sleepAmount
    }
  }
}
```

<img src="https://res.cloudinary.com/dg3gyk0gu/image/upload/v1563555708/transcript-images/query-graphql-interface-types-in-graphql-playground-sleep-amount.png">

Podemos hacer esto para cualquier campo adicional que nos gustaría. Puedo decir sobre `Stingray` y obtener qué tan `chill` calmada es la mantarraya en una escala de 1 a 10. Esto se proporcionará en la respuesta solo para las mantarrayas.

```
query {
  allPets {
    __typename
    id
    name
    ... on Cat {
      sleepAmount
    }
    ... on Stimgray {
      chill
    }
  }
}
```
Una interfaz nos da un poco más de flexibilidad cuando diseñamos los objetos de nuestro dominio. Tenemos una mascota, eso es una interfaz. Algunos campos se basan en esa interfaz de mascota, y luego podemos crear implementaciones de esa interfaz y luego crear campos personalizados para cada tipo de mascota adicional.
