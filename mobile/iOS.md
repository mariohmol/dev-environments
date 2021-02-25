# iOS

## Plugins

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


```sh
yarn react-native run-ios
yarn react-native run-android
```


## App Icons

Generate the icons here https://appicon.co/ and you will have an ios folder with a lot of different sizes (20, 40 ..)

Open your Xcode and load the projectname.xcworkspace file. On the root directory you will have somethins like ProjectName > ProjectName > Images.xcassets. Add the images from the generated folder according with the size asked in Xcode.

