# Cordova Project Setup

## Use [Capacitor Setup](https://gist.github.com/benjhengeveld/91e34337ae543326cb069b468d9ee430). Dont use Cordova any more.

1. Open `project-name\package.json` and make sure the version is `0.0.1` or above

2. Open `project-name\package.json` and replace this line
    ```json
    "build": "ng build",
    ```
with this line

    ```json
    "build": "ng build --configuration production --base-href ./ --output-path ./mobile/www/ && cd ./mobile && sed -i \"s/version=\\\"[^\\\"]*\\\"/version=\\\"${npm_package_version}\\\"/\" config.xml && cordova platform rm android && cordova platform add android && cordova build android --prod && mkdir -p apk && cp ./platforms/android/app/build/outputs/apk/debug/app-debug.apk ./apk/${npm_package_name}.apk && cd ..",
    ```


3. Open `project-name\package.json` and add this into the scripts list
    ```json
    "setup-cordova": "cordova create mobile && cd ./mobile/ && cordova platform add android && cd ..",
    "serve-chrome": "ng build --configuration production --base-href ./ --output-path ./mobile/www/ && cd ./mobile && sed -i \"s/version=\\\"[^\\\"]*\\\"/version=\\\"${npm_package_version}\\\"/\" config.xml && cordova platform remove browser && cordova platform add browser && cordova run browser && cd ..",
    ```

4. Open a vscode terminal in the projects folder and run this command
    ```cmd
    yarn setup-cordova
    ```

5. Open `project-name\src\index.html` and add this to the head
    ```html
    <script type="text/javascript" src="cordova.js"></script>
    ```

6. Open `project-name\src\main.ts` and replace this code
    ```ts
    platformBrowserDynamic().bootstrapModule(AppModule)
      .catch(err => console.error(err));
    ```
with this

    ```ts
    function bootstrap(): void {
      platformBrowserDynamic()
        .bootstrapModule(AppModule)
        .catch((err) => console.error(err));
    }
    if (window.hasOwnProperty('cordova')) {
      document.addEventListener('deviceready', () => bootstrap());
    } else {
      bootstrap();
    }
    ```

7. Open a vscode terminal in the projects folder and run this command
    ```cmd
    yarn add @types/cordova
    ```

8. Open `project-name\tsconfig.app.json` and add this to the types list in the compilerOptions
    ```json
    "cordova"
    ```

9. Open `project-name\mobile\config.xml` and add this inside the widget tag
    ```xml
    xmlns:android="http://schemas.android.com/apk/res/android"
    ```

10. Open `project-name\mobile\config.xml` and add this in the widget tag. Replace name-of-app with the name of the app. 
    ```xml
    <access origin="*" subdomains="true" />
    <allow-navigation href="http://*/*" />
    <allow-navigation href="https://*/*" />
    <allow-intent href="http://*/*" />
    <allow-intent href="https://*/*" />
    <access origin="https://*.idealsupply.com" />
    <preference name="hostname" value="name-of-app.idealsupply.com" />
    ```

11. Open `project-name\mobile\config.xml` and change the widget id, name, description, and author

12. Open a vscode terminal in the projects folder and run this command
    ```cmd
    yarn build
    ```



## Changing the app icon
This changes the icon of the app you see on the device. It sould be a square png image
1. Put your icon as a png in `project-name\mobile` and make sure the name of the image is `icon.png`

2. Add this line to the `project-name\mobile\config.xml` inside the widget tags
    ```xml
    <icon src="icon.png" />
    ```



## Changing the app name
This changes the name of the appl you see on the device
1. Open `project-name\mobile\config.xml` and change the text in the name tag



## Changing the app version
This changes the name of the appl you see on the device
1. Open `project-name\package.json` and change the text in the version

2. Open a vscode terminal in the projects folder and run this command
    ```cmd
    yarn build
    ```



## Building the Cordova apk
This builds the apk that can be installed onto an android device
1. Open a vscode terminal in the projects folder and run this command
    ```cmd
    yarn build
    ```

2. The apk can be found in `project-name\mobile\apk` (You might have to reload the vscode explorer)



## Serving the Cordova app to Chrome
Serve the cordova app to Chrome lets you test Cordova specific features using proxys for things Chrome cannot use like the camera
1. Open a vscode terminal in the projects folder and run this command<br><br>
    ```cmd
    yarn serve-chrome
    ```



## Adding a Cordova plugin
A Cordova plugin lets you use the hardware of the device like the camera and storage
1. Open a vscode terminal in the projects folder and run this command if you have not already
    ```cmd
    yarn add @awesome-cordova-plugins/core
    ```

2. Open a vscode terminal in the projects folder and run this command. Replace plugin-name with the name of the plugin
    ```cmd
    yarn add @awesome-cordova-plugins/plugin-name
    ```

3. Open a vscode terminal in `project-name\mobile` and run this command. Replace plugin-name with the name of the plugin
    ```cmd
    cordova plugin add cordova-plugin-plugin-name
    ```

4. Open `project-name\srv\app\app.module.ts` and add this to the top. Replace plugin-name with the name of the plugin. Replace PluginName with the name of the plugin
    ```ts
    import { PluginName } from '@awesome-cordova-plugins/plugin-name/ngx';
    ```

5. Open `project-name\srv\app\app.module.ts` and add this to the providers list. Replace PluginName with the name of the plugin
    ```ts
    PluginName
    ```



## Plugins

https://danielsogl.gitbook.io/awesome-cordova-plugins/

| Name                | Yarn Package                                  | Cordova Plugin                             |
| ------------------- | --------------------------------------------- | ------------------------------------------ |
| Android Permissions | @awesome-cordova-plugins/android-permissions  | cordova-plugin-android-permissions         |
| App Version         | @awesome-cordova-plugins/app-version          | cordova-plugin-app-version                 |
| Barcode Scanner     | @awesome-cordova-plugins/barcode-scanner      | phonegap-plugin-barcodescanner             |
| Dialogs             | @awesome-cordova-plugins/dialogs              | cordova-plugin-dialogs                     |
| File                | @awesome-cordova-plugins/file                 | cordova-plugin-file                        |
| File Transfer       | @awesome-cordova-plugins/file-transfer        | github:apache/cordova-plugin-file-transfer |
| Screen Orientation  | @awesome-cordova-plugins/screen-orientation   | cordova-plugin-screen-orientation          |
| Web Intent          | @awesome-cordova-plugins/web-intent           | github:IdealSupply/idealsupply-cordova-plugin-intent   |
| Device              | @awesome-cordova-plugins/device               | cordova-plugin-device                      |

<br>
