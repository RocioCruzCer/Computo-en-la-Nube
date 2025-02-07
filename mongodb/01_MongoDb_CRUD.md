
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

