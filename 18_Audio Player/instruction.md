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

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:gravity="center_horizontal|center_vertical"
        android:orientation="vertical"
        tools:layout_editor_absoluteX="1dp"
        tools:layout_editor_absoluteY="1dp">

        <Button
            android:id="@+id/play"
            android:layout_width="183dp"
            android:layout_height="wrap_content"
            android:backgroundTint="@android:color/holo_green_light"
            android:text="Play" />

        <Button
            android:id="@+id/pause"
            android:layout_width="233dp"
            android:layout_height="wrap_content"
            android:backgroundTint="#FFC107"
            android:text="Pause" />

        <Button
            android:id="@+id/stop"
            android:layout_width="226dp"
            android:layout_height="wrap_content"
            android:backgroundTint="@android:color/holo_red_light"
            android:text="Stop" />
    </LinearLayout>
</androidx.constraintlayout.widget.ConstraintLayout>
```

## MainActivity.java

```java
package com.example.mymusicdemoapp;

import android.media.MediaPlayer;
import android.os.Bundle;
import android.widget.Button;
import android.widget.Toast;

import androidx.activity.EdgeToEdge;
import androidx.appcompat.app.AppCompatActivity;
import androidx.core.graphics.Insets;
import androidx.core.view.ViewCompat;
import androidx.core.view.WindowInsetsCompat;

public class MainActivity extends AppCompatActivity {
    Button play,pause,stop;
    MediaPlayer mediaPlayer;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        EdgeToEdge.enable(this);
        setContentView(R.layout.activity_main);

        play=findViewById(R.id.play);
        pause=findViewById(R.id.pause);
        stop=findViewById(R.id.stop);
        mediaPlayer=MediaPlayer.create(this,R.raw.song);

        play.setOnClickListener(v->{
            if(!mediaPlayer.isPlaying()){
                mediaPlayer.start();
                Toast.makeText(this, "Music is playing...", Toast.LENGTH_SHORT).show();
            }
        });
        pause.setOnClickListener(v->{
            if(mediaPlayer.isPlaying()){
                mediaPlayer.pause();
                Toast.makeText(this, "Music Paused.", Toast.LENGTH_SHORT).show();
            }
        });
        stop.setOnClickListener(v->{
            if(mediaPlayer.isPlaying()){
                mediaPlayer.stop();
                mediaPlayer=MediaPlayer.create(this,R.raw.song);
                Toast.makeText(this, "Music Stopped.", Toast.LENGTH_SHORT).show();
            }
        });
    }

    @Override
    protected void onDestroy() {
        super.onDestroy();
        if(mediaPlayer!=null){
            mediaPlayer.release();
            mediaPlayer=null;
        }
    }
}
```

# Add A Music into your project

Create a folder under res, `res/raw` and add your music file like `song.mp3`
