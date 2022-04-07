# View binding

learn to use viewBinding instead of using findViewById

## Enable view binding



1. open the app's    (**Gradle Scripts->build.gradle(Medule:work.app)**)

2. in the **android** selection, add the follow lines:

   ```kotlin
   buildFeatures {
       viewBinding true
   }
   ```

   

## initialize the binding object

1. 

   ```kotlin
   //original
   class MainActivity : AppCompatActivity() {
       override fun onCreate(savedInstanceState: Bundle?) {
           super.onCreate(savedInstanceState)
           setContentView(R.layout.activity_main)
       }
   }
   ```

   replace it with the code below:

   ```kotlin
    class MainActivity : AppCompatActivity() {
   
        lateinit var binding: ActivityMainBinding
   
        override fun onCreate(savedInstanceState: Bundle?) {
            super.onCreate(savedInstanceState)
            binding = ActivityMainBinding.inflate(layoutInflater)
            setContentView(binding.root)
        }
    }
   ```


##　 ex

```kotlin
// Old way with findViewById()
val myButton: Button = findViewById(R.id.my_button)
myButton.text = "A button"

// Better way with view binding
val myButton: Button = binding.myButton
myButton.text = "A button"

// Best way with view binding and no extra variable
binding.myButton.text = "A button"
```



## ignore ViewBinding

> Now, there might be a case where you wan to ignore the view binding feature so you can go ahead and add the following code ```tools:viewBindingIgnore=true```  to you XML files



## differences from findViewById

> 1. null safety
> 2. Type safety; no class cast exception

# Hide keyboard when press Enter

It's a bit of cumbersome to hide the keyboard manually sometime, we hope that the keyboard automatically hide itself after the Enter key is pressed

define a key listener to respond the event when certain keys are tapped



1. create a function

```kotlin
private fun handleKeyEvent(view: View, keyCode: Int): Boolean {
   if (keyCode == KeyEvent.KEYCODE_ENTER) {
       // Hide the keyboard
       val inputMethodManager =
           getSystemService(Context.INPUT_METHOD_SERVICE) as InputMethodManager
       inputMethodManager.hideSoftInputFromWindow(view.windowToken, 0)
       return true
   }
   return false
}
```



2. inside the MainAcitivity.kt ->oncreate()

   // the **costOfServiceEditText** is the id of the **editText**

   ```kotlin
   binding.costOfServiceEditText.setOnKeyListener { view, keyCode, _ -> handleKeyEvent(view, keyCode)
      }
   ```



# intent to the call pane

firstly, you can add permission in the Manifest 

```xml
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.elan.androidpermission">
    <uses-permission android:name="android.permission.CALL_PHONE"/>
    .....
```



```kotlin
binding.btnCallPhone.setOnClickListener {
    /*val telephoneNumber= binding.edtPhoneNumber.text.takeUnless {
        it.isEmpty()
    }
        ?.toString()
        ?:"10086"
    callPhone(telephoneNumber)*/
}
```

```kotlin
private fun callPhone(telephoneNumber: String) {


        //check out the permission of dial phone
        if (ActivityCompat.checkSelfPermission(
                this,
                Manifest.permission.CALL_PHONE
            ) != PackageManager.PERMISSION_GRANTED
        ) {
            //without permission, request it now
            ActivityCompat.requestPermissions(
                this, arrayOf(Manifest.permission.CALL_PHONE),
                CALL_PHONE_REQUEST_CODE
            )
            return
        } else {
            //permission granted
            Intent(Intent.ACTION_CALL).setData(
                Uri.parse("tel:$telephoneNumber")
            ).apply {
                startActivity(this)
            }
        }
    }
```



out scope of the Main class, create a variable and assign value 1

```kotlin
val CALL_PHONE_REQUEST_CODE = 1
```

override the method

```kotlin
override fun onRequestPermissionsResult(
    requestCode: Int,
    permissions: Array<out String>,
    grantResults: IntArray
) {
    super.onRequestPermissionsResult(requestCode, permissions, grantResults)

    when (requestCode) {
        CALL_PHONE_REQUEST_CODE -> {
            if (grantResults[0] == PackageManager.PERMISSION_GRANTED) {
                //permission to make call is granted
                callPhone("10086")
            } else {
                Toast.makeText(
                    this, "You have rejected the app's requirement.", Toast.LENGTH_SHORT
                ).show()
            }
        }
    }
}
```

# add view

```kotlin
btnAdd.setOnClickListener {
            counter++

//            从xml文件中加载并instance一个TextView view
            val textView=layoutInflater.inflate(R.layout.my_textview,null) as TextView

            textView.text="new view $counter"

            textView.setOnClickListener{
                Toast.makeText(this,"${(it as TextView).text}",Toast.LENGTH_SHORT)
                    .show()
            }

//            add to the viewContainer
            viewContainer.addView(textView)

        }

        val btnRemoverView:Button=findViewById(R.id.btnRemoveAllView)
        btnRemoverView.setOnClickListener {
            viewContainer.removeAllViews()
        }
```





# invoke music player

```kotlin
class MainActivity : AppCompatActivity() {

    var mediaPlayer:MediaPlayer?=null

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

       val  btnPlay=findViewById<Button>(R.id.btnPlay)
       val  btnStop=findViewById<Button>(R.id.btnStop)

        btnPlay.setOnClickListener {
            mediaPlayer=MediaPlayer.create(this,R.raw.jay_give_me_the_time_of_a_song)
            mediaPlayer!!.start()
        }

        btnStop.setOnClickListener {
            mediaPlayer?.stop()
        }
    }
}
```



# Save instance state



```kotlin
class MainActivity : AppCompatActivity() {

    private var counter=0
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        val btnClickMe:Button=findViewById(R.id.btnClickMe)
        val tvCount:TextView=findViewById(R.id.tvCount)

        btnClickMe.setOnClickListener {
            counter++
            tvCount.text="Counter number: $counter"
        }

        //If we rotate the screen, the value of counter will lost, but the string remain
        //solution:

        //2. recover the instance data
        if(savedInstanceState!=null){
            counter=savedInstanceState.getInt("counter")
            tvCount.text="Counter number: $counter"
        }


    }

    /*
    1. 计数器属性的当前值保存到Bundle中，此对象由Android负责创建和维护
     */
    override fun onSaveInstanceState(outState: Bundle) {
        super.onSaveInstanceState(outState)
        //Save instance data
        outState.putInt("counter",counter)
        Log.d("MainActivity","counter data saved: $counter")
    }
}
```

# Static variables

```kotlin
class MainActivity : AppCompatActivity() {

    //Static variables
    companion object{
        var globalCount:Int= 0
    }

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        val tvInfo:TextView=findViewById(R.id.tvInfo)
        tvInfo.text="Global Counter: $globalCount"
    }
}
```



# Intent another activity

typical code

```kotlin
val btnShowOther=findViewById<Button>(R.id.btnShowOther)

btnShowOther.setOnClickListener {
    val intent=Intent(this,OtherActivity::class.java)
    startActivity(intent)
}
```



# Try read bit map

```kotlin
//The function read a specifier file, and decoding it into a bit object
private fun tryReadBitmap(data: Uri): Bitmap? {
    return try {
        //for Android P, this method is recommend
        if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.P) {
            ImageDecoder.createSource(contentResolver, data).let {
                ImageDecoder.decodeBitmap(it)
            }
        } else {
            //For earlier version, using the traditional ways
            MediaStore.Images.Media.getBitmap(contentResolver, data)
        }
    } catch (e: Exception) {
        e.printStackTrace()
        null
    }
}
```

```kotlin
if (it.resultCode == Activity.RESULT_OK) {
    tryReadBitmap(it.data?.data as Uri)?.apply {
        binding.imageView.setImageBitmap(this)
    }
}
```



# Up navigation

1. in AndroidManifest.xml

```xml
<activity
    android:name=".FourthActivity"
    android:exported="false"
    android:parentActivityName=".ThirdActivity"/>
```



2. manually

```kotlin
override fun onOptionsItemSelected(item: MenuItem): Boolean {
    //Up button's id is home
    if (item.itemId==android.R.id.home){
        //Usually, the function of up and back is similar
        //therefore, you should finish itself here
        finish()
    }

    return super.onOptionsItemSelected(item)
}
```

in onCreate()

```kotlin
//The statement will show on ActionBar in any situation
supportActionBar?.setDisplayHomeAsUpEnabled(true)

//you could replace the icon of narrow
supportActionBar?.setHomeAsUpIndicator(R.drawable.ic_baseline_arrow_back_24)
```







# App permissions



projection   **Multiple Permission**



## check whether a permission is granted or not

```kotlin
//permission of access location background
private fun hasBackgroundLocationPermission()=
    ActivityCompat.checkSelfPermission(
        this,
        Manifest.permission.ACCESS_BACKGROUND_LOCATION
    )==PackageManager.PERMISSION_GRANTED


//Access camera
private fun hasCameraPermission()=
    ActivityCompat.checkSelfPermission(
        this,
        Manifest.permission.CAMERA
    )==PackageManager.PERMISSION_GRANTED
```



## request a permission

```kotlin
//request special permission
private fun requestPermissions(){
    //get list of permissions not granted
    val permissionToRequest= mutableListOf<String>()

    if(!hasBackgroundLocationPermission()){
        permissionToRequest.add(Manifest.permission.ACCESS_BACKGROUND_LOCATION)
    }

    if (!hasCallPhonePermission()){
        permissionToRequest.add(Manifest.permission.CALL_PHONE)
    }

    if (!hasCameraPermission()){
        permissionToRequest.add(Manifest.permission.CAMERA)
    }

    //if there are permissions not granted
    if(permissionToRequest.isNotEmpty()){
        //open the permission request interface, asking for the grantee
        permissionLauncher.launch(permissionToRequest.toTypedArray())
    }
}
```



# Different resources for different device

![image-20220313231551524](C:\Users\Admin\AppData\Roaming\Typora\typora-user-images\image-20220313231551524.png)



projection: Load Different Resource





# show message



## Toast



### show a brief message when a view is tapped

```kotlin
class MainActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
        
        val rollButton: Button = findViewById(R.id.button)//it's an indicator
        //what going to do when a tap or click happened
        rollButton.setOnClickListener {
            val toast=Toast.makeText(this,"Dice Rolled",Toast.LENGTH_SHORT)
            toast.show()
        }
    }
```



## AlertDialog



```kotlin
button.setOnClickListener {
    AlertDialog.Builder(this)
        .setMessage("This is an interact message box")
        .setPositiveButton("OK"){_,which ->
            textView.text="ok!"
        }
        .setNegativeButton("Cancel"){_,which ->
        textView.text="Cancled!"
        }
        .create()
        .show()
}
```





## show message when long pressed

```kotlin
button.setOnLongClickListener {
    Toast.makeText(this,"You've long press it",Toast.LENGTH_SHORT)
        .show()
    //return value
    true
}
```
