# LTA mobile apps 
#### *iPhone app, Android app  and Firebase database*
[![License](https://img.shields.io/badge/License-Apache_2.0-blue.svg)](https://opensource.org/licenses/Apache-2.0) 
[![Generic badge](https://img.shields.io/badge/iOS-Swift-orange.svg)](https://developer.apple.com/swift) [![Generic badge](https://img.shields.io/badge/Android-Kotlin-lightblue.svg)](https://kotlinlang.org/) [![Generic badge](https://img.shields.io/badge/Cloud-Firebase-yellow.svg)](https://firebase.google.com/) [![Generic badge](https://img.shields.io/badge/IDE-Xcode-blue.svg)](https://developer.apple.com/xcode/) [![Generic badge](https://img.shields.io/badge/IDE-AndroidStudio-green.svg)](https://developer.android.com/studio)

## Firebase
On Firebase LTA use **Auth**, **cloud messaging** and **realtime database**. LTA also retrieves the correct url from firebase db depending on whether you selected to use prod or staging server.
### Auth
Users / participants are created manually on the firebase console. A fake email is used for login, as firebase needs email to create users. The fake e-mail is never visible to the user, it is put together in code before authentication is done.
`username` -> `username@fake_email.com`

In the firebase web console, create anonymous user names per study, and admin user names, eg  `s1_abc_123`  and  `adm_def_456`, and secure passwords for users.
### Cloud messaging
When the user click a received notification, the app is opened and the lists are updated with the new survey displayed as active.
The app registers for push notification at startup and the IDToken is sent to the backend. A listener is also registered and updates the IDToken in the event of a change. The user's IDToken is used to send push notifications when a new survey is sent out. 

### Realtime database
URL to backend is managed from firebase realtime database. There, the url is saved for production and staging servers and retrieved for calls from the app.
Which url is downloaded depends on the staging switch in the menu.
`Database.database().reference().child("url")`
or
`Database.database().reference().child("stagingUrl")`
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
To use the app, log in with one of the user accounts created on firebase. The app will download the list of assigned surveys for that login and display in the list.
### Register for messaging
At startup, the app is registered for firebase cloud messaging and the user ID is saved to the backend and linked to the username, so notification can be sent when a new survey is sent out to that user.
### Defining admin user names in the apps
In the code you can enter admin user names, so that the test switch and staging server switch are displayed in the menu.
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
Make sure that external libraries are imported correctly and that the apps can communicate with firebase.
### Url
At root level enter the url for your backend on the realtime database:
url : `<yourUrl>`
and
stagingUrl : `<yourStagingUrl>`
### Users and admin users
You also need to create a number of admin users, and anonymous users.
Open the Firebase console for your project. Under `Authentication/sign-in providers` enable `Email/Password`.
Then, under `Users`, click `add user` and enter the email and password for the new user. This is where you use the fake email address `username@fake_email.com`.

If the created user is to be an admin, you need to enter the username in the code for the apps and also in the Web Service config file, so that user is managed as an admin in the apps and can also log in to the web app to assign surveys.
You also need to enter the fake email address you have chosen to use in the code for the apps, so the same address is used.

### Is login working?
Try logging in to the apps both as an admin and as an anonymous user. If you log in as admin, the test view should appear at the bottom of the menu. 
1. If you can't log in, check error messages in the log. Make sure you enter the correct fake email address in the code.
2. If you can log in but there is no test view when you log in as admin, check that you have entered the correct admin username in the code.
### Push notification
To check that push notification works properly, you can send a notification manually from Firebase.
Go to `Cloud Messaging` on the Firebase Console and click on `New Notification`. Notifications is later sent automatically from the backend when a survey is published. 
