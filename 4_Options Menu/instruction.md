# Options Menu

## activity_main.xml

```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/main"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Hello World!"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent" />

</androidx.constraintlayout.widget.ConstraintLayout>
```

## color_menu.xml

```xml
<?xml version="1.0" encoding="utf-8"?>
<menu xmlns:android="http://schemas.android.com/apk/res/android">

    <item
        android:id="@+id/cRed"
        android:title="Red" />
    <item
        android:id="@+id/cGreen"
        android:title="Green" />
    <item
        android:id="@+id/cBlue"
        android:title="Blue" />
    <item
        android:id="@+id/cDefault"
        android:title="System Default" />
</menu>
```

## MainActivity.java

```java
package com.example.mydemoapp; // Write Your Project name in smallcase

import android.graphics.Color;
import android.os.Bundle;
import android.view.Menu;
import android.view.MenuItem;

import androidx.activity.EdgeToEdge;
import androidx.annotation.NonNull;
import androidx.appcompat.app.AppCompatActivity;
import androidx.constraintlayout.widget.ConstraintLayout;
import androidx.core.graphics.Insets;
import androidx.core.view.ViewCompat;
import androidx.core.view.WindowInsetsCompat;

public class MainActivity extends AppCompatActivity {
    ConstraintLayout main;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        EdgeToEdge.enable(this);
        setContentView(R.layout.activity_main);
        main=findViewById(R.id.main);
    }

    @Override
    public boolean onCreateOptionsMenu(Menu menu) {
        getMenuInflater().inflate(R.menu.color_menu,menu);
        return true;
    }

    @Override
    public boolean onOptionsItemSelected(@NonNull MenuItem item) {
        int itemId=item.getItemId();
        if(itemId==R.id.cRed){
            main.setBackgroundColor(Color.RED);
            return true;
        }else if(itemId==R.id.cGreen){
            main.setBackgroundColor(Color.GREEN);
            return true;
        }else if(itemId==R.id.cBlue){
            main.setBackgroundColor(Color.BLUE);
            return true;
        }else if(itemId==R.id.cDefault){
            main.setBackgroundColor(Color.TRANSPARENT);
            return true;
        }
        return super.onOptionsItemSelected(item);
    }
}
```

## Output

```markdown
![Options Menu Output](https://drive.google.com/file/d/1_wfFdpRrKAV8V0MKkscf-SdVNDWd6sc4/view?usp=sharing)
```
