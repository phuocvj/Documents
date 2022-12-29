
NodeJs Download: [Click Here To Download](https://nodejs.org/download/release/v16.3.0/)

	> **npm i -g expo-cli**

	> **npx create-expo-app my-app**

	> **cd my-app**

	> **npx expo start**


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

	npm install -g eas-cli

	eas login

	 > You can check whether you are logged in by running `eas whoami`.
	 
  	eas build:configure

	eas build --platform android | ios | all

	Wait for the build to complete


# Submitting to the Apple App Store
eas submit --platform android | ios | all
More: [More Infor](https://docs.expo.dev/submit/introduction/)
