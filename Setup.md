# Setup Steps from Tutorial 10 to 1
This guide will show you the steps to perform if you are working from Tutorial 10 source code.

# Install Google Signin

`yarn add @react-native-community/google-signin`

# iOS Setup for Google Signin

`cd ios && pod install && cd ..`

Configure URL types in the xCode Project target Info panel. Under the URL Types, add in a new URL Type by specify the Reversed Client ID in the URL Schemes. The Reversed Client ID can be found in the GoogleService-Info.plist file.

# Android Setup for Google Signin
## Update android/build.gradle

```
buildscript {
    ext {
        ...
        googlePlayServicesAuthVersion = "16.0.1" // <--- use this version or newer
    }
...
    dependencies {
        ...
    }
...
allprojects {
    repositories {
        mavenLocal()
        google() // <--- make sure this is included
        jcenter()
        ...
    }
}
```

## Update android/app/build.gradle

```
...
dependencies {
    ...
    implementation 'androidx.swiperefreshlayout:swiperefreshlayout:1.0.0' // <-- add this - already hv; newer versions should work too
}

apply plugin: 'com.google.gms.google-services' // <--- this should be the last line
```

## Google Sign In require the app need to be signed by a keystore.
By default, a debug.keystore is created in android/app folder when you create the React Native project. The android/app/build.gradle is configured to use this debug keystore. To properly configure Firebase Authentication, we need to get the SHA fingerprint from the key store and added in the Firebase Console.

* Show keystore info
`keytool -list -v -alias androiddebugkey -keystore ./android/app/debug.keystore -storepass android -keypass android`

Copy the SHA from the terminal output and paste it into the Firebase Console.

In Firebase Console, click on the Project. Under the Project Overview, click on the gear icon. Under the general tab, click on the android app. Click on the "Add fingerprint" button. Paste the copied SHA value into the textbox and click on the save button.

Click on the download "google-services.json" file button to download the JSON file. Copy the downloaded JSON file into the android/app folder.

## Clean the build files

`cd android && ./gradlew clean && cd ..`
