# get a random nmber

```kotlin
class getRandomNumber(private val leftSides: Int,private val rightSides:Int) {
    fun roll(): Int {
        return (leftSides..rightSides).random()
    }
}
```