# show a brief message when a view is tapped

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