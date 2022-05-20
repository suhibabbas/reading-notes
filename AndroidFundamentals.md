# Android Fundamentals

Android apps can be written using Kotlin, Java, and C++ languages.

Each Android app lives in its own security sandbox, protected by the following Android security features:

1. The Android operating system is a multi-user Linux system in which each app is a different user.
2. y default, the system assigns each app a unique Linux user ID (the ID is used only by the system and is unknown to the app). The system sets permissions for all the files in an app so that only the user ID assigned to that app can access them.
3. Each process has its own virtual machine (VM), so an app's code runs in isolation from other apps.
4. By default, every app runs in its own Linux process. The Android system starts the process when any of the app's components need to be executed, and then shuts down the process when it's no longer needed or when the system must recover memory for other apps.

the ways for sharing data with other apps:

1. it can be done if the tow of them have the same Linux user ID.
2. by reaquest permission to access device data.

App components

App components are the essential building blocks of an Android app.

There are four different types of app components:

1. Activities
2. Services
3. Broadcast receivers
4. Content providers

## The manifest file

the systme must Know that the component exists by reading the app's manifest file, `AndroidManifest.xml`.

The manifest does a number of things in addition to declaring the app's components, such as the following:

- Identifies any user permissions the app requires, such as Internet access or read-access to the user's contacts.
- Declares the minimum API Level required by the app, based on which APIs the app uses.
- Declares hardware and software features used or required by the app, such as a camera, bluetooth services, or a multitouch screen.
- Declares API libraries the app needs to be linked against (other than the Android framework APIs), such as the Google Maps library.