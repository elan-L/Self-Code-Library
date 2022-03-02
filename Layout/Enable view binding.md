# view binding

learn to use viewBinding instead of using findViewById

## Enable view binding



1. open the app's    (**Gradle Scripts->build.gradle(Medule:work.app)**)

2. in the **android** selection, add the follow lines:

   ```kotlin
   buildFeatures {
       viewBinding true
   }
   ```

3. Note the message **Gradle files have changed since last project sync**
   ![image-20220302162140933](C:\Users\Admin\AppData\Roaming\Typora\typora-user-images\image-20220302162140933.png)

4. press **sync now**

   

## initialize the binding object

1. open **MainActivity.kt (app > java > com.example.work > MainActivity)**

2. Replace all of the existing code for `MainActivity` class with this code to setup the `MainActivity` to use view binding:

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
