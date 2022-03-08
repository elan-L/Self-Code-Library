# ImageView



## scaleType


Scale the image uniformly (maintain the image's aspect ratio) so that both dimensions (width and height) of the image will be equal to or larger than the corresponding dimension of the view (minus padding). The image is then centered in the view.

It makes the image fill the screen without distorting it.

https://developer.android.google.cn/reference/kotlin/android/widget/ImageView.ScaleType

```xml
android:scaleType="centerCrop"
```



# EditText



```xml
<EditText
    android:id="@+id/input_phone_number"
    android:layout_width="250dp"
    android:layout_height="wrap_content"
    android:layout_marginTop="40dp"
    android:ems="10"
    android:hint="Please input your number"
    android:inputType="number"
    android:maxLength="11"
    android:minHeight="48dp"
    app:layout_constraintEnd_toEndOf="parent"
    app:layout_constraintStart_toStartOf="parent"
    app:layout_constraintTop_toBottomOf="@+id/imageView" />
```

# Radio Group
##  checkedButton

It would be nice to select one of the radio button options by default.

```xml
android:checkedButton="@id/option_eighteen_percent"
```



# Switch

## checked

```xml
android:checked="true"
```



# Recycler view

There are a number of pieces involved in creating and using a recycler view

- **item** - One data item of the list to display. Represents one `Affirmation` object in your app.
- **Adapter** - Takes data and prepares it for `RecyclerView` to display.
- **ViewHolders** - A pool of views for `RecyclerView` to use and reuse to display affirmations.
- **RecyclerView** - Views on screen

## 1 add a recycler view to the layout

```xml
<androidx.recyclerview.widget.RecyclerView
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:id="@+id/recycler_view"
    android:scrollbars="vertical"
    app:layoutManager="androidx.recyclerview.widget.LinearLayoutManager"/>
```

## 2 implement an adapter for the recycler view

### create the adapter

#### create a layout for items

In **res > layout**, create a new empty **File** called `list_item.xml`

update the code like that shown below

```xml
<?xml version="1.0" encoding="utf-8"?>
<TextView xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/item_title"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content" />
```



#### create an itemAdapter class

1. In Android Studio in the **Project** pane, right-click **app > java > com.example.affirmations** and select **New > Package**.
2. Enter `adapter` as the last part of the package name.
3. Right-click on the `adapter` package and select **New > Kotlin File/Class.**
4. Enter `ItemAdapter` as the class name, finish, and the `ItemAdapter.kt` file opens.

```xml
import android.content.Context
import com.example.work.model.Affirmation

class ItemAdapter(private val context: Context, private val dataset: List<Affirmation>) {
}
```



### create a viewHolder

interact with item views

```kotlin
class ItemAdapter(private val context: Context, private val dataset: List<Affirmation>) {
    /*
    Since ItemViewHolder is only used by ItemAdapter, creating it inside ItemAdapter shows
     this relationship. This is not mandatory, but it helps other developers understand
     the structure of your program.
     */
    class ItemViewHolder(private val view: View) : RecyclerView.ViewHolder(view) {
        //        Assign it the view with the ID item_title that you defined in list_item.xml.
        val textView: TextView = view.findViewById(R.id.item_title)
    }
}
```



### Override adapter methods

```kotlin
/*
Add the code to extend your ItemAdapter from the abstract class RecyclerView.Adapter.
Specify ItemAdapter.ItemViewHolder as the view holder type in angle brackets.
 */
/**
 * Adapter for the [RecyclerView] in [MainActivity]. Displays [Affirmation] data object.
 */
class ItemAdapter(
    private val context: Context,
    private val dataset: List<Affirmation>
) : RecyclerView.Adapter<ItemAdapter.ItemViewHolder>() {
    /*
    Since ItemViewHolder is only used by ItemAdapter, creating it inside ItemAdapter shows
     this relationship. This is not mandatory, but it helps other developers understand
     the structure of your program.
     */
    // Provide a reference to the views for each data item
    // Complex data items may need more than one view per item, and
    // you provide access to all the views for a data item in a view holder.
    // Each data item is just an Affirmation object.
    class ItemViewHolder(private val view: View) : RecyclerView.ViewHolder(view) {
        //        Assign it the view with the ID item_title that you defined in list_item.xml.
        val textView: TextView = view.findViewById(R.id.item_title)
    }

    /**
     * Create new views (invoked by the layout manager)
     */
    override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): ItemViewHolder {
        // create a new view
        val adapterLayout = LayoutInflater.from(parent.context)
            .inflate(R.layout.list_item, parent, false)

        return ItemViewHolder(adapterLayout)
    }

    /**
     * Replace the contents of a view (invoked by the layout manager)
     */
    override fun onBindViewHolder(holder: ItemViewHolder, position: Int) {
        val item = dataset[position]
        holder.textView.text = context.resources.getString(item.stringResourceId)
    }

    /**
     * Return the size of your dataset (invoked by the layout manager)
     */

    override fun getItemCount(): Int {
        return dataset.size

    }
}
```



## 3  Modify the MainActivity to use a RecyclerView

```kotlin
class MainActivity : AppCompatActivity() {

    lateinit var binding: ActivityMainBinding

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        binding = ActivityMainBinding.inflate(layoutInflater)
        setContentView(binding.root)

        // Initialize data.
        val myDataset = DataSource().loadAffirmations()
        val recyclerView = binding.recyclerView

        recyclerView.adapter = ItemAdapter(this, myDataset)

        // Use this setting to improve performance if you know that changes
        // in content do not change the layout size of the RecyclerView
        recyclerView.setHasFixedSize(true)

    }
}
```
