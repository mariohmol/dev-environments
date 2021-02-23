


# Mobile

## Android

Install [Java JDK](https://www.oracle.com/java/technologies/javase/javase-jdk8-downloads.html)

Install [android studio](https://developer.android.com/studio/#downloads) for windows.
 
After that download the [platform-tools](https://dl.google.com/android/repository/platform-tools-latest-windows.zip)
and unzip into `~/Documents` folder.

Add this path to the 

https://techcult.com/wiki/how-to-install-adb-android-debug-bridge-on-windows-10/#Method_5_%E2%80%93_Add_ADB_to_System_Path

## Expo

`npm install --global expo-cli`
# Node

Install node for windows from [Nodejs downloads](https://nodejs.org/en/download/).

## Npm

Configure the path for npm packages.

Hit Windows key and search for Environment variables.
Inside the Path variable under System variables, add a new entry with this content (without /node_modules ):
%USERPROFILE%\AppData\Roaming\npm

## Environment Variable

You can open the terminal and teste your environment using `echo $PATH`. 
To open the Env editor, Hit Windows key and search for Environment variables.
You can edit the User or system environment variables.

# SSH


Open the windows terminal and run: `ssh-keygen`.

He will tell you where the key will be created. It's not necessary to put pasphrase.
This will give you an folder like `~/.ssh/id_rsa.pub`.

Copy the content of this file and paste in your server or any other app.



