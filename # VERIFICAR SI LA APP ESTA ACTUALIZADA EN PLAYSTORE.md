# VERIFICAR SI LA APP ESTA ACTUALIZADA EN TIENDA  

## LIBRERIAS 
    Primero debemos implementar la siguiente libreria para realizar la consulta a la tienda playstore y poder leer el achivo html que nos retornara.

``kotlin
implementation 'org.jsoup:jsoup:1.11.3'
``

## CODIGO DE PETICION DE INFORMACION DE APLICACION EN PLAYSTORE 

``kotlin

import android.os.AsyncTask
import org.jsoup.Jsoup
import java.io.IOException

open class UpdateVersionApp : AsyncTask<Void, String, String>() {

    var list = mutableListOf<UpdateVersionModal>()

    override fun doInBackground(vararg voids: Void): String? {
        var newVersion: String? = null
        try {
            val document = 
                Jsoup.connect("https://play.google.com/store/apps/details?id=" + ctx.packageName + "&hl=en")//RUTA DE EL APLICATIVO EN PLAYSTORE MEDIANTE EL ID QUE SERA EL PACKAGENAME
                     .timeout(30000)
                     .referrer("http://www.google.com")
                     .get()

            document?.let {
                val element = it.getElementsContainingOwnText("Current Version") // EXTRAEREMOS EL FRAGMENTO HTML QUE CONTIENE LA VERSION ACTUAL 
                for (ele in element) {
                    if (ele.siblingElements() != null) {
                        val sibElemets = ele.siblingElements()
                        for (sibElemet in sibElemets) {
                            newVersion = sibElemet.text() // LA ALMACENAMOS EN UNA VARIABLE 
                        }
                    }
                }
            }
        } catch (e: IOException) {
            e.printStackTrace()
        }
        return newVersion
    }

    override fun onPostExecute(onlineVersion: String?) {

        if (onlineVersion != null && onlineVersion.isNotEmpty()) {
            val isUpadate = isModalActualizacion(onlineVersion) // COMPARAMOS LA VERSION EXTRAIDA DE PLAYSTORE CON LA QUE TENERMOS EN NUESTRO PAQUETE
            list.forEach {
                if(isUpadate) it.createModalInflate(onlineVersion) // MOSTRAMOS UN MODAL AVISANDO QUE DEBE ACTUALIZAR SU APP
            }
        }
    }

    fun inject(delegate: UpdateVersionModal){
        if (!list.contains(delegate))
            list.add(delegate)
    }

    companion object {
        private var instance: UpdateVersionApp? = null
        fun getInstance(delegate: UpdateVersionModal): UpdateVersionApp {
            if (instance == null) {
                instance = UpdateVersionApp()
            }
            instance?.inject(delegate)
            return instance ?: UpdateVersionApp()
        }

        fun destroyInstance() {
            instance = null
        }
    }

}

interface UpdateVersionModal{
    fun createModalInflate(newVersion:String)
}


fun isModalActualizacion(newVersion: String = ctx.getVersionName()) : Boolean { // FUNCION DE APOYO QUE COMPARA LAS VERSIONES LA ACTUAL EN EL PROYECTO CON LA QUE ESTA EN TIENDA 
    return try {
        val newArray = newVersion.split(".")
        val oldArray = ctx.getVersionName().split(".")
        var isVisible = false
        if(newArray.size > oldArray.size || newArray.size != oldArray.size) return true

        for(i  in newArray.indices){
            if(newArray[i].toInt() >  oldArray[i].toInt()){
                isVisible =  true
                break
            }
            isVisible = false
        }
        return isVisible
    }catch (e: Exception){
        false
    }
}

``

## IMPLEMENTACION 

``kotlin
    // DECLARAMOS NUESTRA CLASE DENTRO DE LA VISTA PRINCIPAL O LAS VISTAS QUE DESEAMOS QUE SE MUESTRE EL MODAL DE ACTUALIZACION
    lateinit var updateVersionApp : UpdateVersionApp

    // NOTA: LA INTERFACE UpdateVersionModal SE DEBE IMPLEMENTAR EN CADA VISTA DONDE SE DECLARE UpdateVersionApp  

    // EN EL ONCREATE O ONCREATEVIEW INICIALIZAREMOS LA CLASE ENVIANDO COMO PARAMETRO LA INSTANCIA DE LA INTERFACE IMPLEMENTADA 
    updateVersionApp = UpdateVersionApp.getInstance(this)

    // EN EL ONDESTROY AL FINALIZAR LA VISTA BORRAREMOS LA INSTANCIA Y CON ESTO LIBERAMOS LA LISTA DE INSTANCIAS 
    UpdateVersionApp.destroyInstance()

``kotlin
