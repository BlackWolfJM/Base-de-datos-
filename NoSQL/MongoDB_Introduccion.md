# **MongoDB: Introducción al Paradigma NoSQL, Colecciones, Documentos y Consultas Básicas**

## Módulo 1: Introducción al Paradigma NoSQL

NoSQL significa Not Only SQL, es decir, No solo SQL. Esto no significa que SQL haya dejado de utilizarse, sino que existen otros modelos de almacenamiento distintos al modelo relacional tradicional.

Estas bases de datos NoSQL resuelven los siguientes problemas:

- La información cambia constantemente
- Existen millones de registros
- Se requiere alta disponibilidad
- Se necesita escalabilidad horizontal
- Los datos pueden tener estructuras diferentes entre sí

Existen varios tipos de bases de datos NoSQL, como por ejemplo:

- Orientada a documentos (MongoDB)
- Clave-valor (Redis)
- Orientada a columnas (Cassandra)
- Orientada a grafos (Neo4j)


## Módulo 2: MongoDB, Documentos y Formato BSON

MongoDB es un sistema de gestión de bases de datos orientado a documentos, que fue desarrollado inicialmente por la empresa 10gen en el año 2009.

Este sistema, en lugar de almacenar la información en filas y columnas, lo hace mediante BSON, que significa Binary JSON y es una representación binaria optimizada del formato JSON.

Este formato tiene la ventaja de que cada documento puede almacenar diferentes tipos de información y estructuras internas complejas, sin la necesidad de modificar previamente el esquema de la base de datos.

Esta característica proporciona flexibilidad para aplicaciones modernas donde los requisitos evolucionan constantemente.

MongoDB tiene una estructura jerárquica que puede entenderse de la siguiente forma:

```
Servidor 
├── Base de datos 
|      ├── Colección 
|              ├── Documento 
|                     |
|                     ├── Campo 
|                     ├── Valor
```

Cada uno de estos niveles cumple una función específica:

- El servidor puede contener múltiples bases de datos.
- Cada base de datos puede contener varias colecciones.
- Cada colección almacena múltiples documentos.
- Cada documento contiene campos con sus respectivos valores.

La unidad básica de almacenamiento en MongoDB es un documento, similar a un objeto JSON, compuesto por claves y valores, como por ejemplo:

```json
{
  "nombre": "Marjorie Alexandra",
  "apellidos": "Cueva Zapata",
  "edad": 21,
  "carrera": "Medicina",
  "promedio": 20
}
```

A diferencia de una fila en SQL, un documento puede contener lo siguiente:

- Arreglos
- Objetos anidados
- Fechas
- Booleanos
- Coordenadas geográficas
- Imágenes de referencia
- Listas dinámicas

Lo que hace MongoDB internamente es convertir el formato JSON en BSON para optimizar su almacenamiento y procesamiento.

## Módulo 3: Instalación de MongoDB con Docker

Si estás utilizando **Docker Desktop**, antes de inicializar el contenedor debes descargar la imagen del sistema de base de datos que vas a utilizar. En este caso vamos a instalar **MongoDB**.

Para descargar la imagen oficial de MongoDB desde la terminal, ejecuta alguno de los siguientes comandos:

```
docker pull mongo
```

o

```
docker pull mongo:latest
```

Ambos comandos realizan lo mismo, ya que descargan la versión estable más reciente de la imagen oficial de MongoDB.

Sin embargo, si deseas instalar una versión específica de MongoDB, puedes indicar el número de versión que deseas instalar, como en el siguiente ejemplo:

```
docker pull mongo:7.0 
```

Si deseas otra versión, cambia el 7.0 por la versión que requieras descargar.

Recuerda que no debe existir un espacio después de los dos puntos (`:`) y el número de la versión a descargar o la etiqueta `latest`, ya que se mostrará un error.

Una vez finalizada la descarga, puedes verificar que la imagen se encuentre disponible en tu equipo mediante el siguiente comando:

```
docker images
```
Después de ejecutarlo en tu terminal deberías observar una salida como la siguiente:

| IMAGE | ID | DISK USAGE | CONTENT SIZE |
|:--------|:------:|------:|------:|
| mongo:7.0 | xxxxxxxxxxx | 1.19GB | 297MB |
| mongo:latest | xxxxxxxxxxx | 1.3GB | 341MB  |

---
## Módulo 4: Creación de un contenedor Docker usando MongoDB

Tras la descarga exitosa de la imagen en el entorno, en este caso local, el siguiente paso consiste en inicializar y desplegar el contenedor de la base de datos NoSQL. Para crear el contenedor, utiliza la siguiente estructura en un solo bloque de código:

```
docker run -d \
  --name [Nombre_contenedor] \
  -p [Puerto_local]:27017 \
  -e MONGO_INITDB_ROOT_USERNAME=[Nombre_usuario_administrador] \
  -e MONGO_INITDB_ROOT_PASSWORD=[Clave] \
  -v [Ruta_carpeta]:/data/db \
  mongo:[version_utilizar]
```

A continuación, se explicará cada línea del comando. Para ello, comenzaremos analizando el primer parámetro:

```
docker run -d
```

Este comando es esencial porque permite crear e inicializar el contenedor a partir de la imagen base.

El parámetro `run` indica a Docker que debe crear una nueva instancia utilizando la imagen que hemos instalado previamente. Por su parte, la opción `-d` (detached) permite que el contenedor se ejecute en segundo plano, dejando la terminal disponible para seguir ejecutando otros comandos. Sin embargo, si se omite dicha opción, la terminal quedará asociada al proceso del contenedor y mostrará sus registros (logs) en tiempo real.

Por otro lado, el siguiente parámetro:

```
--name [Nombre_contenedor]
```
Asigna un nombre personalizado al contenedor.

Siguiendo con:

```
-p [Puerto_local]:27017
```
Este parámetro establece el mapeo de puertos entre el host y el contenedor. En la parte izquierda va el puerto local de la máquina y en la parte derecha va el puerto interno que MongoDB usa por defecto. No olvides que si el puerto ya está en uso se lanzará un error; además, debes recordar que no debe haber espacio entre el puerto local, los dos puntos y el puerto de MongoDB, ya que es un error que se repite mucho.

```
-e MONGO_INITDB_ROOT_USERNAME=[Nombre_usuario_administrador] \
```

Este parámetro permite asignar un nombre al usuario administrador o root, que será creado automáticamente cuando se inicialice MongoDB por primera vez.

```
  -e MONGO_INITDB_ROOT_PASSWORD=[Clave] \

```

Este parámetro establece una contraseña asociada al usuario administrador, y `-e` se utiliza para enviar la variable de entorno al contenedor.

```
  -v [Ruta_carpeta]:/data/db \

```

Permite crear un volumen persistente para almacenar los datos de MongoDB fuera del contenedor; sin embargo, puedes omitirlo:

Si omites -v → los datos se guardan dentro del contenedor. Funcionan normalmente, pero se pierden si eliminas el contenedor.

Si usas -v → los datos se guardan en una carpeta de tu computadora y permanecen aunque elimines o recrees el contenedor.


```
  mongo:[version_utilizar]

```
Este parámetro indica la imagen de MongoDB que Docker utilizará para crear el contenedor. La parte `mongo` corresponde al nombre de la imagen y `[version_utilizar]` especifica la versión exacta que se desea ejecutar.

Este comando es muy importante porque permite controlar la versión de MongoDB que se instalará, garantizando compatibilidad con aplicaciones, librerías y entornos de trabajo. Además, utilizar una versión específica evita problemas derivados de cambios o actualizaciones automáticas entre versiones.


## Módulo 5: Colecciones en MongoDB

Las colecciones en MongoDB son equivalentes a una tabla en una base de datos relacional.

Sin embargo, existen diferencias entre ambas: en SQL todas las filas deben respetar exactamente las mismas columnas, mientras que en MongoDB dos documentos de la misma colección pueden tener estructuras diferentes:

ejemplo

```json
// Documento 1
{
  "nombre": "Pedro",
  "edad": 22
}

// Documento 2
{
  "nombre": "Marilla",
  "edad": 20,
  "carrera": "Medicina"
}
```

Ambos pertenecen a la misma colección.


## Ventajas y desventajas de MongoDB

Ventajas:

- Flexibilidad: No requiere modificar estructuras cada vez que aparecen nuevos atributos.

- Escalabilidad: Puede distribuir información entre múltiples servidores de forma sencilla.

- Alto rendimiento: La lectura y escritura de documentos suele ser muy rápida para grandes volúmenes de datos.

- Desarrollo ágil: Los desarrolladores pueden evolucionar el modelo sin rediseñar completamente la base de datos.

- Integración con JSON: Las estructuras resultan naturales para aplicaciones modernas.

Desventajas:

- No reemplaza completamente a una base de datos relacional.

- Las relaciones complejas pueden resultar más difíciles de implementar.

- Algunas operaciones tipo JOIN requieren estrategias diferentes.

- El diseño incorrecto de documentos puede generar redundancia.

Para evitar este problema es necesario seleccionar adecuadamente la base de datos o el tipo de base de datos según el problema que se quiere resolver.

## Operaciones CRUD

CRUD representa las cuatro operaciones fundamentales de cualquier sistema de base de datos, estas son las siguientes:

- Create
- Read
- Update
- Delete

MongoDB implementa estas operaciones mediante comandos específicos.

**Crear documentos**
```javascript
db.nombrebase.insertOne({
  nombre: "Marjorie",
  edad: 20,
  carrera: "Medicina"
})
```

Nota: Es importante recordar que el sistema crea automáticamente un identificador único llamado `_id`.



**Leer documentos**

```javascript
db.nombrebase.find()
```

Dicho comando recupera todos los documentos existentes en la colección. También es posible aplicar filtros:

```javascript
db.nombrebase.find({
  carrera: "Ingeniería"
})
```

**Actualizar documentos**

```javascript
db.nombrebase.updateOne(
  { nombre: "Marjorie" },
  { $set: { edad: 21 } }
)
```

La respuesta del comando `db.nombrebase.find()`:

```javascript
[
  {
    _id: ObjectId('6850f3a2c1a123456789abcd'),
    nombre: 'Ana',
    edad: 21
  },
  {
    _id: ObjectId('6850f3a2c1a123456789abce'),
    nombre: 'Luis',
    edad: 22
  }
]
```

El operador `$set` modifica únicamente el campo indicado, sin afectar el resto del documento.

**Eliminar documentos**

```javascript
db.nombrebase.deleteOne({
  nombre: "Oreykel"
})
```

También se pueden eliminar múltiples documentos utilizando `deleteMany()`:

```javascript
db.nombrebase.deleteMany({ filtro })
```

Como por ejemplo:

Eliminar por coincidencias exactas:

```javascript
db.usuarios.deleteMany({ 
  estado: "Activo"
});
```

También se puede integrar el uso de operadores:

```javascript
db.usuarios.deleteMany({
  edad: { $lt: 18 }
});
```

Con este comando se borran todos los que sean menores de 18.

Y también se puede borrar absolutamente todos los documentos de la base de datos:

```javascript
db.usuarios.deleteMany({});
```

## Operadores de consulta de MongoDB

**Mayor que**

```javascript
{
  edad: { $gt: 18 }
}
```

**Menor que**

```javascript
{
  edad: { $lt: 18 }
}
```

**Mayor o igual que**

```javascript
{
  edad: { $gte: 18 }
}
```

**Menor o igual que**

```javascript
{
  edad: { $lte: 18 }
}
```

**Diferente**

```javascript
{
  edad: { $ne: 18 }
}
```

Estos operadores permiten construir filtros similares a los de SQL.

## Otros comandos importantes

```javascript
use nombrebase;
```

Este comando activa o crea la base de datos en la que vas a trabajar.

```javascript
show collections;
```

Este comando muestra el nombre de las colecciones que existen dentro de la base de datos.

```javascript
db.dropDatabase();
```
Elimina por completo la base de datos que se está utilizando.

```javascript
db.alumnos.insertMany([
  { nombre: "Luis", edad: 22, ciudad: "Lima" },
  { nombre: "María", edad: 19, ciudad: "Cusco" },
  { nombre: "Pedro", edad: 21, ciudad: "Arequipa" }
]);
```

Este comando inserta varios documentos dentro de una sola colección.

```javascript
db.alumnos.deleteMany({
  carrera: "Ingeniería"
})
```

Elimina todos los documentos que cumplan con la condición, a diferencia de `deleteOne`, que solo elimina el primer documento que encuentre y cumpla con la condición.

## Modelado de documentos

Uno de los aspectos más importantes de MongoDB es decidir cómo organizar los documentos o la información.

Existen dos estrategias principales:

Por ejemplo, los documentos embebidos:

Se almacenan objetos dentro de otros objetos.

ejemplo

```javascript
{
  "nombre": "Marjorie",
  "dirección": {
    "ciudad": "Lima",
    "distrito": "Comas"
  }
}
```

Referencias:

Se almacena únicamente el identificador de otros documentos. Esta estrategia reduce la redundancia cuando la información será compartida por múltiples entidades.

```javascript
{
  "_id": ObjectId("507f1f77bcf86cd799439011"),
  "nombre": "Gabriel García Márquez",
  "pais": "Colombia"
}

{
  "_id": ObjectId("507f1f77bcf86cd799439022"),
  "titulo": "Cien años de soledad",
  "paginas": 471,
  "autor_id": ObjectId("507f1f77bcf86cd799439011") 
}
```

La elección depende del tipo de consulta y de la frecuencia con la que los datos cambian.

## Casos reales de utilización

MongoDB es ampliamente utilizado, por ejemplo, en:

- Uber
- Netflix
- Adobe
- eBay
- Sistemas IoT
- Redes sociales
- Aplicaciones móviles
- Plataformas educativas
- Comercio electrónico
- Sistemas de monitoreo en tiempo real

Su capacidad para manejar millones de documentos lo convierte en una excelente opción para aplicaciones modernas.

## Buenas prácticas

- Diseñar documentos coherentes.
- Evitar duplicar información innecesariamente.
- Crear índices sobre los campos consultados con frecuencia.
- Mantener tamaños adecuados de documentos.
- Utilizar validaciones cuando sea necesario.
- Analizar previamente el patrón de consultas antes de modelar la base de datos.
