# Textfield

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