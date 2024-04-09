## Practica Agregaciones en MongoDB

`1. Cuenta los productos de tipo “medio”, usando un método básico`

**Código y salida**
```javascript
db.productos.countDocuments({ tipo: "medio" })

25
```

`2. Indicar con un distinct, las empresas (fabricantes) que hay en la colección`

**Código y salida**
```javascript
db.productos.distinct( "fabricante" )

[
  'A.O. Smith',
  'Alere',
  'American Tire Distributors Holdings',
  'Anthem',
  'Archrock',
  'Ascena Retail Group',
  'AutoNation',
  'Best Buy',
  'CIT Group',
  'Cabot',
  'Comcast',
  'Comerica',
  'Core-Mark Holding',
  'DST Systems',
  'Darling Ingredients',
  'Delta Air Lines',
  'Delta Tucker Holdings',
  "Dick's Sporting Goods",
  'First Solar',
  'HCA Holdings',
  'Hanesbrands',
  'Hartford Financial Services Group',
  'Hawaiian Holdings',
  'HealthSouth',
  'Hyatt Hotels',
  'Kar Auction Services',
  'Kelly Services',
  'Kemper',
  'Kimberly-Clark',
  'Lennar',
  'Mercury General',
  'Mondelez International',
  'Motorola Solutions',
  'Nasdaq OMX Group',
  'National Oilwell Varco',
  'Nordstrom',
  'OneMain Holdings',
  'Oneok',
  'Orbital ATK',
  'Pep Boys-Mann',
  'Pool',
  'Precision Castparts',
  'Primoris Services',
  'Raymond James Financial',
  'Seaboard',
  'Securian Financial Group',
  'Simon Property Group',
  'State Farm Insurance Cos.',
  'State Street Corp.',
  'SunPower',
  'TEGNA',
  'Telephone & Data Systems',
  'Total System Services',
  'Tractor Supply',
  'TransDigm Group',
  'Trinity Industries',
  'TrueBlue',
  'Universal American',
  'Universal Health Services',
  'WGL Holdings',
  "Wendy's",
  'Werner Enterprises',
  'WestRock',
  'Williams-Sonoma'
]
```

`3. Usando aggregate, visualizar los productos que tengan más de 80 unidades`

**Código y salida**
```javascript
db.productos.aggregate([{$match:{unidades:{$gt:80}}}])

[
  {
    _id: ObjectId('66149c4ee095814914e34ab2'),
    codigo: 0,
    nombre: 'Fantastic Wooden Fish',
    unidades: 95,
    precio: 291,
    fabricante: 'Kimberly-Clark',
    tipo: 'avanzado'
  },
  {
    _id: ObjectId('66149c4ee095814914e34ab4'),
    codigo: 2,
    nombre: 'Small Soft Fish',
    unidades: 96,
    precio: 189,
    fabricante: 'Primoris Services',
    tipo: 'medio'
  },
  {
    _id: ObjectId('66149c4ee095814914e34abe'),
    codigo: 12,
    nombre: 'Refined Concrete Salad',
    unidades: 90,
    precio: 129,
    fabricante: 'Universal Health Services',
    tipo: 'avanzado'
  },
  {
    _id: ObjectId('66149c4ee095814914e34ad0'),
    codigo: 30,
    nombre: 'Small Rubber Pants',
    unidades: 89,
    precio: 16,
    fabricante: 'Hanesbrands',
    tipo: 'basico'
  },
  {
    _id: ObjectId('66149c4ee095814914e34ad3'),
    codigo: 33,
    nombre: 'Generic Concrete Hat',
    unidades: 82,
    precio: 70,
    fabricante: 'American Tire Distributors Holdings',
    tipo: 'basico'
  },
  {
    _id: ObjectId('66149c4ee095814914e34ae7'),
    codigo: 53,
    nombre: 'Licensed Plastic Hat',
    unidades: 96,
    precio: 38,
    fabricante: 'Best Buy',
    tipo: 'medio'
  },
  {
    _id: ObjectId('66149c4ee095814914e34ae8'),
    codigo: 54,
    nombre: 'Generic Metal Sausages',
    unidades: 84,
    precio: 77,
    fabricante: 'DST Systems',
    tipo: 'medio'
  },
  {
    _id: ObjectId('66149c4ee095814914e34aef'),
    codigo: 61,
    nombre: 'Sleek Rubber Keyboard',
    unidades: 82,
    precio: 33,
    fabricante: 'Alere',
    tipo: 'basico'
  },
  {
    _id: ObjectId('66149c4ee095814914e34af4'),
    codigo: 66,
    nombre: 'Incredible Concrete Fish',
    unidades: 96,
    precio: 336,
    fabricante: 'Darling Ingredients',
    tipo: 'medio'
  }
]
```

`4. Con $project visualizar solo el nombre, unidades y precio de los productos que
tengan menos de 10 unidades`

**Código y salida**
```javascript
db.productos.aggregate([{"$match": {"unidades": {"$lt": 10}}},
  {"$project": {"_id": 0,"nombre": 1,"unidades": 1,"precio":}}
])

[
  { nombre: 'Ergonomic Metal Ball', unidades: 5, precio: 246 },
  { nombre: 'Handmade Plastic Hat', unidades: 7, precio: 253 },
  { nombre: 'Ergonomic Metal Table', unidades: 0, precio: 94 },
  { nombre: 'Practical Frozen Chips', unidades: 0, precio: 305 },
  { nombre: 'Fantastic Metal Pants', unidades: 5, precio: 129 },
  { nombre: 'Intelligent Frozen Sausages', unidades: 3, precio: 111 },
  { nombre: 'Rustic Plastic Mouse', unidades: 5, precio: 24 }
]

```

`5. Con $project ponemos el fabricante, pero le cambiamos el nombre por “empresa”.
Usamos el mismo comando anterior`

**Código y salida**
```javascript
db.productos.aggregate([ { "$match": { "unidades": { "$lt": 10 } } }, { "$project": { "_id": 0, "nombre": 1, "unidades": 1, "precio": 1,"empresa":{"$toString": "$fabricante" } }}] )

[
  {
    nombre: 'Ergonomic Metal Ball',
    unidades: 5,
    precio: 246,
    empresa: 'Seaboard'
  },
  {
    nombre: 'Handmade Plastic Hat',
    unidades: 7,
    precio: 253,
    empresa: "Dick's Sporting Goods"
  },
  {
    nombre: 'Ergonomic Metal Table',
    unidades: 0,
    precio: 94,
    empresa: 'Kelly Services'
  },
  {
    nombre: 'Practical Frozen Chips',
    unidades: 0,
    precio: 305,
    empresa: 'Delta Air Lines'
  },
  {
    nombre: 'Fantastic Metal Pants',
    unidades: 5,
    precio: 129,
    empresa: 'OneMain Holdings'
  },
  {
    nombre: 'Intelligent Frozen Sausages',
    unidades: 3,
    precio: 111,
    empresa: 'A.O. Smith'
  },
  {
    nombre: 'Rustic Plastic Mouse',
    unidades: 5,
    precio: 24,
    empresa: 'Orbital ATK'
  }
]
```

`6. Añadir a la consulta anterior un campo calculado que se llame total y que multiplique
precio por unidades.`

**Código y salida**
```javascript
```

`7. Hacer que el nombre salga en mayúsculas con el operador $toUpper`

**Código y salida**
```javascript

db.productos.aggregate([{$match:{unidades: {$lt: 10}}},{$project: {_id: 0,nombre: {$toUpper:"$nombre"},unidades: 1,precio: 1,empresa: {$toString: "$fabricante"}}}])

[
  {
    unidades: 5,
    precio: 246,
    nombre: 'ERGONOMIC METAL BALL',
    empresa: 'Seaboard'
  },
  {
    unidades: 7,
    precio: 253,
    nombre: 'HANDMADE PLASTIC HAT',
    empresa: "Dick's Sporting Goods"
  },
  {
    unidades: 0,
    precio: 94,
    nombre: 'ERGONOMIC METAL TABLE',
    empresa: 'Kelly Services'
  },
  {
    unidades: 0,
    precio: 305,
    nombre: 'PRACTICAL FROZEN CHIPS',
    empresa: 'Delta Air Lines'
  },
  {
    unidades: 5,
    precio: 129,
    nombre: 'FANTASTIC METAL PANTS',
    empresa: 'OneMain Holdings'
  },
  {
    unidades: 3,
    precio: 111,
    nombre: 'INTELLIGENT FROZEN SAUSAGES',
    empresa: 'A.O. Smith'
  },
  {
    unidades: 5,
    precio: 24,
    nombre: 'RUSTIC PLASTIC MOUSE',
    empresa: 'Orbital ATK'
  }
]

```

`8. Añadir un campo calculado que ponga el nombre del producto y el tipo concatenado
con el operador $concat. Le llamamos al campo “completo”`

**Código y salida**
```javascript
db.productos.aggregate([
  {$match:{unidades: {$lt: 10}}},{$project:{_id: 0,nombre: {$toUpper: "$nombre"},unidades: 1,precio: 1,completo: {$concat: ["$nombre"," - ","$tipo"]},empresa:{$toString: "$fabricante"}}}])

[
  {
    unidades: 5,
    precio: 246,
    nombre: 'ERGONOMIC METAL BALL',
    completo: 'Ergonomic Metal Ball - medio',
    empresa: 'Seaboard'
  },
  {
    unidades: 7,
    precio: 253,
    nombre: 'HANDMADE PLASTIC HAT',
    completo: 'Handmade Plastic Hat - medio',
    empresa: "Dick's Sporting Goods"
  },
  {
    unidades: 0,
    precio: 94,
    nombre: 'ERGONOMIC METAL TABLE',
    completo: 'Ergonomic Metal Table - avanzado',
    empresa: 'Kelly Services'
  },
  {
    unidades: 0,
    precio: 305,
    nombre: 'PRACTICAL FROZEN CHIPS',
    completo: 'Practical Frozen Chips - medio',
    empresa: 'Delta Air Lines'
  },
  {
    unidades: 5,
    precio: 129,
    nombre: 'FANTASTIC METAL PANTS',
    completo: 'Fantastic Metal Pants - basico',
    empresa: 'OneMain Holdings'
  },
  {
    unidades: 3,
    precio: 111,
    nombre: 'INTELLIGENT FROZEN SAUSAGES',
    completo: 'Intelligent Frozen Sausages - basico',
    empresa: 'A.O. Smith'
  },
  {
    unidades: 5,
    precio: 24,
    nombre: 'RUSTIC PLASTIC MOUSE',
    completo: 'Rustic Plastic Mouse - avanzado',
    empresa: 'Orbital ATK'
  }
]
```

`9. Ordena el resultado por el campo “total”`

**Código y salida**
```javascript
db.productos.aggregate([{$match: {unidades: {$lt: 10}}},{$project: {_id: 0,nombre: {$toUpper: "$nombre"},unidades: 1,precio: 1,completo: {$concat:["$nombre"," - ","$tipo"]},empresa: {$toString: "$fabricante"},total: {$multiply: [ "$precio", "$unidades"]}}},{$sort:{total:1}}])

[
  {
    unidades: 0,
    precio: 94,
    nombre: 'ERGONOMIC METAL TABLE',
    completo: 'Ergonomic Metal Table - avanzado',
    empresa: 'Kelly Services',
    total: 0
  },
  {
    unidades: 0,
    precio: 305,
    nombre: 'PRACTICAL FROZEN CHIPS',
    completo: 'Practical Frozen Chips - medio',
    empresa: 'Delta Air Lines',
    total: 0
  },
  {
    unidades: 5,
    precio: 24,
    nombre: 'RUSTIC PLASTIC MOUSE',
    completo: 'Rustic Plastic Mouse - avanzado',
    empresa: 'Orbital ATK',
    total: 120
  },
  {
    unidades: 3,
    precio: 111,
    nombre: 'INTELLIGENT FROZEN SAUSAGES',
    completo: 'Intelligent Frozen Sausages - basico',
    empresa: 'A.O. Smith',
    total: 333
  },
  {
    unidades: 5,
    precio: 129,
    nombre: 'FANTASTIC METAL PANTS',
    completo: 'Fantastic Metal Pants - basico',
    empresa: 'OneMain Holdings',
    total: 645
  },
  {
    unidades: 5,
    precio: 246,
    nombre: 'ERGONOMIC METAL BALL',
    completo: 'Ergonomic Metal Ball - medio',
    empresa: 'Seaboard',
    total: 1230
  },
  {
    unidades: 7,
    precio: 253,
    nombre: 'HANDMADE PLASTIC HAT',
    completo: 'Handmade Plastic Hat - medio',
    empresa: "Dick's Sporting Goods",
    total: 1771
  }
]

```

`10. Haciendo una nueva consulta, averiguar el numero de productos por tipo de producto`

**Código y salida**
```javascript
db.productos.aggregate([{$group: {_id: "$tipo",totalProductos: {$sum: 1}}}])

[
  { _id: 'basico', totalProductos: 24 },
  { _id: 'avanzado', totalProductos: 18 },
  { _id: 'medio', totalProductos: 25 }
]

```

`11. Añadir el valor mayor y el menor`

**Código y salida**
```javascript
db.productos.aggregate([{$group: {_id: "$tipo",totalProductos: {$sum: 1},precioMin: {$min: "$precio"},precioMax: {$max: "$precio"}}}])

[
  { _id: 'basico', totalProductos: 24, precioMin: 16, precioMax: 285 },
  { _id: 'medio', totalProductos: 25, precioMin: 16, precioMax: 337 },
  {_id: 'avanzado',totalProductos: 18,precioMin: 18, precioMax: 331
  }
]

```

`12. Añade el total de unidades por cada tipo`

**Código y salida**
```javascript
db.productos.aggregate([
  {$group:{_id: "$tipo",totalProductos:{$sum: 1},precioMinimo:{$min: "$precio"},precioMaximo:{$max: "$precio"},totalUnidades:{$sum: "$unidades"}}}])

[
  {
    _id: 'avanzado',
    totalProductos: 18,
    precioMinimo: 18,
    precioMaximo: 331,
    totalUnidades: 773
  },
  {
    _id: 'medio',
    totalProductos: 25,
    precioMinimo: 16,
    precioMaximo: 337,
    totalUnidades: 1224
  },
  {
    _id: 'basico',
    totalProductos: 24,
    precioMinimo: 16,
    precioMaximo: 285,
    totalUnidades: 1067
  }
]

```

`13. Con el operador $set y el operador “$substr” visualiza todos los datos del producto
"Small Metal Tuna" y los primeros 5 caracteres del nombre`

**Código y salida**
```javascript
db.productos.aggregate([{$match:{nombre: "Small Metal Tuna"}},{$project:{_id: 0,nombreCompleto: "$nombre",nombreCorto: {$substr:["$nombre",0, 5]},unidades: 1,precio: 1,tipo: 1,fabricante: 1}}])

[
  {
    unidades: 46,
    precio: 43,
    fabricante: 'Raymond James Financial',
    tipo: 'medio',
    nombreCompleto: 'Small Metal Tuna',
    nombreCorto: 'Small'
  }
]

```


`14. Creamos una salida que tenga el nombre del articulo y el total (precio por unidades) y
lo guardamos en una colección denominada productos2`

**Código y salida**
```javascript
db.productos.aggregate([
  {
    $project: {
      _id: 0,
      nombre: 1,
      total: { $multiply: [ "$precio", "$unidades" ] }
    }
  },
  {
    $out: {
      db: "productos2",
      coll: "precio_unidades"
    }
  }
])


```

`15. Comprobamos que se ha creado`

**Código y salida**
```javascript
productos2> show collections

precio_unidades
```


`16. Hacemos un find para comprobar el resultado`

**Código y salida**
```javascript
db.precio_unidades.find()

[
  {
    _id: ObjectId('6614b51cce23ea6c142869fb'),
    nombre: 'Fantastic Wooden Fish',
    total: 27645
  },
  {
    _id: ObjectId('6614b51cce23ea6c142869fc'),
    nombre: 'Rustic Concrete Pants',
    total: 18414
  },
  {
    _id: ObjectId('6614b51cce23ea6c142869fd'),
    nombre: 'Small Soft Fish',
    total: 18144
  },
  {
    _id: ObjectId('6614b51cce23ea6c142869fe'),
    nombre: 'Practical Soft Pants',
    total: 2747
  },
  {
    _id: ObjectId('6614b51cce23ea6c142869ff'),
    nombre: 'Ergonomic Metal Ball',
    total: 1230
  },
  {
    _id: ObjectId('6614b51cce23ea6c14286a00'),
    nombre: 'Small Steel Gloves',
    total: 1665
  },
  {
    _id: ObjectId('6614b51cce23ea6c14286a01'),
    nombre: 'Ergonomic Wooden Shirt',
    total: 14994
  },
  {
    _id: ObjectId('6614b51cce23ea6c14286a02'),
    nombre: 'Handmade Steel Chair',
    total: 5392
  },
  {
    _id: ObjectId('6614b51cce23ea6c14286a03'),
    nombre: 'Handcrafted Soft Gloves',
    total: 4512
  },
  {
    _id: ObjectId('6614b51cce23ea6c14286a04'),
    nombre: 'Fantastic Concrete Salad',
    total: 12985
  },
  {
    _id: ObjectId('6614b51cce23ea6c14286a05'),
    nombre: 'Handmade Plastic Hat',
    total: 1771
  },
  {
    _id: ObjectId('6614b51cce23ea6c14286a06'),
    nombre: 'Refined Wooden Tuna',
    total: 8480
  },
  {
    _id: ObjectId('6614b51cce23ea6c14286a07'),
    nombre: 'Refined Concrete Salad',
    total: 11610
  },
  {
    _id: ObjectId('6614b51cce23ea6c14286a08'),
    nombre: 'Unbranded Soft Fish',
    total: 9434
  },
  {
    _id: ObjectId('6614b51cce23ea6c14286a09'),
    nombre: 'Small Concrete Fish',
    total: 5040
  },
  {
    _id: ObjectId('6614b51cce23ea6c14286a0a'),
    nombre: 'Refined Concrete Bike',
    total: 2700
  },
  {
    _id: ObjectId('6614b51cce23ea6c14286a0b'),
    nombre: 'Tasty Cotton Pants',
    total: 3536
  },
  {
    _id: ObjectId('6614b51cce23ea6c14286a0c'),
    nombre: 'Incredible Granite Gloves',
    total: 20590
  },
  {
    _id: ObjectId('6614b51cce23ea6c14286a0d'),
    nombre: 'Practical Metal Mouse',
    total: 6650
  },
  {
    _id: ObjectId('6614b51cce23ea6c14286a0e'),
    nombre: 'Handcrafted Steel Chicken',
    total: 6215
  }
]
```





 




