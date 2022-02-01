How to create an development enviroment at windows.

First enable to run scripts, open the Powershell as an Administrator and run:
`set-executionpolicy remotesigned`

# Node

Install node for windows from [Nodejs downloads](https://nodejs.org/en/download/).

**Configure the path for npm packages**
Hit Windows key and search for Environment variables. After open the window, click in the button Environment Variables.
Inside the Path variable under System variables, add a new entry with this content (without /node_modules ):
%USERPROFILE%\AppData\Roaming\npm
You can open the terminal and teste your environment using `echo $PATH`. 

# SSH

Download and install [Putty](https://www.putty.org/) for ssh connections and choose 64-bit version.

Open the windows terminal ( Windows key + prompt ) and run: `ssh-keygen`. During this process you will be asked for questions, just press enter until de end.

He will tell you where the key will be created. It's not necessary to put pasphrase.
This will give you an folder like `~/.ssh/id_rsa.pub`.

Copy the content of this file and paste in your server or any other app.


# Development

**PowerShell**
If you have problems running scripts on powershell/vscode, you have to giver permissions for your user.
Find powershell in windows search, right click, and open as an administrator. Then run this command:
`Set-ExecutionPolicy RemoteSigned -Scope LocalMachine`

**VSCode**
Install the Code Editor for many languages , [VSCode](https://code.visualstudio.com/Download).

You can run VSCode from the command line. Find for Edit Environment Variables, edit Path and add the folder directory where that was installed, for example: `C:\users\{username}\AppData\Local\Programs\Microsoft VS Code`

**Git**
To have access to the source files for the project you need to use [Git](https://en.wikipedia.org/wiki/Git).
You can use Git online, creating an account on [ Gitub](http://github.com/) and/or [BitBucket](http://bitbucket.com/).

To avoid problems with git and file configs, use:
`git config --global core.autocrlf true`

Install [SourceTree](https://www.sourcetreeapp.com/) as a user interface to manage your source with git.
During install it asks for an bitbucket account, you can skip if you want. 
After install, click in Remote repo, add an Account, choose Github and make the Oauth with your user.

**Bash**
Install [git bash](https://gitforwindows.org/) for use linux style commands on windows.


# Databases

**Mongo**
Install [mongo](https://www.mongodb.com/try/download/community?tck=docs_server) and 
to manage the data install [Robot 3T](https://robomongo.org/download).

**MySQL**
Install [mysql](https://dev.mysql.com/downloads/installer/) and 
to manage data use [Dbeaver](https://dbeaver.io/download/)

**Postgres**
Install [postgres](https://www.postgresql.org/download/windows/) and 
to manage data use [Dbeaver](https://dbeaver.io/download/)
# Mobile

## Android

Install [Java JDK](https://www.oracle.com/java/technologies/javase/javase-jdk8-downloads.html)

Install [android studio](https://developer.android.com/studio/#downloads) for windows.
 
After that download the [platform-tools](https://dl.google.com/android/repository/platform-tools-latest-windows.zip)
and unzip into `~/Documents` folder. 

Run this in your terminal:
sdkmanager "platform-tools" "platforms;android-28"

For full information check this [doc](https://techcult.com/wiki/how-to-install-adb-android-debug-bridge-on-windows-10/#Method_5_%E2%80%93_Add_ADB_to_System_Path)


Hit Windows key and search for Environment variables. After open the window, click in the button Environment Variables.
Inside the Path variable under System variables, add a variable called `ANDROID_SDK_HOME` with value `c:\Users\<username>\.android`.


Create a file called `.bash_profile` inside your users folder `c:\Users\<username>\`
```sh
export JAVA_HOME="c:\Program Files\java\jdk-1.5.2"
export ANDROID_SDK_HOME="c:\users\<username\.android"
export ANDROID_SDK_ROOT="c:\Users\<username>\AppData\Local\Android\Sdk"
export "PATH=$PATH:c:\Users\<username>\documents\platrform-tools-windows\platform-tools"
```

Android Settings -> Developers Settings -> USB depuration and activate

adb devices

should list

### Emulators

You can check the [ Android Doc]() for more info about it, but you might end to an error when trying to load an emulator.
If you find the error `The emulator process for AVD ... was killed`, try to..

## Expo

If you are using the expo for react-native client 
`npm install --global expo-cli`


## Ruby

Install Ruby, we tested using 2.6.x:
* https://rubyinstaller.org/