
# Crud y Consultas en MongoDB

## Crear una Base de Datos
Solo se crea si contiene por lo menos una coleccion

**use db1**

## Como crear una coleccion
use db1
db.createCollection("Empleado")

## Mostrar lac colecciones
show collections

## Insertar un Documento
```json

db.alumnos.insertOne(
{
nombre: 'Soyla',
apellido1: 'Vaca',
edad: 32,
ciudad: 'San Miguel de las Piedras'
}
)

```


## Inserción de un documento más complejo con array
```json

db.alumnos.insertOne(
 {
nombre: "Joaquin",
apellido: "Dorian",
apellidos2: "Guerrero",
edad: 15,
aficiones: [
                 "Cerveza", "Hueva", "Canabis"
              ]
}
)

```


## Inserción de documentos más complejos con documentos aninados y ID
```json
db.alumnos.insertOne(
 {
 nombre: "Jose Luis",
 apellido1: "Herrera",
 apellido2: "Gallardo",
edad: 41,
estudios: [
        "Ingenieria en sistemas computacionales",
        "Maestria en Tecnologias de la informacion"
],
experiencia: {
         lenguaje: "SQL",
         sbd: "SQL Server",
         aniosExp: 14 
}
 }
)
```

```json
db.alumnos.insertOne({
    _id: 3,
    nombre: "Ramos",
    equipo: "Nomterrey",
    aficiones: ["Dinero", "hombres", "Fiesta" ],
    talentos:{
        futbol: true,
        bañarse: false
    }
}
)
```



## Insertar multiples documentos
``` json
db.alumnos.insertMany(
[
    {
        _id: 12,
        nombre: "Roberto",
        apellido: "Gomez",
        edad: "23",
        descripcion: "Es un comediante bueno"
    },
    {
        nombre: "Luis",
        apellido: "Suarez",
        edad: 43,
        habilidades: [
            "correr", "dormir", "morder"
        ],
        direcciones: {
            calle: "del Infierno",
            numero: 666,
            ciudad: "Mexico"
        },
        esposas: [
            {
                nombre:"Marisol",
                edad: 20,
                pension: 350,
                hijos: ["Joaquin", "Bridget"]
            },
            {
                nombre: "Dorien",
                edad: 46,
                pension: 6500.56,
                complaciente: true
            }
        ]
    }
])
```

# Practica

1. Bases de Datos, colecciones e inserts
     . Nos conectamos con mongosh al mongodb
     . Crear una base de datos denominada curso
     ```json
     use curso
     ```

     . Verificar que la base de datos no existe
     ```json
     show dbs / show databases
     ```

     . Crear una coleccion denominada "factura" con el comando create collection y comprobar que aparece, tanto la colección como la base de datos
     ```



# Practica1

## Cargar datos
[libros.json](./data/libros.json)

## Busquedas. Condiciones simples de igualdad. Método find()

1. Señeccionar todos los documentos de la colección libros
``` json
db.libros.find({})
```

2. Mostrar todos los documentos que de de la editorial Biblio
``` json
db.libros.find({editorial:"Biblio"})
```

3. Mostrar todos los documentos que el precio sea 25
``` json
db.libros.find({precio:25})
```

4. Seleccionar todos los documentos donde el titulo sea json para todos
``` json
db.libros.find({titulo:"JSON para todos"})
```

## Operadores de Comparación

[Operadores de Comparación](https://www.mongodb.com/docs/manual/reference/operator/query/)


![Operadores de COmparación](./img/Operadores%20Relacionales.png)

1. Mostrar todos los documentos donde el precio sea mayor a 25
``` json
db.libros.find({
    precio: { $gt: 25
    }
})
```

2. Mostrar los documentos donde el precio sea 25
``` json
db.libros.find({
    precio: { $eq: 25
    }
})
```
3. Mostrar los documentos cuya cantidad sea menor a 5
``` json
db.libros.find({
    cantidad: { $lt: 5
    }
})
```

4. Mostrar los documentos que pertenezcan a la editorial Biblio o Planeta
``` json
db.libros.find({
    editorial:{$in:["Biblio", "Planeta"]}
    }
)
```

5. Mostrar todos los documentos de libros que cuesten 20 o 25
``` json
db.libros.find({
    precio:{$in:[20, 25]}
    }
)
```

6. Mostrar todos los documentos de libros que no cuesten 20 o 25
``` json
db.libros.find({
    precio:{$nin:[20, 25]}
    }
)
```

7. Mostrar el primer documento de libros, que cueste 20 o 25
``` json
db.libros.findOne({
    precio:{$in:[20, 25]}
    }
)
```

## Operadores Lógicos

[Operadores Lógicos](https://www.mongodb.com/docs/manual/reference/operator/query/)


![Operadores Lógicos](./img/Operadores%20Lógicos.png)

### Operador AND

Dos posibles opciones de AND 
1. La simple, mediante condiciones, separadas por comas
***sintaxis:***

db.coleccion.find({condicion1, condicion2}) -> con esto asume que es una ***AND***

2. Usando el operador $and
***sintaxis:***
db.coleccion.find({$and:[{condicion1}, {condicion2}]}) 




#### Ejercicios

***forma simple***
1. Mostrar todos aquellos libros que cuesten más de 25 y cuya cantidad sea inferior a 15

``` json
db.libros.find({ precio:{$gt:25}, cantidad:{$lt:15 }})
```

2. Mostrar todos aquellos libros que cuestem más de 25 y cuya cantidad sea inferior a 15 y id igual a 4

``` json
db.libros.find({ precio:{$gt:25}, cantidad:{$lt:15 }, _id:4})
```

``` json
db.libros.find({ precio:{$gt:25}, cantidad:{$lt:15 }, _id:{$eq:4}})
```



***forma AND***

1. Mostrar todos aquellos libros que cuesten más de 25 o cuya cantidad sea inferior a 15

```json
db.libros.find(
    {
        $and:[
            {precio:{$gt:25}},
            {cantidad:{$lt:15}}
        ]
    }
)
```

### Operador OR 

#### Mosrtar todos aquellos lirbos que cuesten mas de 25 o cuya cantidad sea inferior a 15

```json
db.libros.find(
    {
        $or:[
            {precio:{$gt:25}},
            {cantidad:{$lt:15}}
        ]
    }
)
```


### AND y OR combinadas

1. Mostrar los libros de la editorial biblio, con un precio mayor a 40 o libros de la Editorial planeta con un precio mayor a 30

``` json
db.libros.find(
{
    $or:[
        {$and:[{editorial:"Biblio"}, {precio:{$gt:30}}]},
        {$and:[{editorial:{$eq:"Planeta"}}, {precio:{$gt:20}}]}
    ]
})
```
***forma simple***

``` json
db.libros.find(
{
    $or:[
        {editorial:"Biblio", precio:{$gt:30}},
        {editorial:{$eq:"Planeta"}, precio:{$gt:20}}
    ]
}
)
```


## Proyeccion de Columnas

***Sintaxis***
``` JSON
db.coleccion.find(filtro, columnas)

db.libros.find({}, {titulo:1})
```

1. Seleccionar todos loa documentos mostrando el titulo y la editorial

``` json
db.libros.find({}, {titulo:1, editorial:2})

sin ID
db.libros.find({}, {titulo:1, editorial:2, _id:0})
``` 

2. Seleccionar todos los documentos de la editorial planeta mostrando solamente el titulo y la editorial

``` json
db.libros.find({editorial:"Planeta"}, {titulo:1, editorial:1, _id:0})
``` 

## Operador exists (permite saber si un campo o no se encuentra en un documento)

```json
db.libros.find(
{
    editorial:{$exists:true}
})
```

```json
db.libros.insertOne(
{
    _id:10,
    titulo:"Mongo en entornos gráficos",
    editorial: "Terra",
    precio:125
})
```

1. Mostrar todos los documentos que no contengan el campo cantidad
```json
db.libros.find(
{
    cantidad:{$exists:false}
})
```

## Operador Type (permite preguntar si un determinado campo corresponde con un tipo)

[Operador Type](https://www.mongodb.com/docs/manual/reference/operator/query/type/#mongodb-query-op.-type)

1. Mostrar todos los documentos donde el precio sean dobles
```json
db.libros.find({precio:{$type:1}})

sin id
db.libros.find({precio:{$type:1}}, {_id:0})

sin id y sin cantidad
db.libros.find({precio:{$type:1}}, {_id:0, cantidad:0})

para enteros
db.libros.find({precio:{$type:16}})
```

```json
db.libros.insertOne(
{
    _id:11,
    titulo:"IA",
    editorial: "Terra",
    precio:125.4,
    cantidad: 20
})
````

```json
db.libros.insertMany([
 {
    _id: 12,
    titulo: 'IA',
    editorial: 'Terra',
    precio: 125, 
	cantidad: 20
  },
  {
    _id: 13,
    titulo: 'Python para todos',
    editorial: 2001,
    precio: 200, 
	cantidad: 30
  }]
  )
```

2. Seleccionar todos los documentos donde la editorial sea de tipo entero
```json
db.libros.find({editorial:{$type:16}})
db.libros.find({editorial:{$type:int}})

```

3. Seleccionar todos los documentos donde la editorial sea de tipo string
```json
db.libros.find({editorial:{$type:2}})
db.libros.find({editorial:{$type:"string"}})
```

## Practica de Consultas
1. Instalar las tools de mongodb
[DatabaseTools](https://www.mongodb.com/try/download/database-tools)

2. Cargar el json empleados (Debemos estar ubicados en la carpeta donde se encuentra el JSON empleados)

- En Local:
comando:
mongoimport --db curso --collection empleados --file empleados.json

- En Docker
comando:
mongoimport --db curso --collection empleados --file empleados.json --port 27018



# Modificando Documentos
## Comandos importantes

1. updateOne -> Modificar un solo documento
2. updateMany -> Modificar multiples documentos
3. replaceOne -> Sustituir el contenido completo de un documento


Tiene el siguiente formato
```json
db.collection.updateOne(
{filtro},
{Operador: }
)
```

[Operadores Update](https://www.mongodb.com/docs/manual/reference/operator/update/)

### Operador set

1. Modificar un documento 
```json
  db.libros.updateOne(
  {titulo:"Python para todos"},{$set:{titulo:"Java para Todos"}}
  )
```

2. Actualizar el precio a 100 y la cantidad a 50 para el _id:10
```json
  db.libros.updateOne({_id:10},{$set:{precio:100,cantidad:50}})
```

#### Modificar multiples documentos

-- Modificar todos los documentos donde el precio sea mayor a 100 a un precio de 150
```json
db.libros.updateMany(
    {precio:{$gt:100}},
    {$set:{precio:150}}
)
```

2. Operador $inc y $mul

- actualiza con un incremento de 5 todos los documentos
```json
db.libros.updateMany(
    {}, 
    {$inc:{precio:5}}
)
```

- actualiza con multiplicación de 2 todos los documentos de la catidad que sean mayores a 20
```json
db.libros.updateMany(
    {cantidad:{$gt:20}}, 
    {$mul:{cantidad:2}}
)
```


- Actualizar todos los documentos donde el precio sea mayor a 20 y se multiplique por 2 la cantidad y el precio
```json
db.libros.updateMany(
    {precio:{$gt:20}}, 
    {$mul:{cantidad:2}, $mul:{precio:2}}

)
```

3. Remplazar Documentos (replaceOne)
```json
ver por _id 2
db.libros.find({_id:2})


db.libros.updateOne({_id:2},{$set:{precio:56, existencia:10}})


db.libros.replaceOne({_id:2},{titulo:"De la tierra a la luna", autor:"Julio Verne", precio:500})
```


# Borrar Documentos
1. deleteOne -> Elimina un solo documento
2. deleteMany -> Elimina Multiples documentos

1. Eliminar el documento con id 2
```json
db.libros.deleteOne({_id:2})
```

2. Eliminar los documentos donde la cantidad sea mayor o igual a 150 
```json
para buscarlos
db.libros.find({cantidad:{$gte:150}})

db.libros.deleteOne({cantidad:{$gte:150}})
```

# Expresiones Regulares 

1. Buscar los libros que contenga el titulo la leta t
```json
db.libros.find({titulo:/t/})
```

2. Buscar los libros que en el titulo contengan la palabra json
```json
db.libros.find({titulo:/JSON/})
```

3. Buscar todos los documentos que en el titulo termimen el tos
```json
db.libros.find({titulo:/tos$/})
```

4. Todos los documentos que en el titulo comiencen con J
```json
db.libros.find({titulo:/^J/})
```

# Operador $regex
[Operador Regex](https://www.mongodb.com/docs/manual/reference/operator/query/regex/)

- Seleccionar los libros que contengan la palabra para
``` json
db.libros.find({titulo:{$regex: 'para'}})
```

```json
db.libros.find({titulo: {$regex: /JSON/}})
```

- Distinguir entre mayúsculas y minúsculas
```json
db.libros.find({titulo: {$regex: {/json/}}}) ->No distingue entre mayúsculas y minúsculas
```

```json
db.libros.find({titulo: {$regex: /json/, $options:"i"}}) -> Si distingue entre mayúsculas y minúsculas
```

```json
db.libros.find({titulo: {$regex: /json/i}})
```

- Seleccionar todos los libros que comiencen con J o j
```json
db.libros.find({titulo: {$regex: /^j/i}})
```

- Seleccionar todos los libros que terminen es
```json
db.libros.find({titulo: {$regex: /es$/i} })

db.libros.find({titulo: {$regex: 'es$', $options: 'i'} })
```

# Método sort (Ordenar documentos)

1. Ordenar los libros de manera ascendente por el precio
```json
db.libros.find({}, {titulo:1, precio:1, _id:0}).sort({precio: 1})
```

2. Ordenar los libros de manera descendente por el precio
```json
db.libros.find({}, {titulo:1, precio:1, _id:0}).sort({precio: -1})
```

3. Ordenar los libros de manera ascendente por la editorial y de manera descendente por el precio, mostrando el titulo, el precio y la editorial
```json
db.libros.find({}, {titulo:1, precio:1, editorial:1, _id:0}).sort({editorial: 1, precio:-1})
```

# Otros métodos: skip, limit, size

```json
db.libros.find({}).size()

db.libros.find({titulo: {$regex:/Java/i}}).size()
```

- Buscar todos los libros pero mostrando los 2 primeros
```json
db.libros.find({}, {titulo:1, editorial:1, precio:1, _id:0}).limit(2)
```

- Mostrar los 3 últimos libros 
```json
db.libros.find({}, {titulo:1, editorial:1, precio:1, _id:0}).sort({precio:-1}).limit(3)

db.getCollection('libros')
  .find({}, { titulo: 1, editorial: 1, _id: 0 })
  .sort({ titulo: -1 })
  .limit(4);
```

```json
db.libros.find({}, {titulo: 1, editorial:1, precio:1}).skip(2)
```

-Seleccionar todos los libros ordenados por título de forma descendente saltando los dos primeros documentos y mostrando el tamaño

```json
db.libros.find({}, {titulo:1, editorial:1, precio:1, _id:0}).sort({titulo:-1}).skip(2).size()
```

# Borrar colecciones y base de 
```json
use db5
db.createCollection('ejemplo')
show collections

db.ejemplo.insertOne({nombre: 'Chapuin'})

db.ejemplo.drop()

db.dropDatabase()
```



