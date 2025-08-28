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

    <EditText
        android:id="@+id/lInp"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:ems="10"
        android:hint="Enter Lower Bound:"
        android:inputType="text"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.497"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintVertical_bias="0.309" />

    <EditText
        android:id="@+id/uInp"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:ems="10"
        android:hint="Enter Upper Bound:"
        android:inputType="text"
        app:layout_constraintBottom_toTopOf="@+id/btn"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.497"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toBottomOf="@+id/lInp"
        app:layout_constraintVertical_bias="0.347" />

    <Button
        android:id="@+id/btn"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Submit"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintVertical_bias="0.547" />
</androidx.constraintlayout.widget.ConstraintLayout>

```

## MainActivity.java

```java
package com.example.myprimeapp;

import android.Manifest;
import android.content.Intent;
import android.content.pm.PackageManager;
import android.os.Build;
import android.os.Bundle;
import android.widget.Button;
import android.widget.EditText;
import android.widget.Toast;

import androidx.activity.EdgeToEdge;
import androidx.annotation.RequiresApi;
import androidx.appcompat.app.AppCompatActivity;
import androidx.core.app.ActivityCompat;
import androidx.core.content.ContextCompat;
import androidx.core.graphics.Insets;
import androidx.core.view.ViewCompat;
import androidx.core.view.WindowInsetsCompat;

public class MainActivity extends AppCompatActivity {
    EditText lInp,uInp;
    Button btn;
    @RequiresApi(api = Build.VERSION_CODES.TIRAMISU)
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        EdgeToEdge.enable(this);
        setContentView(R.layout.activity_main);

        if(ContextCompat.checkSelfPermission(this, Manifest.permission.POST_NOTIFICATIONS)!= PackageManager.PERMISSION_GRANTED){
            ActivityCompat.requestPermissions(this,new String[]{Manifest.permission.POST_NOTIFICATIONS},101);
        }

        lInp=findViewById(R.id.lInp);
        uInp=findViewById(R.id.uInp);
        btn=findViewById(R.id.btn);
        btn.setOnClickListener(v->{
            String lower=lInp.getText().toString().trim();
            String upper=uInp.getText().toString().trim();
            if(lower.isEmpty() || upper.isEmpty()){
                Toast.makeText(this, "All fields are requires!", Toast.LENGTH_SHORT).show();
                return;
            }
            Intent intent=new Intent(this,PrimeGenService.class);
            intent.putExtra("lower",Integer.parseInt(lower));
            intent.putExtra("upper",Integer.parseInt(upper));
            startService(intent);
        });
    }
}
```

## PrimeGenService.java

```java
package com.example.myprimeapp;

import static android.app.NotificationManager.IMPORTANCE_DEFAULT;

import android.app.Notification;
import android.app.NotificationChannel;
import android.app.NotificationManager;
import android.app.Service;
import android.content.Intent;
import android.os.Build;
import android.os.IBinder;

import androidx.annotation.Nullable;
import androidx.core.app.NotificationCompat;

import java.util.ArrayList;

public class PrimeGenService extends Service {
    private static final String CHANNEL_ID="PrimeChannel";
    @Override
    @Nullable
    public IBinder onBind(Intent intent) {
        return null;
    }
    public boolean isPrime(int n){
        if(n<=1) return false;
        for(int i=2;i<=Math.sqrt(n);i++){
            if(n%i==0) return false;
        }
        return true;
    }

    @Override
    public int onStartCommand(Intent intent, int flags, int startId) {
        int lower= intent.getIntExtra("lower",0);
        int upper=intent.getIntExtra("upper",0);
        new Thread(()->{
            ArrayList<Integer> primeList=new ArrayList<>();
            for(int i=lower;i<=upper;i++){
                if(isPrime(i)){
                    primeList.add(i);
                }
            }
            sendNotification("Prime list is Ready!","List: "+primeList.toString());
            stopSelf();
        }).start();
        return START_NOT_STICKY;
    }
    public void sendNotification(String title,String msg){
        NotificationManager manager=(NotificationManager) getSystemService(NOTIFICATION_SERVICE);
        if(Build.VERSION.SDK_INT>=Build.VERSION_CODES.O){
            NotificationChannel channel=new NotificationChannel(CHANNEL_ID,"Prime Channel",IMPORTANCE_DEFAULT);
            manager.createNotificationChannel(channel);
        }
        Notification notification=new NotificationCompat.Builder(this,CHANNEL_ID)
                .setContentTitle(title)
                .setContentText(msg)
                .setSmallIcon(android.R.drawable.ic_dialog_info)
                .setStyle(new NotificationCompat.BigTextStyle().bigText(msg))
                .build();
        manager.notify(1,notification);
    }
}
```
