# Building an Android WebView App for https://app.viktorostergren.io/

This guide explains how to create a simple Android application that loads the website `https://app.viktorostergren.io/` in a WebView. The resulting APK can be installed on Android devices.
You can find a minimal sample project in the [webwrapper-app](webwrapper-app/) directory of this repository.

The repository also includes a GitHub Actions workflow that automatically builds
the APK so you can download it without installing Android Studio.

## Prerequisites

- **Android Studio** installed on your system (version 4.0 or later).
- **Java Development Kit (JDK)** 8 or newer.

## Steps

1. **Create a New Android Project**
   - Open Android Studio and choose **"New Project"**.
   - Select **"Empty Activity"** and click **Next**.
   - Set the **Application name** (e.g., `WebWrapper`).
   - Choose a **package name** (e.g., `io.viktorostergren.webwrapper`).
   - Select the **minimum SDK** (API 21 or higher recommended).
   - Finish to create the project.

2. **Configure the Layout**
   - Open `app/src/main/res/layout/activity_main.xml` and replace its contents with:

   ```xml
   <?xml version="1.0" encoding="utf-8"?>
   <androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
       xmlns:app="http://schemas.android.com/apk/res-auto"
       android:layout_width="match_parent"
       android:layout_height="match_parent">

       <WebView
           android:id="@+id/webview"
           android:layout_width="0dp"
           android:layout_height="0dp"
           app:layout_constraintTop_toTopOf="parent"
           app:layout_constraintBottom_toBottomOf="parent"
           app:layout_constraintLeft_toLeftOf="parent"
           app:layout_constraintRight_toRightOf="parent" />
   </androidx.constraintlayout.widget.ConstraintLayout>
   ```

3. **Load the Website in a WebView**
   - Open `app/src/main/java/<your package>/MainActivity.java` and update the code:

   ```java
   package io.viktorostergren.webwrapper; // use your own package name

   import android.os.Bundle;
   import android.webkit.WebView;
   import android.webkit.WebViewClient;
   import androidx.appcompat.app.AppCompatActivity;

   public class MainActivity extends AppCompatActivity {
       @Override
       protected void onCreate(Bundle savedInstanceState) {
           super.onCreate(savedInstanceState);
           setContentView(R.layout.activity_main);

           WebView webView = findViewById(R.id.webview);
           webView.getSettings().setJavaScriptEnabled(true);
           webView.setWebViewClient(new WebViewClient());
           webView.loadUrl("https://app.viktorostergren.io/");
       }
   }
   ```

4. **Update the Android Manifest**
   - Edit `app/src/main/AndroidManifest.xml` to include internet permissions:

   ```xml
   <manifest xmlns:android="http://schemas.android.com/apk/res/android"
       package="io.viktorostergren.webwrapper"> <!-- use your package -->

       <uses-permission android:name="android.permission.INTERNET" />

       <application
           android:allowBackup="true"
           android:label="@string/app_name"
           android:supportsRtl="true"
           android:theme="@style/Theme.AppCompat.Light.NoActionBar">
           <activity android:name=".MainActivity">
               <intent-filter>
                   <action android:name="android.intent.action.MAIN" />
                   <category android:name="android.intent.category.LAUNCHER" />
               </intent-filter>
           </activity>
       </application>
   </manifest>
   ```

5. **Build the APK**
   - In Android Studio, click **"Build" → "Build Bundle(s) / APK(s)" → "Build APK(s)"**.
   - Once the build completes, you'll find the APK in the `app/build/outputs/apk/` directory.

6. **Install the APK on a Device**
   - Transfer the APK file to your Android device.
   - Enable installation from unknown sources in your device settings (if necessary).
 - Open the APK on the device and follow the prompts to install.

## Download from GitHub

Every push builds the APK using GitHub Actions. Visit the repository's
**Actions** tab, open the latest workflow run, and download the `WebWrapper-APK`
artifact.

## Notes

- This approach simply wraps the web app inside a WebView. If you want offline functionality or deeper integration, consider building a native Android app that interacts with your backend APIs.
- Ensure the website allows embedding in a WebView and supports mobile-friendly layout.

