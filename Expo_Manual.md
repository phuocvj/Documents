# Expo Create amazing apps that run everywhere
﻿
NodeJs Download: [Click Here To Download](https://nodejs.org/download/release/v16.3.0/)

 `1`  Installation
 
	 npm i -g expo-cli
 `2` Create a new app
 
	 npx create-expo-app my-app
	 
`You can also use the --template option with the create-expo-app command. Run npx create-expo-app --template to see the list of available templates.`

`3` In Your Project

	 cd my-app
	

`4` Start

	 npx expo start


`Command:`

    Press a │ open Android
    › Press w │ open web
    › Press r │ reload app
    › Press m │ toggle menu
    › Press d │ show developer tools
    › shift+d │ toggle auto opening developer tools on startup (enabled)

If you are using a simulator or emulator, you may find the following Expo CLI keyboard shortcuts to be useful to open the app on any of the following platforms:

-   Pressing  a  will open in an  [Android Emulator or connected device](https://docs.expo.dev/workflow/android-studio-emulator/).
-   Pressing  i  will open in an  [iOS Simulator](https://docs.expo.dev/workflow/ios-simulator/).
-   Pressing  w  will open in a web browser. Expo supports all major browsers.
More: [More Info Here](https://docs.expo.dev/get-started/create-a-new-app/)
---
# Build App


`1` Install the latest EAS CLI

	npm install -g eas-cli

`2` Log in to your Expo account

	eas login


`3` Configure the project

	eas build:configure

`4` Run a build

	eas build --platform android | ios | all

`5` Wating a build

	Wait for the build to complete


# Submitting to the Apple App Store

	eas submit --platform android | ios | all
	
More: [More Infor](https://docs.expo.dev/submit/introduction/)
