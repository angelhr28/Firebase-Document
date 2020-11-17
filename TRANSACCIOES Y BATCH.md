# METODOS DE MANIPULACION DE DATOS POR TRANSACIONES O BATCH 

## TRANSACCCIONES 

Metodo de lectura y escritura para la manupulacion de datos con la cual nosotros podresmo validar o cualquier modificacion en esta bd.

``kotlin
    val bogDocRef = db.collection("cities").document("BOG") 

    db.runTransaction{
        val snapshot = transaction.get(bogDocRef) // Obtengo el valor en tiempo real de un valor existente dentro de la bd 
        val newPopulation = snapshot.getDouble("population") ?: 0 + 1 // Altero el valor origial obtenido 
        transaction.update(bogDocRef, "population", newPopulation) //Inserto mi cambio dentro de la bd y actualizo 
        null
    }.addOnSuccessListener{
        Log.e(TAG, "Todo correcto prro")
    }.addOnFailureListener{ e->
        Log.e(TAG, "revisa este mmda $e")
    }

``

## BATCH 

Metodo exclusivo de escritura en el cual podremos cambiar valores en la base de datos de firebsase.
``kotlin
    //Obtenemos un nuevo batch de escritura
    val batch = db.batch()

    //Asignamos el valor de 'BOG'
    val bogotaRef = db.collection("cities").document("BOG")
    batch.set(bogotaRef, city()) // city es un clase que contiene los datos de una ciudad

    //Actualizamos el atributo population de 'BOG'
    val bogotaRef = db.collection("cities").document("BOG")
    batch.update(bogotaRef,"population", 100000000L)
``

Eliminacion de datos a travez de batch

``kotlin 
    //Eliminar la ciudad 'BOG'  
    val bogotaRef = db.collection("cities").document("BOG)
    batch.delete(bogotaRef)

    //Realizamos el commit del batch
    batch.commit().addOnCompleteListener{} //Verificamos que eliminaremos un dato de la db 
``

## CUANDO USAR TRANSACCION O BATCH 

Se realiza cuando tenermos operacione simultaneas en una misma coleccion o cuando vamos a realizar migraciones de datos de manera masiva, ademas de cuando nosotros cambiamos el modelo de datos definidos 
