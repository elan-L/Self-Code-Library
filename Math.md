# Ceil()

adjusting a decimal number up or down to the closest integer value

```kotlin
if (roundUp) {
    tip = kotlin.math.ceil(tip)
}
```



# Format number to currency

```kotlin
val formattedTip = NumberFormat.getCurrencyInstance().format(tip)
```