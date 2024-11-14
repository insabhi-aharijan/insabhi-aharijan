Flutter App Deployment Guide for Google Play Store
1. Prepare Your App for Release:----
      Update app information in build.gradle
      Open the android/app/build.gradle file.

In the defaultConfig block, set your unique applicationId:

 '''defaultConfig {
        applicationId "com.yourdomain.yourappname"
        minSdkVersion 23
        targetSdkVersion 34
        versionCode flutterVersionCode.toInteger()
        versionName flutterVersionName
    }'''

applicationId: Unique ID for your app.
versionCode: Integer representing the app version (increment with each release).
versionName: User-friendly version name (e.g., 1.0.0).

2. Create and Configure a Keystore:----
      To sign your app, create a keystore file.
      
      (0) Create the Keystore:--
      Run this command in your terminal (replace alias and path with your details):
      
              '''keytool -genkey -v -keystore $env:USERPROFILE\upload-keystore.jks `
                      -storetype JKS -keyalg RSA -keysize 2048 -validity 10000 `
                      -alias upload '''
                      
      (0) Store the Keystore Securely: It will be used for all future app updates.
   
      Add Keystore Details to build.gradle:--
      -- Place the keystore file in your project’s android/app directory.
      -- Create a key.properties file in the android directory with your keystore information:

         key.properties:-
            storePassword=your_keystore_password
            keyPassword=your_key_password
            keyAlias=your_key_alias
            storeFile=your-keystore.jks

      (0) In android/app/build.gradle, add this configuration to load the keystore details:
           '''def keystoreProperties = new Properties()
              def keystorePropertiesFile = rootProject.file("key.properties")
              keystoreProperties.load(new FileInputStream(keystorePropertiesFile))
              
              android {
                  signingConfigs {
                      release {
                          keyAlias keystoreProperties['keyAlias']
                          keyPassword keystoreProperties['keyPassword']
                          storeFile file(keystoreProperties['storeFile'])
                          storePassword keystoreProperties['storePassword']
                      }
                  }
                  buildTypes {
                      release {
                          signingConfig signingConfigs.release
                      }
                  }
              }'''

       (0) Add key.properties to .gitignore to keep your keystore secure.

3. Build the Release APK or App Bundle
    Build a release version of your app for the Play Store:

    Run this command in the terminal in the project location:-

    '''flutter build appbundle --release'''



      
      

      
