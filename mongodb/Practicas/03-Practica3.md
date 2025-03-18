# Practica 3. Updates y Deletes

1. Cambiar el salario del empleado Imogene Nolan. Se le asigna 8 mil
```json
consultar los datos
 db.empleados.find({nombre:"Imogene"})

consulta
db.empleados.updateOne(
    {nombre:"Imogene", apellidos:"Nolan"},
    {$set:{salario:8000}}
)
```

2. Cambiar "Belgium" por "Belgica" en los empleados (Debe haber dos)
```json
consultar los datos
db.empleados.find({pais:"Belgium"})

consulta
db.empleados.updateMany(
    {pais:"Belgium"},
    {$set:{pais:"Bélgica"}}
)
```

3. Incrementar el salario Incrementar el salario de todos los empleados de google en 1000
```json
consultar los datos
db.empleados.find({empresa:"Google"})

db.empleados.updateMany(
    {empresa:"Google"}, 
    {$inc:{salario:1000}}
)
```

4. Remplazar el empleado Omar Gentry por el siguiente documento
```json
{
"nombre": "Omar",
"apellidos": "Gentry",
"correo": "sin correo",
"direccion": "Sin calle",
"region": "Sin region",
"pais": "Sin pais",
"empresa": "Sin empresa",
"ventas": 0,
"salario": 0,
"departamentos": "Este empleado ha sido anulado"
}
```

```json
db.empleados.replaceOne(
{_id:ObjectId('67ae3f8cb5dd9dcde96d50b8')},
{
"nombre": "Omar",
"apellidos": "Gentry",
"correo": "sin correo",
"direccion": "Sin calle",
"region": "Sin region",
"pais": "Sin pais",
"empresa": "Sin empresa",
"ventas": 0,
"salario": 0,
"departamentos": "Este empleado ha sido anulado"
})
```

5. Con un find comprobar que el empleado ha sido modificado
```json
db.empleados.find({nombre:"Omar", apellidos:"Gentry"})
```

6. Borrar todos los empleados que ganen más de 8500.
Nota: deben ser borrados, 3 documentos
```json
visualizar los datos
db.empleados.find({salario:{$gt:8500}})

consulta
db.empleados.drop({salario:{$gt:8500}})
```

7. Visualizar con una expresión regular todos los empleados con apellidos que comiencen con "R"
```json
db.empleados.find({apellidos:/^R/})
```

8. Buscar todas las regines que contenga un "V". Hacerlo con el operador $regex y que no distinga mayusculas y minuscula. Deben de salir dos
```json
db.empleados.find({region: {$regex: {/v/}}}) 
```

9. Visualizar los apellidos de los empleados ordenador por el propio apellido.
```json
 db.empleados.find({},{_id:0,nombre:0,correo:0,direccion:0,region:0,pais:0,empresa:0,ventas:0,salario:0,departamentos:0}).sort({apellidos:-1})
```

10. Indicar el numero de empleados que trabajan en Google
```json
 db.empleados.find({empresa:"Google"}).size()
```

11. Borrar la coleccion empleados y la base de datos
```json
 db.empleados.drop()
```
```json 
 db.dropDatabase()
```
