**Adjust the scaleType of the ImageView, which says how to size and align the images**

Scale the image uniformly (maintain the image's aspect ratio) so that both dimensions (width and height) of the image will be equal to or larger than the corresponding dimension of the view (minus padding). The image is then centered in the view. From XML, use this syntax: `android:scaleType="centerCrop"`.

https://developer.android.google.cn/reference/kotlin/android/widget/ImageView.ScaleType

```xml
android:scaleType="centerCrop"
```