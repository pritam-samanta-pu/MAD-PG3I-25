# Simple Static Auth in an Java Based Android App

## 1. `activity_main.xml`

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
        android:id="@+id/eInp"
        android:layout_width="298dp"
        android:layout_height="42dp"
        android:ems="10"
        android:hint="Enter Email:"
        android:inputType="text"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.495"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintVertical_bias="0.258" />

    <EditText
        android:id="@+id/pInp"
        android:layout_width="297dp"
        android:layout_height="49dp"
        android:ems="10"
        android:hint="Enter Password:"
        android:inputType="textPassword"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toBottomOf="@+id/eInp"
        app:layout_constraintVertical_bias="0.038" />

    <Button
        android:id="@+id/btn"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:enabled="false"
        android:text="Login"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.498"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintVertical_bias="0.499" />

    <TextView
        android:id="@+id/errorMail"
        android:layout_width="293dp"
        android:layout_height="26dp"
        android:fontFamily="monospace"
        android:includeFontPadding="true"
        android:text="Invalid Email!"
        android:textColor="@android:color/holo_red_light"
        android:visibility="gone"
        app:layout_constraintBottom_toTopOf="@+id/pInp"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.516"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toBottomOf="@+id/eInp"
        app:layout_constraintVertical_bias="0.0" />
</androidx.constraintlayout.widget.ConstraintLayout>
```

## 2. `MainActivity.java`

```java
package com.example.mytemp; //Your app name in smallCase

import android.content.Intent;
import android.os.Bundle;
import android.text.Editable;
import android.text.TextWatcher;
import android.view.View;
import android.widget.Button;
import android.widget.EditText;
import android.widget.TextView;
import android.widget.Toast;

import androidx.activity.EdgeToEdge;
import androidx.appcompat.app.AppCompatActivity;
import androidx.core.graphics.Insets;
import androidx.core.view.ViewCompat;
import androidx.core.view.WindowInsetsCompat;

import java.util.HashMap;
import java.util.Map;
import java.util.regex.Pattern;

public class MainActivity extends AppCompatActivity {
    EditText eInp,pInp;
    Button btn;
    TextView errorMail;

    final static Pattern emailPattern=Pattern.compile("^[a-zA-Z0-9+_.-]+@[a-zA-Z0-9.-]+$");

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        EdgeToEdge.enable(this);
        setContentView(R.layout.activity_main);

        eInp=findViewById(R.id.eInp);
        pInp=findViewById(R.id.pInp);
        btn=findViewById(R.id.btn);
        errorMail=findViewById(R.id.errorMail);

        Map<String,String> db=new HashMap<>();
        db.put("admin@gmail.com","123456");
        db.put("abc@gmail.com","123456");

        TextWatcher textWatcher=new TextWatcher() {
            @Override
            public void afterTextChanged(Editable editable) {
                checkValidation();
            }
            @Override
            public void beforeTextChanged(CharSequence charSequence, int i, int i1, int i2){}
            @Override
            public void onTextChanged(CharSequence charSequence, int i, int i1, int i2){}
        };
        eInp.addTextChangedListener(textWatcher);
        pInp.addTextChangedListener(textWatcher);
        btn.setOnClickListener(v->{
            String email=eInp.getText().toString().trim();
            String pass=pInp.getText().toString().trim();

            if(db.containsKey(email)){
                if(db.get(email).matches(pass)){
                    Intent intent=new Intent(this,SecondActivity.class);
                    intent.putExtra("email",email);
                    startActivity(intent);
                }else{
                    Toast.makeText(this, "Enter a valid Password!", Toast.LENGTH_SHORT).show();
                }
            }else{
                Toast.makeText(this, "User is not Registered!", Toast.LENGTH_SHORT).show();
            }
        });

    }
    public void checkValidation(){
        String email=eInp.getText().toString().trim();
        String pass=pInp.getText().toString().trim();
        boolean isEmail=emailPattern.matcher(email).matches();
        boolean isPass=!pass.isEmpty();

        if(!email.isEmpty() && !isEmail){
            errorMail.setVisibility(View.VISIBLE);
        }else{
            errorMail.setVisibility(View.GONE);
        }

        btn.setEnabled(isEmail && isPass);
    }
}
```

## 3. `activity_second.xml`

```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/main"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".SecondActivity">

    <TextView
        android:id="@+id/tv"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:textSize="20sp"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.52"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintVertical_bias="0.475" />
</androidx.constraintlayout.widget.ConstraintLayout>
```

## 4. `SecondActivity.java`

```java
package com.example.mytemp; //Your app name in smallCase

import android.os.Bundle;
import android.widget.TextView;

import androidx.activity.EdgeToEdge;
import androidx.appcompat.app.AppCompatActivity;
import androidx.core.graphics.Insets;
import androidx.core.view.ViewCompat;
import androidx.core.view.WindowInsetsCompat;

public class SecondActivity extends AppCompatActivity {
    TextView tv;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        EdgeToEdge.enable(this);
        setContentView(R.layout.activity_second);

        tv=findViewById(R.id.tv);
        tv.setPadding(40,110,40,40);
        tv.setText("Logged in as:\n"+getIntent().getStringExtra("email"));

    }
}
```
