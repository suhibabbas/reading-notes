# Monetization

Publishing is the general process that makes your Android applications available to users.

## Preparing your app for release

Preparing your application for release is a multi-step process that involves the following tasks:

1. Configuring your application for release.
    by removing `Log` calls and remove the  `android:debuggable` and make sure to provide values for the `android:versionCode` and `android:versionName` attributes.
2. Building and signing a release version of your application.
3. Testing the release version of your application.
    test the application on at least one target handset device and one target tablet device.
4. Updating application resources for release.
5. Preparing remote servers and services that your application depends on.

## Releasing your app to users

You can release your Android applications several ways.

### Releasing through an app marketplace

you can distribute your apps through any app marketplace you want or you can use multiple marketplaces.

### Releasing your apps on Google Play

Releasing your application on Google Play is a simple process that involves three basic steps:

1. reparing promotional materials.
2. Configuring options and uploading assets.
3. Publishing the release version of your application.

### Releasing through a website

f you do not want to release your app on a marketplace like Google Play, you can make the app available for download on your own website or server, including on a private or enterprise server.

the installation process will start automatically only if the user has configured their Settings to allow the installation of apps from unknown sources.

## User opt-in for unknown apps and sources

Android protects users from inadvertent download and install of apps from locations other than a first-party app store, such as Google Play, which is trusted.

### Install unknown apps

On devices running Android 8.0 (API level 26) and higher, users must navigate to the Install unknown apps system settings screen to enable app installations from a particular source.

### Unknown sources

On devices running Android 7.1.1 (API level 25) and lower, users must either enable the Unknown sources system setting or allow a single installation of an unknown app.
