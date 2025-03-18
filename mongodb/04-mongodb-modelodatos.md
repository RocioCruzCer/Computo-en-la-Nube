# Modelo de Datos en MongoDB

- Modelos 
        - Embebidos
        - Referenciados

- Modelos Embebidos

**Uno a Uno**

```json
db.departamentos.insertMany(
[  
    {
        _id:1,
    nombre: "Tecnologías de la información",
    responsable: {
        nombre: "Monico",
        apellidos: "Martinez Perez"
    },
    descripción: "ejemplo de departamento"
    },
    {
        _id:q2,
    nombre: "Contabilidad",
    responsable: {
        nombre: "Raul",
        apellidos: "López Hernandez"
    },
    descripción: "ejemplo de departamento"
    }
]
)
```

- Modelos Referenciados 

**Uno a Uno**
```json
db.localidades.insertMany(
[
    {
        _id:"BA",
        ciudad:"Buenos Aires",
        pais: "Argentina",
        población: "16 Millones",
        turismo:[
            "edificios",
            "tango",
            "gastronomia",
            "museos",
            "parques"
        ],
        direccion: "Avenida Tortolos",
        cod_departamento: 1
    },
    {
        _id:"SA",
        ciudad:"Santiago",
        pais: "Chile",
        población: "20 Millones",
        turismo:[
            "iglesias",
            "vino",
            "gastronomia",
            "museos"
        ],
        direccion: "Calle soy la vaca del corral",
        cod_departamento: 2
    }
]
)
```
```json
db.departamentos.aggregate(
    [
        {
            $lookup:{
                from:"localidades",
                localField: "_id",
                foreignField:"cod_departamento",
                as: "localidades"
            }
        }
    ]
)
```

    - en mongo
```json
[
  {
    $lookup:
      {
        from: "departamentos",
        localField: "_id",
        foreignField: "cod_departamentos",
        as: "localidades"
      }
  },
  {
    $project:
      {
        _id: 0,
        nombre: 1,
        "localidades.ciudad": 1
      }
  }
]
```


