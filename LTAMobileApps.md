# LTA Mobile apps 
#### *iPhone and  Android app*
[![License](https://img.shields.io/badge/License-Apache_2.0-blue.svg)](https://opensource.org/licenses/Apache-2.0) 
[![Generic badge](https://img.shields.io/badge/iOS-Swift-orange.svg)](https://developer.apple.com/swift) [![Generic badge](https://img.shields.io/badge/Android-Kotlin-lightblue.svg)](https://kotlinlang.org/) [![Generic badge](https://img.shields.io/badge/IDE-Xcode-blue.svg)](https://developer.apple.com/xcode/) [![Generic badge](https://img.shields.io/badge/IDE-AndroidStudio-green.svg)](https://developer.android.com/studio)

Click [here](https://github.com/HumlabLu/LangTrackAppAndroid) to go to Android repo

## Apps

### iPhone

The iOS app is written in **Swift** with **Xcode**. It is made for iPhone and is not optimized for use with iPad.

You need to connect the app to your Firebase account and implement dependencies.
For LTA we used [CocoaPods](https://cocoapods.org/) as dependency manager, see `LangTrackAppIphone/Podfile`.

### Android

The Android app is written in **Kotlin** with **Android Studio**, MVVM design pattern. It is made for phone and is not optimized for use with tablet.
You need to connect the app to your fire base account and implement dependencies, listed in `langTrackAppAndroid/app/build.gradle`.

### Language support
Both apps have language support for
 - English
 - Arabic
 - Turkish
 - Persian (Farsi)
 - Persian (Dari)
 - Swedish

### Login
To use the app, log in with one of the user accounts created on Firebase. The app will download the list of assigned surveys for that login and display in the list.
### Register for messaging
At startup, the app is registered for Firebase Cloud Messaging and the user ID is saved to the backend and linked to the username, so notification can be sent when a new survey is sent out to that user.
### Defining Admin user names in the apps
In the code you can enter Admin user names, so that the test switch and staging server switch are displayed in the menu.
### Notification
A push notification is received.
##### If the app is open:
No notification is displayed, the lists are updated and the new survey is displayed as active in the phone.
##### If the app is closed:
The notification is shown and a number is displayed on the icon.
If the notification is clicked, the app opens, the lists are updated and the new survey is displayed as active in the phone.
When the new survey is opened, the number disappears from the icon.
## Get it spinning
### Apps and Firebase account
You need to use this code to create your own apps for iOS and Android and a new project on Firebase where you connect your mobile apps, as described for each platform on Firebase.
Make sure that external libraries are imported correctly and that the apps can communicate with Firebase.
### Url
At root level enter the url for your backend on the Realtime Database:
url : `<yourUrl>`
and
stagingUrl : `<yourStagingUrl>`
### Users and Admin users
You also need to create a number of Admin users, and anonymous users.
Open the Firebase console for your project. Under `Authentication/sign-in providers` enable `Email/Password`.
Then, under `Users`, click `add user` and enter the email and password for the new user. This is where you use the fake email address `username@fake_email.com`. Firebase requires an email address for login and we have chosen to solve this with a fake email address that is added at login and is never visible to users, who only log in with their username and password

If the created user is to be an Admin, you need to enter the username in the code for the apps and also in the Web Service config file, so that user is managed as an Admin in the apps and can also log in to the Admin Page to assign surveys.
You also need to enter the fake email address you have chosen to use in the code for the apps, so the same address is used.

### Is login working?
Try logging in to the apps both as an Admin and as an anonymous user. If you log in as Admin, the test view should appear at the bottom of the menu. 
1. If you can't log in, check error messages in the log. Make sure you enter the correct fake email address in the code.
2. If you can log in but there is no test view when you log in as Admin, check that you have entered the correct Admin username in the code.
### Push notification
To check that push notification works properly, you can send a notification manually from Firebase.
Go to `Cloud Messaging` on Firebase Console and click on `New Notification`. Notifications is later sent automatically from the backend when a survey is published. 
