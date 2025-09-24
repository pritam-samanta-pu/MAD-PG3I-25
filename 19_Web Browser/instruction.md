```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/main"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <LinearLayout
        android:id="@+id/linearLayout"
        android:layout_width="match_parent"
        android:layout_height="200sp"
        android:gravity="center_horizontal|center_vertical"
        android:orientation="vertical"
        tools:layout_editor_absoluteX="1dp"
        tools:layout_editor_absoluteY="1dp"
        tools:ignore="MissingConstraints">

        <EditText
            android:id="@+id/url"
            android:layout_width="371dp"
            android:layout_height="wrap_content"
            android:layout_marginTop="30sp"
            android:ems="10"
            android:hint="Enter the url:"
            android:inputType="text" />

        <Button
            android:id="@+id/search"
            android:layout_width="262dp"
            android:layout_height="wrap_content"
            android:layout_marginTop="5sp"
            android:layout_marginBottom="7sp"
            android:text="Search" />

        <LinearLayout
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            android:orientation="horizontal">

            <Button
                android:id="@+id/back"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:layout_marginLeft="5sp"
                android:layout_marginRight="5sp"
                android:layout_weight="1"
                android:text="Back" />

            <Button
                android:id="@+id/forward"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:layout_marginLeft="5sp"
                android:layout_marginRight="5sp"
                android:layout_weight="1"
                android:text="Forward" />

            <Button
                android:id="@+id/clear"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:layout_marginLeft="5sp"
                android:layout_marginRight="5sp"
                android:layout_weight="1"
                android:text="Clear History" />
        </LinearLayout>

    </LinearLayout>

    <WebView
        android:id="@+id/web"
        android:layout_width="match_parent"
        android:layout_height="0dp"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintTop_toBottomOf="@+id/linearLayout"
        tools:layout_editor_absoluteX="-33dp" />
</androidx.constraintlayout.widget.ConstraintLayout>

```
