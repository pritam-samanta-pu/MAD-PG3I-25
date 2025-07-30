## `activity_main.xml`

```xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/root"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:padding="16dp"
    tools:context=".MainActivity">

    <TextView
        android:id="@+id/tv"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_centerInParent="true"
        android:text="0"
        android:textSize="80sp"
        android:textStyle="bold" />

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_alignParentBottom="true"
        android:gravity="center"
        android:orientation="horizontal">

        <Button
            android:id="@+id/startBtn"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_marginEnd="16dp"
            android:text="Start" />

        <Button
            android:id="@+id/stopBtn"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="Stop" />
    </LinearLayout>

</RelativeLayout>

```

## `MainActivity.java`

```java
package com.example.mydemoapp; //Your App name

import android.os.Bundle;
import android.os.Handler;
import android.widget.Button;
import android.widget.TextView;

import androidx.activity.EdgeToEdge;
import androidx.appcompat.app.AppCompatActivity;
import androidx.core.graphics.Insets;
import androidx.core.view.ViewCompat;
import androidx.core.view.WindowInsetsCompat;

public class MainActivity extends AppCompatActivity {
    TextView tv;
    Button startBtn,stopBtn;
    int count=0;
    Thread counterThread;
    Handler uiHandler=new Handler();
    volatile boolean isRunning=false;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        EdgeToEdge.enable(this);
        setContentView(R.layout.activity_main);

        tv=findViewById(R.id.tv);
        startBtn=findViewById(R.id.startBtn);
        stopBtn=findViewById(R.id.stopBtn);
        stopBtn.setEnabled(false);
        startBtn.setEnabled(true);
        startBtn.setOnClickListener(v->startCounter());
        stopBtn.setOnClickListener(v->stopCounter());
    }
    public void startCounter(){
        if(isRunning)  return;
        isRunning=true;
        startBtn.setEnabled(false);
        stopBtn.setEnabled(true);
        counterThread=new Thread(()->{
            while (isRunning){
                try{
                    Thread.sleep(1000);
                    count++;
                    if(isRunning){
                        uiHandler.post(()->{
                            tv.setText(String.valueOf(count));
                        });
                    }
                }catch (InterruptedException e){
                    Thread.currentThread().interrupt();
                }
            }
        });
        counterThread.start();
    }
    public void stopCounter(){
        isRunning=false;
        stopBtn.setEnabled(false);
        startBtn.setEnabled(true);
    }

    @Override
    protected void onDestroy() {
        super.onDestroy();
        if(isRunning){
            stopCounter();
        }
    }
}
```
