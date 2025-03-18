# Agregaciones

1. Para hacer esta práctica vamos a cargar unos datos ficticios de empresas.
2. Tienes un fichero denominado “productos.json”

[Colección personas](/mongodb/data/productos.json)


3. Debes poner el resultado de las consultas en cada apartado

**Cuenta los productos de tipo “medio”, usando un método básico**

- En Mongo Compass
```json
{tipo: "medio"}
```

- En Mongo COnsole
```json
db.productos.countDocuments({tipo: "medio"})
```

**Indicar con un distinct, las empresas (fabricantes) que hay en la colección**

- En Mongo Console
```json
db.productos.distinct("fabricante")
```

**Usando aggregate, visualizar los productos que tengan más de 80 unidades**

- En Mongo Compass
```json
[
  {
    $match:
      {
        unidades: {
          $gt: 80
        }
      }
  }
]
```

- En Mongo Console
```json
db.productos.aggregate(
    [
        {$match:{unidades:{$gt:80}}}
    ]
)
```

**Con $project visualizar solo el nombre, unidades y precio de los productos que tengan menos de 10 unidades**

- En Mongo Compass
```json
[
  {
    $project: {
      _id: 0,
      nombre: 1,
      unidades: 1,
      precio: 1
    }
  },
  {
    $match:
      {
        unidades: {
          $lt: 10
        }
      }
  }
]
```


**Con $project ponemos el fabricante pero le cambiamos el nombre por “empresa”. Usamos el mismo comando anterior**

- En Mongo Compass
```json
 [
  {
    $project: {
      _id: 0,
      nombre: 1,
      unidades: 1,
      precio: 1,
      Empresa: "$fabricante"
    }
  },
  {
    $match:
      {
        unidades: {
          $lt: 10
        }
      }
  }
]
```


**Añadir a la consulta anterior un campo calculado que se llame total y que multiplique precio por unidades.**

- En Mongo Compass
```json
[
  {
    $project: {
      _id: 0,
      nombre: 1,
      unidades: 1,
      precio: 1,
      Empresa: "$fabricante",
      Total: {
        $multiply: ["$precio", "$unidades"]
      }
    }
  },
  {
    $match:
      {
        unidades: {
          $lt: 10
        }
      }
  }
]
```


**Hacer que el nombre salga en mayúsculas con el operador $toUpper**

- En Mongo Compass
```json
[
  {
    $project: {
      _id: 0,
      nombre: {
        $toUpper: "$nombre"
      },
      unidades: 1,
      precio: 1,
      Empresa: "$fabricante",
      Total: {
        $multiply: ["$precio", "$unidades"]
      }
    }
  },
  {
    $match:
      {
        unidades: {
          $lt: 10
        }
      }
  }
]
```



**Añadir un campo calculado que ponga el nombre del producto y el tipo concatenado con el operador $concat. Le llamamos al campo “completo”**

- En Mongo Compass
```json
[
  {
    $project: {
      _id: 0,
      nombre: {
        $toUpper: "$nombre"
      },
      unidades: 1,
      precio: 1,
      Empresa: "$fabricante",
      Total: {
        $multiply: ["$precio", "$unidades"]
      },
      completo: {
        $concat: ["$nombre", " - ", "$tipo"]
      }
    }
  },
  {
    $match: {
      unidades: {
        $lt: 10
      }
    }
  }
]

```

**Ordena el resultado por el campo “total”**

- En Mongo Compass
```json
 
      _id: 0,
      nombre: {
        $toUpper: "$nombre"
      },
      unidades: 1,
      precio: 1,
      Empresa: "$fabricante",
      Total: {
        $multiply: ["$precio", "$unidades"]
      },
      completo: {
        $concat: ["$nombre", " - ", "$tipo"]
      }
    }
  },
  {
    $match: {
      unidades: {
        $lt: 10
      }
    }
  },
  {
    $sort:
      {
        Total: 1
      }
  }
]
```


**Haciendo una nueva consulta, averiguar el numero de productos por tipo de producto**

- En Mongo Compass
```json
[
  {
    $group: {
      _id: 1,
      totalProductos: {
        $sum: 1
      }
    }
  }
]
```

**Añadir el valor mayor y el menor**  

- En Mongo Compass
```json
[
  {
    $group: {
      _id: "$tipo",
      totalProductos: { $sum: 1 },
      precioMaximo: { $max: "$precio" },
      precioMinimo: { $min: "$precio" }
    }
  },
  {
    $sort: { totalProductos: -1 }
  }
]

```

**Añade el total de unidades por cada tipo**

- En Mongo Compass
```json
[
  {
    $group: {
      _id: "$tipo",
      totalProductos: { $sum: 1 },
      precioMax: { $max: "$precio" },
      precioMin: { $min: "$precio" },
      totalUnidades: { $sum: "$unidades" }
    }
  },
  {
    $sort: { totalProductos: -1 }
  }
]
```



**Con el operador $set y el operador “$substr” visualiza todos los datos del producto "Small Metal Tuna" y los primeros 5 caracteres del nombre.**

- En Mongo Compass
```json
[
  {
    $match: {
      nombre: "Small Metal Tuna"
    }
  },
  {
    $set: {
      nombreUsando$substr: { $substr: ["$nombre", 0, 5] }
    }
  }
]

```


**Creamos una salida que tenga el nombre del articulo y el total (precio por unidades) y lo guardamos en una colección denominada productos2**

- En Mongo Compass
```json
[
  {
    $project: {
      _id: 0,
      nombre: 1,
      Total: { $multiply: ["$precio", "$unidades"] }
    }
  },
  {
    $out: "productos2"
  }
]
```

**Comprobamos que se ha creado**

- En Mongo Console
```json
db.productos2.find()
```

**Hacemos un find para comprobar el resultado**

- En Mongo Console
```json
db.productos2.find()
```


**Usando $cond y $project vamos a visualizar el nombre del producto, el precio y un campo llamado valoración que ponga “barato” si el precio es menor de 250 y caro si es mayor o igual**

- En Mongo Compass
```json
[
  {
    $project: {
      _id: 0,
      nombre: 1,
      precio: 1,
      valoración: {
        $concat: [
          { 
            $or: [
              { $lt: ["$precio", 250] }, 
              { $eq: ["$precio", 250] }
            ] 
            ? "barato" 
            : "caro"
          }
        ]
      }
    }
  }
]
```
