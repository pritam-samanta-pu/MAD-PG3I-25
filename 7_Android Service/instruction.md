# Android Service

## Declare Service in `AndroidManifest.xml`

```xml
<application>
    ...
    ...
    ...
    <service android:name=".MyService" />
</application>
```

## activity_main.xml

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:gravity="center"
    android:padding="24dp">

    <Button
        android:id="@+id/startBtn"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Start Service" />

    <Button
        android:id="@+id/stopBtn"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Stop Service"
        android:layout_marginTop="16dp"/>
</LinearLayout>
```

## MainActivity.java

```java
package com.example.mydemoapp;

import android.content.Intent;
import android.os.Bundle;
import android.widget.Button;

import androidx.appcompat.app.AppCompatActivity;

public class MainActivity extends AppCompatActivity {
    private Button startBtn, stopBtn;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        startBtn = findViewById(R.id.startBtn);
        stopBtn = findViewById(R.id.stopBtn);

        Intent intent = new Intent(this, MyService.class);

        startBtn.setOnClickListener(v -> startService(intent));
        stopBtn.setOnClickListener(v -> stopService(intent));
    }
}
```

## MyService.java

```java
package com.example.mydemoapp;

import android.app.Service;
import android.content.Intent;
import android.os.Handler;
import android.os.IBinder;
import android.widget.Toast;

public class MyService extends Service {
    Handler handler;
    Runnable runnable;
    private final int interval = 5000;

    @Override
    public void onCreate() {
        super.onCreate();
        handler = new Handler();
        runnable = new Runnable() {
            @Override
            public void run() {
                Toast.makeText(getApplicationContext(), "Service is running...", Toast.LENGTH_SHORT).show();
                handler.postDelayed(this, interval);
            }
        };
    }

    @Override
    public int onStartCommand(Intent intent, int flags, int startId) {
        handler.post(runnable);
        return START_STICKY;
    }

    @Override
    public void onDestroy() {
        super.onDestroy();
        handler.removeCallbacks(runnable);
        Toast.makeText(this, "Service stopped", Toast.LENGTH_SHORT).show();
    }

    @Override
    public IBinder onBind(Intent intent) {
        return null; // Not using bound service
    }
}
```

## Output

<img src="https://github.com/pritam-samanta-pu/MAD-PG3I-25/blob/main/out/Android%20Service.png" alt="Android Service" style="width:50%;">
