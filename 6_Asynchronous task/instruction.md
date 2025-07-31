# Asynchronous task

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

    <Button
        android:id="@+id/btn"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:background="#24A62A"
        android:text="Run"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.5"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintVertical_bias="0.5" />

    <ProgressBar
        android:id="@+id/pb"
        style="?android:attr/progressBarStyleHorizontal"
        android:layout_width="311dp"
        android:layout_height="33dp"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintVertical_bias="0.328" />

    <TextView
        android:id="@+id/per"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="18%"
        app:layout_constraintBottom_toTopOf="@+id/btn"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.498"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toBottomOf="@+id/pb"
        app:layout_constraintVertical_bias="0.311" />

</androidx.constraintlayout.widget.ConstraintLayout>
```

## MainActivity.java

```java
package com.example.mydemoapp;

import android.content.Intent;
import android.content.SharedPreferences;
import android.graphics.Color;
import android.os.Bundle;
import android.os.Handler;
import android.view.Menu;
import android.view.MenuItem;
import android.view.View;
import android.widget.Button;
import android.widget.EditText;
import android.widget.ProgressBar;
import android.widget.TextView;
import android.widget.Toast;

import androidx.activity.EdgeToEdge;
import androidx.annotation.NonNull;
import androidx.appcompat.app.AppCompatActivity;
import androidx.constraintlayout.widget.ConstraintLayout;
import androidx.core.graphics.Insets;
import androidx.core.view.ViewCompat;
import androidx.core.view.WindowInsetsCompat;

import java.io.FileNotFoundException;
import java.io.FileOutputStream;
import java.io.IOException;
import java.util.Random;

public class MainActivity extends AppCompatActivity {
    TextView per;
    ProgressBar pb;
    Button btn;
    int count=0;
    Handler uiHandler=new Handler();
    Thread progressThread;
    Random random=new Random();
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        EdgeToEdge.enable(this);
        setContentView(R.layout.activity_main);

        per=findViewById(R.id.per);
        pb=findViewById(R.id.pb);
        btn=findViewById(R.id.btn);
        pb.setVisibility(View.GONE);
        per.setVisibility(View.GONE);

        btn.setOnClickListener(v->{
            btn.setEnabled(false);
            count=0;
            pb.setProgress(0);
            pb.setVisibility(View.VISIBLE);
            per.setVisibility(View.VISIBLE);
            per.setText("0%");

            progressThread=new Thread(()->{
                while(count<100){
                    try{
                        Thread.sleep(290);
                        int inc=1+ random.nextInt(10);
                        count+=inc;
                        if(count>100) count=100;
                        uiHandler.post(()->{
                            pb.setProgress(count);
                            per.setText(String.valueOf(count)+"%");
                        });
                    }catch (InterruptedException e){
                        Thread.currentThread().interrupt();
                    }
                }
                uiHandler.post(()->{
                    pb.setVisibility(View.GONE);
                    per.setVisibility(View.GONE);
                    btn.setEnabled(true);
                    Toast.makeText(this, "Work Done.", Toast.LENGTH_SHORT).show();
                });
            });
            progressThread.start();
        });
    }


}
```

## Output

<img src="https://github.com/pritam-samanta-pu/MAD-PG3I-25/blob/main/out/async.png" alt="Asynchronous task" style="width:50%;">
