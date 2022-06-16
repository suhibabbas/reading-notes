# Kinesis - Location

Using the Google Play services location APIs, your app can request the last known location of the user's device.

## Set up Google Play services

You need the Google Play services so you can access the fused location provider.

## Specify app permissions

Apps whose features use location services must request location permissions, depending on the use cases of those features.

## Create location services client

```Java
private FusedLocationProviderClient fusedLocationClient;

// ..

@Override
protected void onCreate(Bundle savedInstanceState) {
    // ...

    fusedLocationClient = LocationServices.getFusedLocationProviderClient(this);
}
```

### Get the last known location

You can use the fused location provider `getLastLocation()` method to to retrieve the device location.

```java
fusedLocationClient.getLastLocation()
        .addOnSuccessListener(this, new OnSuccessListener<Location>() {
            @Override
            public void onSuccess(Location location) {
                // Got last known location. In some rare situations this can be null.
                if (location != null) {
                    // Logic to handle location object
                }
            }
        });
```

The location object may be `null` in the following situations:

- Location is turned off in the device settings.
- The device never recorded its location.
- Google Play services on the device has restarted.

## Choose the best location estimate

- `getLastLocation()` gets a location estimate more quickly and minimizes battery usage that can be attributed to your app. However, the location information might be out of date, if no other clients have actively used location recently.
- `getCurrentLocation()` gets a fresher, more accurate location more consistently. However, this method can cause active location computation to occur on the device.
