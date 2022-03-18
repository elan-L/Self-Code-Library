# Basic

## tweaking the UI

### Surface

```kotlin
@Composable
fun Greeting(name: String) {
    Surface(color = MaterialTheme.colors.secondary){
        Text(text = "Hello $name!")
    }
}
```



### Modifier

#### .padding()

```kotlin
Text(
    text = "Hello $name!",
    modifier = Modifier.padding(24.dp)
)
```

```kotlin
modifier = Modifier.padding(vertical = 4.dp, horizontal = 8.dp)
```



#### .fillMaxWidth()

to add multiple modifiers to an element, you simply chain them

```kotlin
Column(modifier = Modifier.fillMaxWidth().padding(24.dp)) {
            Text(text = "Hello, ")
            Text(text = name)
        }
```

## reusing  composables

```kotlin
@Composable
private fun MyApp() {
    Surface(color = MaterialTheme.colors.background) {
        Greeting(name = "Android")
    }
}
```



## creating columns and rows

### Column

```kotlin
@Composable
private fun Greeting(name: String) {
    Surface(color = MaterialTheme.colors.primary) {
        Column(modifier = Modifier.padding(24.dp)) {
            Text(text = "Hello,")
            Text(text = name)
        }
    }
}
```



### Row



### Box



## add Button

#### Button

#### OutlineButton

```kotlin
Row(modifier = Modifier.padding(24.dp)) {
            Column(modifier = Modifier.weight(1f)) {
                Text(text = "Hello, ")
                Text(text = name)
            }
            OutlinedButton(
                onClick = { /* TODO */ }
            ) {
                Text("Show more")
            }
        }
```

#### TextlineButton

## save state

#### remember

```kotlin
//remember is used to guard against recomposition, so the state wil not reset
val expand=remember{ mutableStateOf(false)}
```

```kotlin
var shouldShowOnboarding by remember {
    mutableStateOf(true)
}
```



#### rememberSaveable



```kotlin
var shouldShowOnboarding by rememberSaveable{
    mutableStateOf(true)
}
```

## scrollable lazy list

### LazyColumn

```kotlin
LazyColumn(
    modifier = Modifier.padding(vertical = 4.dp)
) {
    items(items=names){name->
        Greeting(name)
    }
}
```



### LazyRow

