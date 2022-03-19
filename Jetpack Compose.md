# Basic

## Text

### fontWeight

```kotlin
Text("Alfred Sisley", fontWeight = FontWeight.Bold)
```



### style

```kotlin
Text(text = "3 minutes ago", style = MaterialTheme.typography.body2)
```



### spacer

```kotlin
Spacer(modifier = Modifier.height(16.dp))
```



## Image

supposing to use [Coil] library to simplify the steps, which provided composables  and run these steps efficiently

```
//build.gradle
implementation 'io.coil-kt:coil-compose:2.0.0-rc01'
```



if you fetch a remote image, add the [INTERNET] permission 

```xml
<!-- AndroidManifest.xml -->
<uses-permission android:name="android.permission.INTERNET" />
```

```kotlin
@Composable
private fun ImageListItem(index: Int) {
    Row(verticalAlignment = Alignment.CenterVertically) {
        Image(
            painter =
            rememberAsyncImagePainter(
//            model = "https://developer.android.com/images/brand/Android_Robot.png"
                model = R.drawable.ic_baseline_android_24
            ),
            contentDescription = "Android Logo",
            modifier = Modifier.size(50.dp)
        )
        Spacer(Modifier.width(10.dp))
        Text(text = "Item #$index", style = MaterialTheme.typography.subtitle1)
    }
}
```







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



#### .background()

```kotin
.background(MaterialTheme.colors.surface)
```



#### .clickable()

```kotlin
.clickable(onClick = {/* Ignoring onClick*/ })
```



#### .clip()

```kotlin
.clip(RoundedCornerShape(4.dp))
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



## list

render all the items, even the ones that not visible on current screen

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

make it scrollabe

```kotlin
@Composable
private fun SimpleList() {
    //save the scroll postion with this state that can be used to programmatically scroll the list
    val scrollState = rememberScrollState()
    Column(
        modifier = Modifier.verticalScroll(scrollState)
    ) {
        repeat(100) {
            Text(text = "Item #$it")
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

### IconButton

```
implementation "androidx.compose.material:material-icons-extended:$compose_version"
```

```kotlin
IconButton(onClick = { expand.value = !expand.value }) {
    Icon(
        imageVector = if (expand.value) Icons.Filled.ExpandLess else Icons.Filled.ExpandMore,
        contentDescription = if (expand.value) {
            stringResource(id = R.string.show_less)
        } else {
            stringResource(id = R.string.show_more)
        }
    )

}
```

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

## lazy list

only compose and layout out the currently visible items

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



### Scroll to Top or Bottom

```kotlin
@Composable
private fun ScrollingList() {
    val listSize = 1000
    //we save the scroll position with this state
    val scrollState = rememberLazyListState()
    //we save the coroutine scope where our animated scroll will be executed
    val coroutineScope = rememberCoroutineScope()

    Column {
        Row {
            Button(
                onClick = {
                    coroutineScope.launch {
                        //0 is the first item index
                        scrollState.animateScrollToItem(0)
                    }
                },
                modifier = Modifier.weight(1f)
            ) {
                Text(text = "Scroll to Top")
            }

            Button(
                onClick = {
                    coroutineScope.launch {
                        //listSize -1 is the last index of the list
                        scrollState.animateScrollToItem(listSize - 1)
                    }
                },
                modifier = Modifier.weight(1f)
            ) {
                Text(text = "Scroll to Bottom")
            }
        }
        Spacer(modifier = Modifier.height(16.dp))
        LazyColumn(state = scrollState) {
            items(listSize) {
                ImageListItem(index = it)
            }
        }
    }
}
```



## animating  the list

### animateDpAsState

```kotlin
val extraPadding by animateDpAsState(targetValue =
    if (expand.value) 48.dp else 0.dp,
    animationSpec = spring(
        dampingRatio = Spring.DampingRatioMediumBouncy,
        stiffness = Spring.StiffnessLow
    )
)
```

```kotlin
modifier = Modifier
    .weight(1f)
    .padding(bottom = extraPadding.coerceAtLeast(0.dp))
```





## style and theme

### MaterialTheme

colors, typography, shapes

```kotlin
Text(text = name, style = MaterialTheme.typography.h4)
```

**copy**

```kotlin
Text(text = name, style = MaterialTheme.typography.h4.copy(
    fontWeight = FontWeight.ExtraBold
))
```





## preview the app

```kotlin
@Preview(
    showBackground = true,
    widthDp = 320,
    uiMode = UI_MODE_NIGHT_YES,
    name = "DefaultPreviewDark"
)
@Preview(showBackground = true, widthDp = 320)
@Composable
fun DefaultPreview() {
    BasicsCodelabTheme {
        Greetings()
    }
}
```





# Layout

## opacity

```kotlin
//LocalContentAlpha is defining opacity level of its children
CompositionLocalProvider(LocalContentAlpha provides ContentAlpha.medium) {
    Text(text = "3 minutes ago", style = MaterialTheme.typography.body2)
}
```





## Slot APIs

### scaffold

```kotlin
@Composable
fun LayoutsCodelab(){
    Scaffold {
        innerPadding->
            BodyContent(Modifier.padding(innerPadding))
    }
}

//make our code more reusable and testable, we should structure it into a small chunk
@Composable
private fun BodyContent(modifier: Modifier=Modifier){
    Column(modifier = modifier) {
        Text(text = "Hi, there!")
        Text(text = "Thanks for going througn the Layouts codelab")
    }
}
```



#### topBar

```kotlin
Scaffold(
    topBar = {
        TopAppBar(title = {
            Text(
                text = "LayoutsCodelab",
                style = MaterialTheme.typography.h4
            )
        })
    }
) { innerPadding ->
    BodyContent(Modifier.padding(innerPadding))
}
```



#### bottomBar

#### floatingActionButton



## customize layout

### layout modifier

```kotlin
//common structure
fun Modifier.firstBaselineToTop(
    firstBaselineToTOp:Dp
)=this.then(
    layout{
        measurable, constraints ->
        
    }
)
```



```kotlin
//create customer layout
private fun Modifier.firstBaselineToTop(
    firstBaselineToTOp: Dp
) = this.then(
    layout { measurable, constraints ->

        //Measure the composable
        val placeable = measurable.measure(constraints)

        //Check the composable has the first baseline
        check(placeable[FirstBaseline] != AlignmentLine.Unspecified)
        val firstBaseline = placeable[FirstBaseline]

        //Height of the composable with padding -first baseline
        val placeableY = firstBaselineToTOp.roundToPx() - firstBaseline
        val height = placeable.height + placeableY

        //now position the composable on the screen
        layout(placeable.width, height) {
            //Where the composable get placed
            placeable.placeRelative(0, placeableY)
        }
    }
)
```



### layout composable

```kotlin
//common structure of a composable that uses the [Layout]
@Composable
fun CustomLayout(
    modifier: Modifier = Modifier,
    //custom layout attrubutes
    content: @Composable () -> Unit
) {
    Layout(
        modifier = modifier,
        content = content,
    ) { measurables, constraints ->
        //measure and position children given constraints logic here
    }
}
```
