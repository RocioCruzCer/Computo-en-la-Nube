# Agregaciones en MongoDB (Framework)

## Métodos para realizar agregaciones simples
- distinct(): Devuelve los valores no duplicados
- countDocumentrs(): Cuenta los documentos en una colección
- estimateDocumentCount(): Cuenta de manera aproximada durante un periodo de tiempo

## Una Aggregation pipeline consta de una o más etapas (stage) que procesan documentos

1. Cada etapa realiza una operación en los documentos de entrada. Por ejemplo, una fase puede filtrar documentos, agrupar documentos y calcular valores.

2. Los documentosque se generan en una fase pasan a la siguiente fase.

3. Puede devolver resultados para grupos de documentos como totales, máximo, minimo, etc

### Se utiliza la clausula "aggregate"

- Existen una serie de operadores que se pueden utilizar para realizar operaciones. Se tienen distintos tipos: etapa, de comparación, booleanos, aritmeticos, de cadena, etc.

## Métodos Simples: countDocument() y distinct

1. contar los documentos de la colección libros
```json
db.libros.countDocuments()
```

2. Contar los documentos de la editorial Terra
```json
db.libros.countDocuments({
    editorial: {$eq:"Terra"}
    })
```
3. Seleccionar o mostrar todos los libros mostrando solamente la editorial
```json
db.libros.find({}, {_id:0, editorial:1})
```

4. Mostrar todos las distintas editoriales
```json
db.libros.distinct("editorial")
```

[Documentación de Agregaciones](https://www.mongodb.com/docs/manual/aggregation/)

## $match. Un pepeline básica
## tienen funciones de etapa
```json
db.libros.aggregate(
    [
        {$match:{editorial:"Terra"}}
    ]
)
```

## $poject. Incluir y renombrar campos
```json
db.libros.aggregate(
    [
        {
            $match:{editorial:"Terra"}
        },
        {
            $project:
            {
                _id:0,
                titulo:1,
                precio:1,
                NombreEditorial:"$editorial",
                editorial:1
            }
        }
    ]
)
```

- Generado por Mongo Compass
```json
[
  {
    $match:
      {
        editorial: "Terra"
      }
  },
  {
    $project:
      {
        _id: 0,
        precio: 1,
        NombreEditorial: "$editorial",
        editorial: 1
      }
  }
]
```

# Crear un campo calculado sencillo con $project
```json
[
  {
    $match:
      {
        editorial: "Terra"
      }
  },
  {
    $project:
      {
        _id: 0,
        titulo: 1,
        precio: 1,
        cantidad: 1,
        "Nombre Editorial": "$editorial",
        "Total de Ganancias": {
          $multiply: ["$precio", "$cantidad"]
        }
      }
  }
]
```

## $sort. Ordenaciones
```json
[
  {
    $match:
      {
        editorial: "Terra"
      }
  },
  {
    $project:
      {
        _id: 0,
        titulo: 1,
        precio: 1,
        cantidad: 1,
        "Nombre Editorial": "$editorial",
        "Total de Ganancias": {
          $multiply: ["$precio", "$cantidad"]
        }
      }
  },
  {
    $sort:
      {
        precio: 1
      }
  }
]
```

## $geoup. Agrupaciones
[Agrupaciones](https://www.mongodb.com/docs/manual/reference/operator/aggregation/group/)


- Cuantos documentos hay por cada una de las editoriales
``` json
[
  {
    $group:
      {
        _id: "$editorial",
        "Numero Documentos": {
          $count: {}
        }
      }
  }
]
```

- Cuantos documentos hay por cada una de las editoriales por número de documentos de manera descendente
``` json
[
  {
    $group:
      {
        _id: "$editorial",
        "Numero Documentos": {
          $count: {}
        }
      }
  },
  {
    $sort:
      {
        editorial: 1
      }
  }
]
```

- Utilizando Mongo Atlas Base de datos sample_airbnb
- Agrupar por tipo de propiedad, mostrando el numero de propiedades y el promedio de sus precios
```json
[
  {
    $group:
      {
        _id: "$property_type",
        Numero: {
          $count: {}
        },
        Media: {
          $avg: "$price"
        }
      }
  }
]
```

- Operador $set
```json
[
  {
    $group:
      {
        _id: "$property_type",
        Numero: {
          $count: {}
        },
        Media: {
          $avg: "$price"
        }
      }
  },
  {
    $set:
      {
        Media_Total: {
          $trunc: "$Media"
        }
      }
  }
]
```

- Orerador $unset
```json

[
  {
    $group:
      {
        _id: "$property_type",
        Numero: {
          $count: {}
        },
        Media: {
          $avg: "$price"
        }
      }
  },
  {
    $set:
      {
        Media_Total: {
          $trunc: "$Media"
        }
      }
  },
  {
    $unset:
      "Media"
  }
]
```

- quitando dos campos
```json
[
  {
    $group:
      {
        _id: "$property_type",
        Numero: {
          $count: {}
        },
        Media: {
          $avg: "$price"
        }
      }
  },
  {
    $set:
      {
        Media_Total: {
          $trunc: "$Media"
        }
      }
  },
  {
    $unset:
      ["Media", "Media_Total"]
  }
]
```

- Creando nuevas colecciones utilizando el operador $out
- Nota: Debe ser el ultimo en la agregacion

```json
[
  ¿
      {
        _id: "$property_type",
        Numero: {
          $count: {}
        },
        Media: {
          $avg: "$price"
        }
      },
  {
    $set:
      {
        Media_Total: {
          $trunc: "$Media"
        }
      }
  },
  {
    $unset:
      "Media"
  },
  {
    $out:
      {
        db: "sample_airbnb",
        coll: "media_propiedades"
        
      }
  }
]
```


- Ejemplos con operadores de comparación y lógicos
```json

[
  {
    $project:
      {
        _id: 0,
        price: 1,
        name: 1,
        room_type: 1,
        caro: {
          $gt: ["$price", 300]
        },
        medio: {
          $and: [
            {
              $gte: ["$price", 100]
            },
            {
              $lte: ["$price", 300]
            }
          ]
        },
        baratito: {
          $lt: ["$price", 300]
        }
      }
  }
]
```

- $cond -> Devuelve valores según una condición (es parecido a un oderador ternario de un lenguaje de programación)
```json
[
  {
    $project:
      {
        _id: 0,
        price: 1,
        name: 1,
        room_type: 1,
        caro: {
          $cond: [
            {
              $gt: ["$price", 300]
            },
            "Si",
            "No"
          ]
        },
        medio: {
          $cond: [
            {
              $and: [
                {
                  $gte: ["$price", 100]
                },
                "Si",
                "No",
                {
                  $lte: ["$price", 300]
                },
                "Si",
                "No"
              ]
            },
            "Si",
            "No"
          ]
        },
        baratito: {
          $cond: [
            {
              $lt: ["$price", 100]
            },
            "Si",
            "No"
          ]
        }
      }
  }
]
```

- Views 
```json
db.createView( "ganancias_libros", 
"libros",
  [
  {
    $match:
      {
        editorial: "Biblio"
      }
  },
  {
    $project:
      {
        _id: 0,
        titulo: 1,
        precio: 1,
        cantidad: 1,
        "Nombre Editorial": "$editorial",
        "Total de Ganancias": {
          $multiply: ["$precio", "$cantidad"]
        }
      }
  },
  {
    $sort:
      {
        "Total de Ganancias": -1
      }
  }
]
)
```