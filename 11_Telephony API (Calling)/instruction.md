# Telephony API (Calling)

## Add permission in `AndroidManifest.xml`

```xml
. . .
<uses-feature android:name="android.hardware.telephony" android:required="false" />
<uses-permission android:name="android.permission.CALL_PHONE"/>
. . .
. . .
```

## `activity_main.xml`

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
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:gravity="center"
        android:orientation="vertical">

        <EditText
            android:id="@+id/et"
            android:layout_width="330dp"
            android:layout_height="wrap_content"
            android:ems="10"
            android:hint="Enter 10 digit number:"
            android:inputType="text" />

        <Button
            android:id="@+id/btn"
            android:layout_width="253dp"
            android:layout_height="wrap_content"
            android:text="Call" />
    </LinearLayout>
</androidx.constraintlayout.widget.ConstraintLayout>
```

## `MainActivity.java`

Here’s a clean and easy-to-understand version:

```java
package com.example.myphoneapp;

import android.Manifest;
import android.content.Intent;
import android.content.pm.PackageManager;
import android.net.Uri;
import android.os.Bundle;
import android.widget.Button;
import android.widget.EditText;
import android.widget.Toast;

import androidx.activity.EdgeToEdge;
import androidx.annotation.NonNull;
import androidx.appcompat.app.AppCompatActivity;
import androidx.core.app.ActivityCompat;
import androidx.core.content.ContextCompat;
import androidx.core.graphics.Insets;
import androidx.core.view.ViewCompat;
import androidx.core.view.WindowInsetsCompat;

public class MainActivity extends AppCompatActivity {
    EditText et;
    Button btn;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        EdgeToEdge.enable(this);
        setContentView(R.layout.activity_main);

        et=findViewById(R.id.et);
        btn=findViewById(R.id.btn);
        if(ContextCompat.checkSelfPermission(this, Manifest.permission.CALL_PHONE)!= PackageManager.PERMISSION_GRANTED){
            ActivityCompat.requestPermissions(this,new String[]{Manifest.permission.CALL_PHONE},100);
        }
        btn.setOnClickListener(v->{
            String s=et.getText().toString().trim();
            if(s.isEmpty()){
                Toast.makeText(this, "Number is required.", Toast.LENGTH_SHORT).show();
                return;
            }
            Intent intent=new Intent(Intent.ACTION_CALL);
            intent.setData(Uri.parse("tel:"+s));
            startActivity(intent);
        });
    }

}
```
