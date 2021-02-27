# iOS

To start your app use:
```sh
yarn react-native run-ios
```
## Plugins

To install modules:
```sh
yarn add react-native-gesture-handler
yarn add react-native-webview
cd ios && pod install && cd .. # CocoaPods on iOS needs this extra step
```

## Pods

Linking plugins:
```sh
# You should not need to link libs, so to unlink
react-native unlink @react-native-community/async-storage
react-native unlink react-native-gesture-handler

# Then you can link and update the pod install
npx react-native link @react-native-community/async-storage
cd ios && pod install
```

## Devices

List all devices and simulators:
`xcrun instruments -s devices`

Run a in a specific emulater, by name or uid:
```
react-native run-ios --device "iPad Pro (9.7-inch) (14.4)"
react-native run-ios --udid D2F18BBA-FB52-4C4B-9F7B-E14E5CFA0827
```
### iPad

In the xcode project, select the Target with your Project Name, go to the general tab and then to the Deployment Info. 
There you can check iPad as well and next time you run your project with "react-native run-ios" targeting an iPad device, it will display the app in fullscreen.

## App Icons

Generate the icons here https://appicon.co/ and you will have an ios folder with a lot of different sizes (20, 40 ..)

Open your Xcode and load the projectname.xcworkspace file. On the root directory you will have somethins like ProjectName > ProjectName > Images.xcassets. Add the images from the generated folder according with the size asked in Xcode.
