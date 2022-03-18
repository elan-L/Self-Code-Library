# Text field

```xml
<com.google.android.material.textfield.TextInputLayout
    android:id="@+id/cost_of_service"
    android:layout_width="160dp"
    android:layout_height="wrap_content"
    android:hint="@string/cost_of_service"
    app:layout_constraintStart_toEndOf="@id/icon_cost_of_service"
    android:layout_marginStart="16dp"
    app:layout_constraintTop_toTopOf="parent">

    <com.google.android.material.textfield.TextInputEditText
        android:id="@+id/cost_of_service_edit_text"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:inputType="numberDecimal" />
</com.google.android.material.textfield.TextInputLayout>
```



# Card view

A Card view provides an easy way to contain a group of views while providing a consistent style for the container.

```xml
<?xml version="1.0" encoding="utf-8"?>
<com.google.android.material.card.MaterialCardView xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:layout_margin="8dp">

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:orientation="vertical"
        android:padding="15dp">

        <ImageView
            android:id="@+id/item_image"
            android:layout_width="match_parent"
            android:layout_height="200dp"
            android:importantForAccessibility="no"
            android:scaleType="centerCrop" />

        <TextView
            android:id="@+id/item_title"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:textAppearance="?attr/textAppearanceHeadline6" />

    </LinearLayout>
    
</com.google.android.material.card.MaterialCardView>
```



# Material icons

dependencies

```
 implementation "androidx.compose.material:material-icons-extended:$compose_version"
```

import

```ko
import ui-material-icons-extended
```

