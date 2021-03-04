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

TODO: 
```sh
# Slider
npm i --save react-native-slider
```

## Paper - Material Design

```sh
npm install react-native-paper

# Dependencies
npm install react-native-gesture-handler
npm add @babel/runtime
```
**[Dropdown for paper](https://github.com/srk-sharingan/sharingan-rn-modal-dropdown)**
```sh
npm i sharingan-rn-modal-dropdown
```


## Icons

Using [react-native-vector-icons](https://github.com/oblador/react-native-vector-icons) to include icons in your project
```sh
npm install react-native-vector-icons
```

Change the file `android/app/build.gradle` and add:
```conf
project.ext.vectoricons = [
    iconFontNames: [ 'MaterialIcons.ttf', 'EvilIcons.ttf' ] // Name of the font files you want to copy
]
apply from: "../../node_modules/react-native-vector-icons/fonts.gradle"
```

For iOS, do this and read this [doc](https://medium.com/@vimniky/how-to-use-vector-icons-in-your-react-native-project-8212ac6a8f06):
```sh
# Add to Podfile and run pod update:
pod 'RNVectorIcons', :path => '../node_modules/react-native-vector-icons'

# Run
cd ios
pod install
# Just do update if your are sure, this can break the expected pods version for your react native
#pod update
```
### Internacionalization

```sh
npm i react-i18next
npm i react-native-localize
```

An example how to use the translate:

```jsx
import { Trans, useTranslation } from "react-i18next";

// Simple translate
 const { t } = useTranslation();
 <Text>{t('areas')}</Text>


// Complex translate
//Create a key in your translate
"areas-main": "Áreas {{count}}"
// Call your translate passing the variable
<Trans count={count}>areas-main</Trans>
```


## Navigation

This is a step by step comprehensive React Native Navigation v5 tutorial. In this tutorial, we will learn how to implement Stack Navigation in React Native app using Stack Navigator.

* https://reactnavigation.org/docs/getting-started/
* [React Native Navigation v5 Example Tutorial](https://www.positronx.io/react-native-navigation-example-tutorial/)
* [Passing & Getting Params to Screen](https://www.positronx.io/react-native-stack-navigator-passing-getting-params-to-screen/)


## Utils

* https://gist.github.com/sibelius/60a4e11da1f826b8d60dc3975a1ac805

# Expo

* Clone repo and run  `npm install`
* Download and install [Android Studio](https://developer.android.com/studio)

To update for a new SQK:
* expo update 39.0.0

