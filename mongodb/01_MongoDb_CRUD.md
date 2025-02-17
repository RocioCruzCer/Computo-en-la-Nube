
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



