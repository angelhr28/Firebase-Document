# Install library Firebase

Puedes usar Firebase Authentication para crear y usar cuentas anónimas temporales a fin de autenticar con Firebase. Estas cuentas se pueden usar para permitir que los usuarios que aún no se hayan registrado en la app trabajen con datos protegidos mediante reglas de seguridad. Si un usuario anónimo decide registrarse para usar la app, puedes vincular sus credenciales de acceso con la cuenta anónima, de manera que pueda continuar usando sus datos protegidos en sesiones futuras.

## Antes de comenzar

1.-Si aún no lo has hecho, agrega Firebase a tu proyecto de Android.
2.-Usa la BoM de Firebase para Android a fin de declarar la dependencia de la biblioteca de Android para Firebase Authentication en el archivo Gradle (generalmente app/build.gradle) de tu módulo (nivel de app).

```kotlin
dependencies {
    implementation platform('com.google.firebase:firebase-bom:25.12.0')
    implementation 'com.google.firebase:firebase-auth-ktx'
}
```
3.-Si aún no conectaste la app al proyecto de Firebase, puedes hacerlo desde Firebase console.
4.-Habilita la autenticación anónima:
    - En Firebase console, abre la sección Auth.
    - En la página Métodos de acceso, habilita el método de acceso Anónimo.
    
    
## Autentica en Firebase de forma anónima    

Cuando un usuario que no accedió a su cuenta usa una función de la app que requiere autenticación en Firebase, sigue estos pasos para que el usuario acceda de forma anónima:

1.-En el método onCreate de tu actividad, obtén la instancia compartida del objeto FirebaseAuth:

```kotlin
private lateinit var auth: FirebaseAuth
// Inicializar Firebase Auth
auth = Firebase.auth
```

2.-Cuando inicialices tu actividad, verifica que el usuario haya accedido:

```kotlin
override fun onStart() {
    super.onStart()
    // Compruebe si el usuario ha iniciado sesión (no nulo) y actualice la interfaz de usuario en consecuencia.
    val currentUser = auth.currentUser
    updateUI(currentUser)
}
```
3.-Por último, llama a signInAnonymously para acceder como usuario anónimo:


```kotlin
auth.signInAnonymously()
    .addOnCompleteListener(this) { task ->
        if (task.isSuccessful) {
            // Iniciar sesión correctamente, actualizar la interfaz de usuario con la información del usuario que inició sesión
            Log.d(TAG, "signInAnonymously:success")
            val user = auth.currentUser
            updateUI(user)
        } else {
            // Si el inicio de sesión falla, muestre un mensaje al usuario.
            Log.w(TAG, "signInAnonymously:failure", task.exception)
            Toast.makeText(baseContext, "Authentication failed.",
                    Toast.LENGTH_SHORT).show()
            updateUI(null)
        }
    }
```

## Convierte una cuenta anónima en una permanente

Cuando un usuario anónimo se registra en la app, tal vez sea conveniente permitirle que continúe su trabajo con su cuenta nueva. Por ejemplo, puede que desees hacer que los elementos que el usuario agregó a su carrito de compras antes de registrarse estén disponibles en el carrito de compras de su cuenta nueva. Para hacerlo, completa los siguientes pasos:

1.-Cuando se registre el usuario, completa el flujo de acceso del proveedor de autenticación del usuario hasta el paso anterior a llamar a uno de los métodos FirebaseAuth.signInWith. Por ejemplo, obtén el token del ID de Google, el token de acceso a Facebook o la dirección de correo electrónico y contraseña del usuario.

2.-Obtén una AuthCredential para el proveedor de autenticación nuevo:

   **Acceso con Google**
```kotlin
    val credential = GoogleAuthProvider.getCredential(googleIdToken, null)
```
   **Acceso con Facebook**
```kotlin
    val credential = FacebookAuthProvider.getCredential(token.token)
```
   **Acceso con correo electrónico y contraseña**
```kotlin
    val credential = EmailAuthProvider.getCredential(email, password)
```

3.-Pasa el objeto AuthCredential al método linkWithCredential del usuario que accedió:

```kotlin
    auth.currentUser!!.linkWithCredential(credential)
        .addOnCompleteListener(this) { task ->
            if (task.isSuccessful) {
                Log.d(TAG, "linkWithCredential:success")
                val user = task.result?.user
                updateUI(user)
            } else {
                Log.w(TAG, "linkWithCredential:failure", task.exception)
                Toast.makeText(baseContext, "Authentication failed.",
                        Toast.LENGTH_SHORT).show()
                updateUI(null)
            }
         }
```
