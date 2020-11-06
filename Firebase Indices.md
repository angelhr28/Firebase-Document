# Firebase Indices

**Campo único** 

  Almacena el orden de todos los documentos en una colección de un sólo campo.

**Campo compuesto** 

  Almacena el orden de todos los documentos en una coleción que contiene subcampos.

## Modos índices

**Ascending**

  Maneja los siguientes operadores <, <=, ==, >= y >. Ademas de que el resultado de la consulta estara de orden ascendente.   

**Descending**

  Maneja los siguientes operadores <, <=, ==, >= y >. Ademas de que el resultado de la consulta estara de orden descendente.

**Array-conteins**

  Retornara los elementos que se encuentran el ID dentro de la lista de objetos de la tabla creada.

## Auto indexacion 
  Por defecto se crean los siguientes índices: 
  - **Campo simple** (no colecciones): Dos índices de campo único (ascendente y descendente)
  - **Diccionarios** : Dos índices campo único (ascendente y descendente) por cada subcampos
  - **Colecciones**  : Índice array-contains
  
## Índice campos único
  Este tipo de índice te permite realizar consultas usando: 
  - Comparadores <, <=, ==, >=, arreglos array_contains

## Operadores Condicionales 
  Dentro de la libreria de firebase la cual se conecta con el la base de datos remota NoSQL nosotros deberemos poder realizar consultas mediantes campos de los documentos, o en cualquier otro caso por los indices por ende se puede realizar un metodo llamado ```where``` que al igual que en un lenguaje plsql sirve para filtras la consulta por campos.
  
  Ejemplo: 
  ```kotlin
    citiesRef.where ( "name", "==", "Lima" )
    
    citiesRef.where ( "population", "<", 100000 )
    
    citiesRef.where ( "regions", "array-constains", "Amazonas" )
  ```
    
    
  
