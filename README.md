# Develop an Android Application Demonstrating AlertDialog and Custom DialogFragment

[← Back to Index](README.md)

## Menus, Dialogs and UI Interaction

**Practical No.:** 2  
**Project Name:** `DialogDemo`

## Aim

To develop an Android application demonstrating `AlertDialog` and a custom `DialogFragment` for displaying messages, collecting user input, and handling dialog actions.

## I. Filename: `activity_main.xml`

**Location:** `app/src/main/res/layout/activity_main.xml`

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/main"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:gravity="center"
    android:padding="20dp">

    <Button
        android:id="@+id/btnAlert"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Show AlertDialog" />

    <Button
        android:id="@+id/btnCustom"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Show Custom Dialog"
        android:layout_marginTop="20dp" />

</LinearLayout>
```

## II. Create the Custom Dialog Layout

Create an XML layout file named `dialog_custom.xml`.

**Location:** `app/src/main/res/layout/dialog_custom.xml`

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:orientation="vertical"
    android:padding="20dp">

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Custom DialogFragment"
        android:textSize="22sp"
        android:textStyle="bold" />

    <EditText
        android:id="@+id/etName"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:hint="Enter Name"
        android:inputType="textPersonName"
        android:layout_marginTop="15dp" />

</LinearLayout>
```

## III. Filename: `CustomDialogFragment.java`

**Location:** `app/src/main/java/com/example/dialogdemo/CustomDialogFragment.java`

```java
package com.example.dialogdemo;

import android.app.AlertDialog;
import android.app.Dialog;
import android.os.Bundle;
import android.view.LayoutInflater;
import android.view.View;
import android.widget.EditText;
import android.widget.Toast;

import androidx.annotation.NonNull;
import androidx.fragment.app.DialogFragment;

public class CustomDialogFragment extends DialogFragment {

    private EditText etName;

    @NonNull
    @Override
    public Dialog onCreateDialog(Bundle savedInstanceState) {

        AlertDialog.Builder builder =
                new AlertDialog.Builder(requireActivity());

        LayoutInflater inflater = requireActivity().getLayoutInflater();

        View view = inflater.inflate(R.layout.dialog_custom, null);

        etName = view.findViewById(R.id.etName);

        builder.setView(view)
                .setTitle("Custom Dialog")
                .setPositiveButton("Submit", (dialog, which) -> {

                    String name = etName.getText().toString().trim();

                    if (name.isEmpty()) {
                        Toast.makeText(
                                requireContext(),
                                "Please enter your name",
                                Toast.LENGTH_SHORT
                        ).show();
                    } else {
                        Toast.makeText(
                                requireContext(),
                                "Entered: " + name,
                                Toast.LENGTH_SHORT
                        ).show();
                    }
                })
                .setNegativeButton("Cancel", (dialog, which) ->
                        dialog.dismiss());

        return builder.create();
    }
}
```

## IV. Filename: `MainActivity.java`

**Location:** `app/src/main/java/com/example/dialogdemo/MainActivity.java`

```java
package com.example.dialogdemo;

import android.os.Bundle;
import android.widget.Button;
import android.widget.Toast;

import androidx.appcompat.app.AlertDialog;
import androidx.appcompat.app.AppCompatActivity;

public class MainActivity extends AppCompatActivity {

    private Button btnAlert, btnCustom;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        btnAlert = findViewById(R.id.btnAlert);
        btnCustom = findViewById(R.id.btnCustom);

        // AlertDialog Example
        btnAlert.setOnClickListener(v -> {

            AlertDialog.Builder builder =
                    new AlertDialog.Builder(MainActivity.this);

            builder.setTitle("Delete File")
                    .setMessage("Are you sure you want to delete?")
                    .setPositiveButton("YES", (dialog, which) -> {

                        Toast.makeText(
                                MainActivity.this,
                                "File Deleted",
                                Toast.LENGTH_SHORT
                        ).show();

                    })
                    .setNegativeButton("NO", (dialog, which) ->
                            dialog.dismiss());

            builder.show();
        });

        // Custom Dialog Example
        btnCustom.setOnClickListener(v -> {

            CustomDialogFragment dialog =
                    new CustomDialogFragment();

            dialog.show(
                    getSupportFragmentManager(),
                    "CustomDialog"
            );
        });
    }
}
```

## Explanation

1. **LinearLayout:** Arranges the two buttons vertically in the center of the screen.
2. **AlertDialog:** Displays a confirmation message with YES and NO buttons.
3. **DialogFragment:** Displays a custom dialog using a separate XML layout.
4. **EditText:** Allows the user to enter their name in the custom dialog.
5. **YES Button:** Displays the message "File Deleted".
6. **NO Button:** Closes the alert dialog without displaying the confirmation message.
7. **Submit Button:** Displays the entered name or asks the user to enter a name.
8. **Cancel Button:** Closes the custom dialog.

## Output

The application displays two buttons:

- **Show AlertDialog:** Opens a confirmation dialog titled "Delete File" with YES and NO options.
- **Show Custom Dialog:** Opens a custom dialog containing a name input field, Submit button, and Cancel button.

## Result

Thus, an Android application demonstrating `AlertDialog` and a custom `DialogFragment` was developed successfully.

## Important Setup Notes

- Ensure that the package name matches your Android project's package name. This example uses `com.example.dialogdemo`.
- The project must include AndroidX AppCompat and AndroidX Fragment dependencies. The standard AndroidX AppCompat project template generally includes the required dependencies.
- The YES button in this demonstration only displays a Toast message; it does not actually delete a file.
