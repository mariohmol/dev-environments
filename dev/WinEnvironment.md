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

Download and install [Putty](https://www.putty.org/) for ssh connections and choose 64-bit version.

Open the windows terminal ( Windows key + prompt ) and run: `ssh-keygen`. During this process you will be asked for questions, just press enter until de end.

He will tell you where the key will be created. It's not necessary to put pasphrase.
This will give you an folder like `~/.ssh/id_rsa.pub`.

Copy the content of this file and paste in your server or any other app.


# Development

**VSCode**
Install the Code Editor for many languages , [VSCode](https://code.visualstudio.com/Download).

**Git**
To have access to the source files for the project you need to use [Git](https://en.wikipedia.org/wiki/Git).
You can use Git online, creating an account on [ Gitub](http://github.com/) and/or [BitBucket](http://bitbucket.com/).

Install [SourceTree](https://www.sourcetreeapp.com/) as a user interface to manage your source with git.

During install he asks for an bitbucket account, you can skip if you want. 

After instal, click in Remote repo, add an Account, choose Github and make the Oauth with your user.


# Databases

**Mongo**
Install [mongo](https://www.mongodb.com/try/download/community?tck=docs_server) and 
to manage the data install [Robot 3T](https://robomongo.org/download).

**MySQL**
Install [mysql](https://dev.mysql.com/downloads/installer/) and 
to manage data use [Dbeaver]
# Mobile

## Android

Install [Java JDK](https://www.oracle.com/java/technologies/javase/javase-jdk8-downloads.html)

Install [android studio](https://developer.android.com/studio/#downloads) for windows.
 
After that download the [platform-tools](https://dl.google.com/android/repository/platform-tools-latest-windows.zip)
and unzip into `~/Documents` folder.

Add this path to the 

https://techcult.com/wiki/how-to-install-adb-android-debug-bridge-on-windows-10/#Method_5_%E2%80%93_Add_ADB_to_System_Path

## Expo

If you are using the expo for react-native client 
`npm install --global expo-cli`
