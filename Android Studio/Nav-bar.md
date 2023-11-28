----
La documentación que se ocupa aquí, es de [material.io](https://m3.material.io/components/navigation-bar/overview)

---
- Lo primero que necesitamos es un menú
- Para eso hacemos click derecho en **res** - **new** y seleccionamos Android Resource  Directory cuando aparezca la pestaña seleccionamos el **Resource** **type** y buscamos **menu** y la creamos
- Ahora creamos un archivo cliqueando en nuestra carpeta menu **new** - **Menu** **resource** **file** y nosotros le colocaremos **bottom_navigation_menu** de nombre
- Vamos a subir un icono para ello hacemos click derecho en drawable **new** - **Vector** **Asset** y elegimos nuestros iconos 

>[!nota]- Dato
>Solo se pueden colocar 5 ítems en el menu 

- Ahora en nuestro bottom_navigation_menu.xml vamos a crear los ítems
```kotlin
<?xml version="1.0" encoding="utf-8"?>  
<menu xmlns:android="http://schemas.android.com/apk/res/android">  
  
    <item        android:title="Inicio"  
        android:icon="@drawable/ic_inicio"  
        android:id="@+id/nav_inicio"/>  
    <item        android:title="datos"  
        android:icon="@drawable/ic_inicio"  
        android:id="@+id/nav_datos"/>  
    <item        android:title="configuracion"  
        android:icon="@drawable/ic_inicio"  
        android:id="@+id/nav_configuracion"/>  
</menu>
```

- Vamos a mostrar nuestro menu para ello vamos a nuestro **activity_main**.**xml** y colocamos el menu 

```kotlin
<?xml version="1.0" encoding="utf-8"?>  
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"  
    xmlns:app="http://schemas.android.com/apk/res-auto"  
    xmlns:tools="http://schemas.android.com/tools"  
    android:layout_width="match_parent"  
    android:layout_height="match_parent"  
    tools:context=".MainActivity"  
    android:layout_margin="30dp"  
    >  
    <com.google.android.material.bottomnavigation.BottomNavigationView        android:layout_width="match_parent"  
        android:layout_height="wrap_content"  
        app:layout_constraintBottom_toBottomOf="parent"  
        android:id="@+id/bottom_nav"  
        app:menu="@menu/bottom_navigation_menu"  
        />  
</androidx.constraintlayout.widget.ConstraintLayout>
```
- En nuestro activity main vamos a incluir un frame layout
```kotlin
<FrameLayout  
    android:layout_width="match_parent"  
    android:layout_height="match_parent"  
    app:layout_constrainBottom_toTopOf="@+id/bottom_nav"  
    android:id="@+id/frameLayout"  
    />
```
- Nos vamos a ir a java - **new** y  creamos un nuevo package en la primera carpeta y en la carpeta creada creamos un archivo fragment en blanco que se van a llamar como nuestro nav bar, ejemplo home, settings, etc...
- y ahora podemos editar nuestros fragments
- ahora vamos a agregar el view biding que está  en gradle scripts y build.grade.kts (Module:app) ya en el archivo escribimos 
```kotlin
buildFeatures{  
    viewBinding = true  
}
```
- En el Mainactivity.kt
```kotlin
package com.example.myapplication  
import androidx.appcompat.app.AppCompatActivity  
import android.os.Bundle  
import com.example.myapplication.databinding.ActivityMainBinding  
  
class MainActivity : AppCompatActivity() {  
    private lateinit var binding: ActivityMainBinding  
    override fun onCreate(savedInstanceState: Bundle?) {  
        super.onCreate(savedInstanceState)  
        binding = ActivityMainBinding.inflate(layoutInflater)  
        setContentView(binding.root)  
    }  
}
```
- Ahora vamos a relacionar la nav-bar con las id quedando asa 
```kotlin
        binding.bottomNav.setOnItemSelectedListener {  
            when(it.itemId){  
                R.id.nav_inicio -> {  
                    //Muestra el fragment de home  
                    supportFragmentManager.beginTransaction()  
                        .replace(R.id.frameLayout,HomeFragment()).commit()  
                    true  
                }  
                R.id.nav_datos -> {  
                    //Muestra el fragment de home  
                    supportFragmentManager.beginTransaction()  
                        .replace(R.id.frameLayout,DatosFragment()).commit()  
                    true  
                }  
                else -> false  
            }  
        }  
    }  
}
```
- Aquí  colocar nuestro fragment que se mostrara al iniciar la aplicación lo colocamos antes que se inicie el binding.bottomNav
```kotlin
if (savedInstanceState == null) {  
    supportFragmentManager.beginTransaction()  
        .replace(R.id.frameLayout, HomeFragment()).commit()  
}
```

>[!nota]- Pulido
>Si tenemos un formulario al cambiar de pestañas se borraran los datos y estará en blanco para ello ocupamos otras opciones aparte del binding.bottomNav.setOnItemReselectedListener

- Vamos a colocar una configuración que cuando clickemos nuevamente el mismo nav-bar  nos dirá que ya estamos en el fragment que seleccionó
```kotlin
binding.bottomNav.setOnItemReselectedListener {  
    when (it.itemId){  
        R.id.nav_inicio -> Toast.makeText(this,"ya estas en la vista home", Toast.LENGTH_LONG).show()  
        R.id.nav_datos -> Toast.makeText(this,"ya estas en la vista datos", Toast.LENGTH_LONG).show()  
    }  
}
```