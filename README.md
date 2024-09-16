<p align="center">
<p align="center">
  <a href="https://github.com/i-rin-eam">
    <img src="https://avatars.githubusercontent.com/u/154800878?s=400&u=5d18880cc28646190a19a971bfcdbc54644eab07&v=4" alt="Logo" width="100" height="100">
  </a> 
<h2 align='center'>Interactive ImageView Movement within a RelativeLayout in Android</h12>
</p>

## Step 1: Here is `activity_main.xml` code.
```xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <RelativeLayout
        android:id="@+id/parentLayout"
        android:layout_width="match_parent"
        android:layout_height="300dp"
        android:background="#EEEEEE">
        <!-- Movable ImageView -->
        <ImageView
            android:id="@+id/movable_image"
            android:layout_width="100dp"
            android:layout_height="100dp"
            android:layout_centerInParent="true"
            android:layout_marginTop="200dp"
            android:src="@drawable/ic_launcher_foreground" />

    </RelativeLayout>

    <!-- Lock/Unlock Button -->
    <Button
        android:id="@+id/lock_unlock_button"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_below="@+id/parentLayout"
        android:layout_centerHorizontal="true"
        android:layout_marginTop="20dp"
        android:text="Lock" />

    <!-- Reset Button -->
    <Button
        android:id="@+id/reset_button"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_below="@+id/lock_unlock_button"
        android:layout_centerHorizontal="true"
        android:layout_marginTop="20dp"
        android:text="Reset" />
</RelativeLayout>

```
## Step 2: Here is `MainActivity.java` code.
```java
package com.myeamin.newtestpro;

import android.os.Bundle;
import android.view.MotionEvent;
import android.view.View;
import android.widget.Button;
import android.widget.ImageView;
import android.widget.RelativeLayout;

import androidx.appcompat.app.AppCompatActivity;

public class MainActivity extends AppCompatActivity {

    private ImageView movableImage;
    private Button lockUnlockButton, resetButton;
    private boolean isLocked = false; // Initially locked
    private float initialX, initialY;
    private float dX, dY;
    RelativeLayout parentLayout;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        movableImage = findViewById(R.id.movable_image);
        lockUnlockButton = findViewById(R.id.lock_unlock_button);
        resetButton = findViewById(R.id.reset_button);
        parentLayout = findViewById(R.id.parentLayout);

        // Save initial position of ImageView
        movableImage.post(new Runnable() {
            @Override
            public void run() {
                initialX = movableImage.getX();
                initialY = movableImage.getY();
            }
        });

        // OnTouchListener to move the ImageView
        movableImage.setOnTouchListener(new View.OnTouchListener() {
            @Override
            public boolean onTouch(View view, MotionEvent event) {
                if (isLocked) {
                    return false; // If locked, prevent moving
                }

                int action = event.getAction();
                if (action == MotionEvent.ACTION_DOWN) {
                    dX = view.getX() - event.getRawX();
                    dY = view.getY() - event.getRawY();
                } else if (action == MotionEvent.ACTION_MOVE) {// Calculate new position
                    float newX = event.getRawX() + dX;
                    float newY = event.getRawY() + dY;

                    // Get the parent layout's dimensions
                    int parentWidth = parentLayout.getWidth();
                    int parentHeight = parentLayout.getHeight();

                    // Get the dimensions of the ImageView
                    int imageViewWidth = view.getWidth();
                    int imageViewHeight = view.getHeight();

                    // Calculate boundaries to keep ImageView within parentLayout
                    float leftBoundary = 0;
                    float rightBoundary = parentWidth - imageViewWidth;
                    float topBoundary = 0;
                    float bottomBoundary = parentHeight - imageViewHeight;

                    // Constrain newX and newY within the boundaries
                    newX = Math.max(leftBoundary, Math.min(newX, rightBoundary));
                    newY = Math.max(topBoundary, Math.min(newY, bottomBoundary));

                    view.animate()
                            .x(newX)
                            .y(newY)
                            .setDuration(0)
                            .start();
                } else {
                    return false;
                }
                return true;
            }
        });

        // Lock/Unlock Button functionality
        lockUnlockButton.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                if (isLocked) {
                    isLocked = false;
                    lockUnlockButton.setText("Lock");
                } else {
                    isLocked = true;
                    lockUnlockButton.setText("Unlock");
                }
            }
        });

        // Reset Button functionality to reset ImageView to its initial position
        resetButton.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                movableImage.animate()
                        .x(initialX)
                        .y(initialY)
                        .setDuration(300)
                        .start();
            }
        });
    }
}
```
## Authors

**MD YEAMIN** - Android Software Developer <a href="https://www.youtube.com/@LearnWithYeamin">**(Learn With Yeamin)**</a> 

<h1 align="center">Thank You ❤️</h1>
