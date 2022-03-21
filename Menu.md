# options menu

1. create **../res/menu/mymenu.xml**

```xml
<?xml version="1.0" encoding="utf-8"?>
<menu xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto">

    <item
        android:id="@+id/menu_file"
        android:title="File">
        <menu>
            <item
                android:id="@+id/menu_new"
                android:title="New" />
            <item
                android:id="@+id/menu_open"
                android:title="Open" />
        </menu>
    </item>
    <item
        android:id="@+id/menu_modify"
        android:title="Modify" />
    <item
        android:id="@+id/menu_save"
        android:title="Save" />
    <item
        android:id="@+id/menu_exit"
        android:title="Exit" />
    <item
        android:id="@+id/menu_about"
        android:title="About" />
</menu>
```



2. int [MainActivity.kt]

```kotlin
override fun onCreateOptionsMenu(menu: Menu?): Boolean {
    super.onCreateOptionsMenu(menu)
    setIconEnable(menu!!, true)
    menuInflater.inflate(R.menu.mymenu, menu)
    return true

}

private fun setIconEnable(menu: Menu, enable: Boolean) {
    try {
        val clazz =
            Class.forName("andriodx.appcompat.view.menu.MenuBuilder")
        val m: Method = clazz.getDeclaredMethod(
            "setOptionalIconsVisible",
            Boolean::class.javaPrimitiveType
        )
        m.isAccessible = true

        m.invoke(menu, enable)

    } catch (e: Exception) {
        e.printStackTrace()
    }
}

override fun onOptionsItemSelected(item: MenuItem): Boolean {
    super.onOptionsItemSelected(item)

    val textView = findViewById<TextView>(R.id.log_text)

    when (item.itemId) {
        R.id.menu_exit -> textView.text = "Exit"
        R.id.menu_file -> textView.text = "File"
        R.id.menu_new -> textView.text = "New"
        R.id.menu_open -> textView.text = "Open"
        R.id.menu_modify -> textView.text = "Modify"
        R.id.menu_about -> textView.text = "About"
    }
    return true
}
```





# content menu

1. create **../res/menu/mymenu.xml**

```xml
<?xml version="1.0" encoding="utf-8"?>
<menu xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto">

    <item
        android:id="@+id/menu_file"
        android:title="File">
        <menu>
            <item
                android:id="@+id/menu_new"
                android:title="New" />
            <item
                android:id="@+id/menu_open"
                android:title="Open" />
        </menu>
    </item>
    <item
        android:id="@+id/menu_modify"
        android:title="Modify" />
    <item
        android:id="@+id/menu_save"
        android:title="Save" />
    <item
        android:id="@+id/menu_exit"
        android:title="Exit" />
    <item
        android:id="@+id/menu_about"
        android:title="About" />
</menu>
```



2. in **onCreate()** method

```kotlin
registerForContextMenu(button)
```

```kotlin
override fun onCreateContextMenu(
    menu: ContextMenu?,
    v: View?,
    menuInfo: ContextMenu.ContextMenuInfo?
) {
    super.onCreateContextMenu(menu, v, menuInfo)
    menuInflater.inflate(R.menu.mymenu,menu)
}
```

responding the tap event

```kotlin
override fun onContextItemSelected(item: MenuItem): Boolean {
    val textView: TextView = findViewById(R.id.log_text)
    val info = when (item.itemId) {
        R.id.menu_about ->
            "You choose the 'About'."
        else -> "Please try again."
    }
    textView.text = info
    return true
}
```
