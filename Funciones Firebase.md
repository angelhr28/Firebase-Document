# Funciones Firebase

Firebase es un modelo de base de datos NoSql el cual presenta una estructura de datos conformada por Collection (Colecciones) y Document (Documentos); los cuales los grupos de documentos son llamdos Collection.

Teniendo como resultado 


```kotlin
Collection [
    Documentos,
    Documentos,
    Documentos
]
```
Tipo de Datos 

```kotlin
  val docData = HashMap<String, Any?>()
  docData["stringExample"]  = "Hello world"
  docData["booleanExample"] = true
  docData["numberExample"]  = 3.14159265
  docData["dateExample"]    = Date()
  docData["listExample"]    = arrayListOf(1,2,3)
  docData["nullExample"]    = null

  val nestedData = HashMap<String, Any>()
  nestedData["a"] = 5
  nestedData["b"] = true

  docData["objectExample"] = nestedData
```

1.- Creación de Documentos

**Si el documento no existe será creado. Si el documento existe será sobreescrito.**

```kotlin
  val city = HashMap<String, Any>()
  city["name"]    = "Lima"
  city["state"]   = "Lima"
  city["country"] = "Perú"
  
  db.collection("cities").document("PER")
    .set(city)
    .addOnSuccessListener{
      Log.d(TAG, "DocumentSnapshot creado exitosamente")
    }
    .addOnFailureListener{ e->
      Log.d(TAG, "Error escribiendo documento $e")
    }
```

2.- Merge de Documentos

**Actualizamos un campo, el docuemto "PER" es creado si no existe.**
    
```kotlin
  val data = HashMap<String,Any>()
  data[capital] = true 
  
  db.collection("cities").document("PER")
    .set(data, SetOpions.merge()) 
    // SetOptions.merge() mergea un valor al documento inpuesto por el id, si no existe los crea, sino los remplaza.
```

3.- Agregar un Documento

**Nuevo Documento con ID definido:**
    
```kotlin
  db.collection("cities")
    .document("new-city-id")
    .set(data) 
```

**Nuevo Documento con ID autogenerado:**
    
```kotlin
  val data = HashMap<String,Any>()
  data["name"]    = "Lima"
  data["country"] = "Perú"
  
  db.collection("cities")
    .add(data) 
```    
    
4.- Actualizar un Documento
    
```kotlin
  val limaRef = db.collection("cities").document("PER")
```

**Actualizar el campo "isCapital" de la ciudad 'PER'**
    
```kotlin
  limaRef.update("capital",true)
         .addOnSuccessListener{
          Log.d(TAG, "DocumentSnapshot successFully updated!")
         }
         .addOnFailureListener{
          Log.d(TAG, "Error updating document $e")
         }
```

5.- Actualizar un SubDocumento
  
```json
  {
    "name": "Angel",
    "favorites": {
                  "food"   : "Ceviche",
                  "color"  : "Blue",
                  "subject": "recess"
               },
    "age": 12 
  }
```

**Asuma que el documento contiene:**

```kotlin
  db.collection("users")
    .document("Angel")
     .update(
        "age", 22,
        "favorites.color", "Red"
     )
```



6.- Actualizar Lista


```kotlin
  val limaRef = db.collection("cities").document("PER")
```

**Agregar una nueva region al arreglo "regiones"**
  
```kotlin
  limaRef.update(
      "region", FieldValue.arrayUnion("Amazonas")
  )
```

**Remover una nueva region al arreglo "regiones"**
  
```kotlin
  limaRef.update(
      "region", FieldValue.arrayRemove("Amazonas")
  )
```





