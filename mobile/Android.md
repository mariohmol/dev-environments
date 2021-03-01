# Android

Environment:
```sh
export JAVA_HOME=/Applications/Android\ Studio.app/Contents/jre/jdk/Contents/Home
export ANDROID_SDK_ROOT=/Users/username/Library/Android/sdk
```
## Run Device

Open you Android mobile, go to the Settings, go to About and click 10 time on the Build version. 
It will show a message like Developer Mode activated.

Then run `npm run android`
## App Icons

You need to replace current icons in mipmap folder named: `android/app/src/main/res/mipmap*`
There you will see files like `ic_launcher` and `ic_launcher_round`, which you can find the same in `AndroidManifest.xml` (check application tag: ‘ic’ in ‘ic_launcher’ stands for ‘icon’)

Generate the icons here https://appicon.co/ and paste the `mipmap`folders into `android/app/src/main/res/`


## App Key

**Create Key Store**
Find the Java path running: `/usr/libexec/java_home`

It will output the directory of the JDK, which will look something like this:
`/Library/Java/JavaVirtualMachines/jdk-15.0.1.jdk/Contents/Home`

Navigate to that directory by using the command $ cd /your/jdk/path and use the keytool command with sudo permission as shown below.

```sh
cd /Library/Java/JavaVirtualMachines/jdk-15.0.1.jdk/Contents/Home
sudo keytool -genkey -v -keystore my-upload-key.keystore -alias my-key-alias -keyalg RSA -keysize 2048 -validity 10000
```

Backup the keystore generated: `cp /Library/Java/JavaVirtualMachines/jdk-15.0.1.jdk/Contents/Home/my-upload-key.keystore ./android/app`



**Create the Private Key**
```sh
java -jar pepk.jar --keystore=appname.keystore --alias=appname --output=encrypted_private_vp --encryptionkey=KEY_HASH_FROM_GOOGLE_CONSOLE
```

This will ask one password wirh 6 chars, save this in a safe place and lets call that `keypass`.

If you need to upgrade the key, in this example the `appname.keystore` its the new key, the `uploadkey.keystore` its the oldkey, and the `appname.zip` will be required by Google Sign dashboard:
```sh
java -jar pepk.jar --keystore=appname.keystore --alias=appname --output=appname.zip  --signing-keystore=uploadkey.keystore --signing-key-alias=upload-key-alias --encryptionkey=KEY_HASH_FROM_GOOGLE_CONSOLE
```

**Setting up Gradle variables**

Edit the file `android/gradle.properties`, and add the following (replace ***** with the correct keystore password, alias and key password),
```conf
MYAPP_UPLOAD_STORE_FILE=my-upload-key.keystore
MYAPP_UPLOAD_KEY_ALIAS=my-key-alias
MYAPP_UPLOAD_STORE_PASSWORD=keypass
MYAPP_UPLOAD_KEY_PASSWORD=keypass
```


**Adding signing config to your app's Gradle config**
The last configuration step that needs to be done is to setup release builds to be signed using upload key. Edit the file `android/app/build.gradle` in your project folder, and add the signing config,

```conf
...
android {
    ...
    defaultConfig { ... }
    signingConfigs {
        release {
            if (project.hasProperty('MYAPP_UPLOAD_STORE_FILE')) {
                storeFile file(MYAPP_UPLOAD_STORE_FILE)
                storePassword MYAPP_UPLOAD_STORE_PASSWORD
                keyAlias MYAPP_UPLOAD_KEY_ALIAS
                keyPassword MYAPP_UPLOAD_KEY_PASSWORD
            }
        }
    }
    buildTypes {
        release {
            ...
            signingConfig signingConfigs.release
        }
    }
}
...
```

## Deploy

Open the `android/app/build.gradle` and increment the versions:
```
...
    defaultConfig {
        ...
        versionCode 2
        versionName "1.2"
    }
...
```
https://reactnative.dev/docs/signed-apk-android
ls android/app/build/outputs/bundle/release/app-release.aab  

## Throubleshoot

Error: Compatible side by side NDK version was not found.
Solution: Android Studio > Settings > Android SDK > SDK Tools:
* NDK (Side by Side) (Add or Remove the NDK)
* CMake (Add)

Error: No toolchains found in the NDK toolchains folder for ABI with prefix: arm-linux-androideabi
Solution: Remove the NDK from the SDK Tools

Error: duplicated resources
Solution: Clean some resources files
```sh
rm -rf android/app/src/main/res/drawable*
rm -rf android/app/src/main/res/raw*
```


# React Native Android

Exportar e fazer upload de uma chave de um keystore Java
Faça o download da ferramenta Play Encrypt Private Key (PEPK). Faça o download do código-fonte
Ative a ferramenta com o comando abaixo para exportar e criptografar sua chave privada. Substitua os argumentos e insira as senhas do keystore e da chave quando solicitado.

