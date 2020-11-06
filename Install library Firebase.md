# Install library Firebase

Puedes usar Firebase Authentication para crear y usar cuentas anónimas temporales a fin de autenticar con Firebase. Estas cuentas se pueden usar para permitir que los usuarios que aún no se hayan registrado en la app trabajen con datos protegidos mediante reglas de seguridad. Si un usuario anónimo decide registrarse para usar la app, puedes vincular sus credenciales de acceso con la cuenta anónima, de manera que pueda continuar usando sus datos protegidos en sesiones futuras.

**Antes de comenzar**

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
