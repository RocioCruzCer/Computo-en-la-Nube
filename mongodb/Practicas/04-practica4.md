# Arrays y documentos anidados

1. Para hacer esta práctica vamos a cargar unos datos ficticios de empresas.
2. Tienes un fichero denominado “personas.json”

[Colección personas](/mongodb/data/personas.json)

3. Debes poner el resultado de las consultas en cada apartado

**Buscar las personas que vivan en Colombia**

- En Mongo Compass:
 ``` json
 {"direccion.pais":"Colombia"}
```

- En Mongo Console:
 ```json
 db.personas.find({
   "direccion.pais":"Colombia"
})
 ```

**Personas a las que le guste el color rosa**

- En Mongo Compass:
 ``` json
{colores: "pink"}
```

- En Mongo Console:
 ```json
 db.personas.find({
   colores: "pink"
})
 ```

**Personas cuyo tercer color preferido sea el amarillo (yellow). Recuerda que los arrays comienzan por Cero. Deben salir 3**

- En Mongo Compass:
 ``` json
{"colores.2" : "yellow"}
```

- En Mongo Console:
 ```json
 db.personas.find({
   "colores.2" : "yellow"
})
 ```

**Personas cuyo tercer color preferido sea el amarillo (yellow) y que vivan en Mauritania (debe salir uno)**

- En Mongo Compass:
 ``` json
{$and: [
    { "colores": "yellow" },
    { "direccion.pais": "Mauritania" }
  ]}
```

- En Mongo Console:
 ```json
db.personas.find({
  $and: [
    { "colores": "yellow" },
    { "direccion.pais": "Mauritania" }
  ]
})
 ```

**Usando el operador $all averiguar las personas que les gusta el plata (silver) y el salmon (salmon)**

- En Mongo Compass:
 ``` json
{colores:{$all:["silver", "salmon"]}
}
```

- En Mongo Console:
 ```json
db.personas.find({
    colores:{$all:["silver", "salmon"]}
})
 ```

**Con el operador $elemMatch, averigua las personas que les guste el rosa (Pink) o el rojo (red)**

- En Mongo Compass:
```json
{colores:{$elemMatch:{$eq:"pink",$eq: "red"}}}
```

- En Mongo Console:
 ```json
 db.personas.find({
   colores:{$elemMatch:{$eq:"pink",$eq: "red"}}
})
 ```

