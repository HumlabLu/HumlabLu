

# Firebase
[![Generic badge](https://img.shields.io/badge/Cloud-Firebase-yellow.svg)](https://firebase.google.com/)

On Firebase LTA use **Authentication**, **Cloud Messaging** and **Realtime Database**. LTA also retrieves the correct url from Firebase db depending on whether you selected to use prod or staging server.
### Authentication
Users / participants are created manually on the Firebase console. A fake email is used for login, as Firebase needs email to create users. The fake e-mail is never visible to the user, it is put together in code before authentication is done.
`username` -> `username@fake_email.com`

In the Firebase web console, create anonymous user names per study, and admin user names, eg  `s1_abc_123`  and  `adm_def_456`, and secure passwords for users.
### Cloud Messaging
When the user click a received notification, the app is opened and the lists are updated with the new survey displayed as active.
The app registers for push notification at startup and the IDToken is sent to the backend. A listener is also registered and updates the IDToken in the event of a change. The user's IDToken is used to send push notifications when a new survey is sent out. 

### Realtime database
URL to backend is managed from Firebase Realtime Database. There, the url is saved for production and staging servers and retrieved for calls from the app.
Which url is downloaded depends on the staging switch in the menu.
`Database.database().reference().child("url")`
or
`Database.database().reference().child("stagingUrl")`
