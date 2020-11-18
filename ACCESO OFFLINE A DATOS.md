# ACCESO OFFLINE A DATOS 

## CONFIGURACION DE LA PERSISTENCIA DE DATOS

```kotlin
    val setting = FirebaseFirestoreSettings.Builder()
                                           .setPersistenceEnabled(true)
                                           .build()
    db.firestoreSettings = settings
```

## CONSULTAS A DATOS OFFLINE

Metodo empleado por firebase con el cual nosotros podremos dar persistencia de datos a nuestros usuarios, dando una mejor iteractividad sin necesidad de una conexion a red, con la cual esta este reactivada se ejecutaran todos los cambios realizados en este estado de offline. 

Monitoreo de datos offline agregados
MetadataChanges.INCLUDE

```kotlin
    db.collection("cities")
      .whereEqualTo("name", "Bogota")
      .addSnapshotListener(MetadataChanges.INCLUDE, EventListener<QuerySnapshot>)
```
    
## PRUEBAS DE ACCESO OFFLINE 

Firebase nos permite realizar pruebas forzando el modo offline y el modo online de la siguiente manera ademas de que nosotros podresmo usar estos metodos para la ejecucion de datos dependiendo del estado de la red. 

```kotlin
    db.disableNetwork()   // Forzamos o notificamos el modo offline a firebase 
      .addOnCompleteListener{
          // Ejecutamos los metodos o funciones de manera offline 
      }
```


```kotlin
    db.enableNetwork()   // Forzamos o notificamos el modo online a firebase 
      .addOnCompleteListener{
          // Ejecutamos los metodos o funciones de manera online 
      }
```