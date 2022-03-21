# architecture

## View model

a class [MyNewModel] extends from ViewModel()

```kotlin
class MyNewModel: ViewModel() {
    var number:Int=0
}
```



create a variable and initialize it

```kotlin
var myNewModel: MyNewModel= MyNewModel()
```



in the onCreate()

```kotlin
myNewModel =ViewModelProvider(this).get(myNewModel::class.java)
```



get the data

```kotlin
//activity restart, get data from view model
textView=findViewById<TextView?>(R.id.textView).apply {
    this.text=myNewModel.number.toString()
}
```