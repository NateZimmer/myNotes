# Cordova Tips


#### Validate Req. Android:

```
cordova requirements android
```

#### Build Android IPK

```
cordova build android --verbose
```

#### Check Active Devices: 

Note this only works because of the android SDK path added

```
adb devices
```

#### Deploy to Android: 

After ensuring your device is `adb` authorized, the following will work. 

```
cordova run android
```
