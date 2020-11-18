# CONSULTAS EN FIREBASE 

## COSULTAS SENCILLAS 

Al igual que multiples gestores de bd para lo optencion de datos debemos realzar consultas mediante querys dirigidos y apuntados a nuestra bd, dando como resultado la informacion solicitada. En cambio dentro de firebase las consultas mediante el dispositivo movil remoto presenta una estructura diferente en el cual nosotros a partir de la referencia de nuestra coleccion insertaremos los filtros necesarios para la obtencion de informacion.

Poseemos 2 tipos de *consultar*

1. Consultas Simples

```kotlin
    //Creamos la referencia a las ciudades de la coleccion 
    val citiesRef = db.collection("cities")

    //Creamos la referencia a querys sobre la coleccion 
    val query = citiesRef.whereEqualTo("name", "Bogota")

    //Obtenemos todas la ciudades capitales
    val db.collection("cities").whereEqualTo("capital", true) 

    //Filtros en la consultas
    citiesRef.whereEqualTo("name","Bogota") // Igualdad de valores

    citiesRef.whereLessThan("population", 100000) // Menor que ...
 
    citiesRef.whereGreaterThan("population", 100000) // Mayor que ...

    // Operaciones Array_containes
    val citiesRef = db.collection("cities")
    citiesRef.whereArrayContains("regions", "Amazonas") // valores que contengan Amazonas en Documents

```

2. Consultas Compuestas

```kotlin
    //Aqui nosotros anidaremos filtros en la misma consulta los cuales vimos en ejemplos de consultas simples
    val db.collection("cities")
          .whereEqualTo("name", "Bogota")
          .whereEqualTo("capital", true)
          .whereLessThan("population", 10000)

    //Rango de filtros validos (un solo campo)
    val db.collection("cities")
          .whereGreaterThanOrEqualTo("country", "Colombia") //mayor o igual que ...
          .whereGreaterThanOrEqualTo("country", "Suecia") //mayor o igual que ...
         
```

3. Ordenamiento y limites en la consulta

    El ordenamiento solo debera ser ejecutado en el mismo campo que se realizo el filtro
```kotlin
    // Ordenamiento por nombre 
    val db.collection("cities")
          .whereEqualTo("name", "Bogota")
          .orderBy("name")
```    
    El limitador si podra realizarce luego de una serie de filtros 
```kotlin
    // Limitacion a 2 resultados 
    val db.collection("cities")
          .whereEqualTo("name", "Bogota")
          .limit(2)
```    


4. Paginacion y cursores de datos
    
    Use los metodos ```StartAt()``` o ```startAfter``` para definir el punto de partida de la consulta.

```kotlin 
    //Obtener todas las ciudades con poblacion >= 1000, ordenadas por population.
    db.collection("cities")
      .orderBy("population")
      .startAt(1000)
```

```kotlin 
    //Obtener todas las ciudades con poblacion <= 1000, ordenadas por population.
    db.collection("cities")
      .orderBy("population")
      .endAt(1000)
```

    Use el snapshot del documento para definir el cursor
  
```kotlin 
    //Obtener data de San Francisco.
    db.collection("cities")
      .document("SF")
      .get()
      .addOnSuccessListener{sp->
            //Obtenemos todas las ciudades con una poblacion mayor que la de san Francisco
            val biggerThanSf = db.collection("cities")
                                 .orderBy("population")
                                 .startAt(sp) 
      }
```



