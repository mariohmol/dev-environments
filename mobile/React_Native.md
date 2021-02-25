# React Native

Environment:
```sh
export JAVA_HOME=/Applications/Android\ Studio.app/Contents/jre/jdk/Contents/Home
export ANDROID_SDK_ROOT=/Users/username/Library/Android/sdk
```

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

## Typescript

Create a new react native app with typescript support:
```sh
npm uninstall -g react-native-cli
npx react-native init ProjectName --template react-native-template-typescript
```

Run instructions for iOS:
* `npx react-native run-ios`
* - or - Open ProjectName/ios/ProjectName.xcworkspace in Xcode or run "xed -b ios"

Run instructions for Android, have an Android emulator running , or a device connected, and run `npx react-native run-android`
Run instructions for [Windows and macOS](https://aka.ms/ReactNative)


# UI

* https://github.com/jondot/awesome-react-native
* https://magnus-ui.com/docs/dropdown/

TODO: npm i --save react-native-slider

## reactNativeNavigation

This is a step by step comprehensive React Native Navigation v5 tutorial. In this tutorial, we will learn how to implement Stack Navigation in React Native app using Stack Navigator.

* https://reactnavigation.org/docs/getting-started/
* [React Native Navigation v5 Example Tutorial](https://www.positronx.io/react-native-navigation-example-tutorial/)
* [Passing & Getting Params to Screen](https://www.positronx.io/react-native-stack-navigator-passing-getting-params-to-screen/)


# Expo

* Clone repo and run  `npm install`
* Download and install [Android Studio](https://developer.android.com/studio)

To update for a new SQK:
* expo update 39.0.0
