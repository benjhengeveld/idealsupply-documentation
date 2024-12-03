# Capacitor Project Setup

This guide was made using 
- [Node.js LTS](https://nodejs.org/en) version 22.3.0
- [Angular CLI](https://github.com/angular/angular-cli) version 18.0.4
- [Capacitor](https://capacitorjs.com/) version 6.1.0
- [Android Studio](https://developer.android.com/studio) Koala version 2024.1.1
- [Java JDK](https://www.oracle.com/java/technologies/downloads/#jdk17-windows) version 17


## Android Studio Setup

1. Download and install [Android Studio](https://developer.android.com/studio)

2. Open Android Studio's SDK manager from `Tools -> SDK Manager` or if on the welcome screen `More Actions -> SDK Manager`

3. Select the check box beside the platform versions you'd like to test with

4. Press `OK` and then `OK` to confirm the change and wait for the SDKs to download

5. Add an environment variable of name `ANDROID_HOME` and value `%USERPROFILE%\AppData\Local\Android\Sdk` to your user variables

6. Logout and back into Windows to have the environment variable update


## Project Setup

1. Make an Angular project

2. Open a terminal in the projects folder and run this command

    ```cmd
    pnpm add @capacitor/core @capacitor/app
    pnpm add -D @capacitor/cli archiver
    npx cap init
    ```

3. Open a terminal in the projects folder and run this command

    ```cmd
    pnpm i @capacitor/android
    npx cap add android
    ```

4. Edit the `webDir` var in the `capacitor.config.ts` to be `dist/project-name/browser`

5. Edit the `appName` var in the `capacitor.config.ts` to be the name you want the app to have

6. Add the scripts from the [Scripts](#Scripts) section

7. Open the `project-name\package.json` and add this to the scripts list
    
    ```json
    "build:prod": "ng build --configuration production",
    "sync": "pnpm run build:prod && npx cap sync",
    "build:update": "node ./scripts/create-android-update.js",
    "build:android": "pnpm run sync && node ./scripts/update-android-version.js && node ./scripts/build-android.js build && node ./scripts/copy-apk-file.js && pnpm build:update",
    "build:android:release": "pnpm run sync && node ./scripts/update-android-version.js && node ./scripts/build-android.js assembleRelease && node ./scripts/copy-apk-file.js assembleRelease && pnpm build:update",
    "build:android:debug": "pnpm run sync && node ./scripts/update-android-version.js && node ./scripts/build-android.js assembleDebug && node ./scripts/copy-apk-file.js assembleDebug && pnpm build:update"
    ```

8. Open a terminal in the projects folder and run this command

    ```cmd
    pnpm run build:android
    ```
9. Add `/builds` to the `.gitignore` file

10. Create a file named `validate-build.yml` under `.github/workflows/` and set the contents to this 

    ```yml
    name: Validate build
    
    on:
      push:
        branches-ignore:
          - release
    
    jobs:
      validate-angular-build:
        runs-on: ubuntu-latest
    
        steps:
          - name: Checkout code
            uses: actions/checkout@v4
    
          - name: Setup node and pnpm
            uses: pnpm/action-setup@v3
            with:
              version: 8
    
          - name: Install Angular CLI
            run: pnpm i -g @angular/cli
    
          - name: Install project dependencies
            run: pnpm i
    
          - name: Build project
            run: pnpm run build:prod
    
      validate-android-build:
        runs-on: ubuntu-latest
    
        steps:
          - name: Checkout code
            uses: actions/checkout@v4
    
          - name: Setup node and pnpm
            uses: pnpm/action-setup@v3
            with:
              version: 8
    
          - name: Install Angular CLI
            run: pnpm i -g @angular/cli
    
          - name: Install project dependencies
            run: pnpm i
    
          - name: Setup Java
            uses: actions/setup-java@v4
            with:
              distribution: "adopt"
              java-version: "17"
    
          - name: Set execute permissions for Gradle wrapper
            run: chmod +x android/gradlew
    
          - name: Decrypt Keystore
            run: gpg --quiet --batch --yes --decrypt --passphrase="$KEYSTORE_ENCRYPT_PASSWORD" --output android/app/release-key.keystore android/app/release-key.keystore.gpg
            env:
              KEYSTORE_ENCRYPT_PASSWORD: ${{ secrets.KEYSTORE_ENCRYPT_PASSWORD }}
    
          - name: Build APK
            run: pnpm run build:android
            env:
              KEYSTORE_PASSWORD: ${{ secrets.KEYSTORE_PASSWORD }}
    ```


## Fix back button

Open the `src\app\app.component.ts` and update the file to be this

```ts
import { Component, OnInit } from '@angular/core';
import { RouterOutlet } from '@angular/router';
import { BackButtonListenerEvent, App as CapacitorApp } from '@capacitor/app';

@Component({
  selector: 'app-root',
  standalone: true,
  imports: [RouterOutlet],
  templateUrl: './app.component.html',
  styleUrl: './app.component.scss',
})
export class AppComponent implements OnInit {
  title = 'project-name';

  ngOnInit(): void {
    this.listenToBackButton();
  }

  private listenToBackButton(): void {
    CapacitorApp.addListener('backButton', (state: BackButtonListenerEvent) => {
      if (!state.canGoBack) {
        CapacitorApp.exitApp();
      } else {
        window.history.back();
      }
    });
  }
}
```


## Adding a Keystore

1. Add `*.keystore` to the `.gitignore` file

2. Open a terminal in the projects folder and run this command. You will have to enter a password, make sure to write it down
    - Replace `my-key-alias` with what you want you want your keys alias to be. For example `ideal_tradeshow_customer_app`
    - You might have to add your Java bin to your environment path. For example `C:\Program Files\Java\jdk-17\bin`. Then logout and back into Windows to have the environment variable update
    ```cmd
    keytool -genkeypair -v -keystore android\app\release-key.keystore -alias my-key-alias -keyalg RSA -keysize 2048 -validity 10000
    ```

3. Open a terminal in the projects folder and run this command
    - Replace `example-password` with a password used to encrypt/decrypt the keystore file. This password should NOT be the same as the keystore's password. Make sure to also write down this password
    - If you are on Windows you will to install [Gpg4win](https://gpg4win.org/download.html) and add `C:\Program Files (x86)\GnuPG\bin` to your environment path. Then logout and back into Windows to have the environment variable update
    ```cmd
    gpg --quiet --batch --yes --symmetric --passphrase="example-password" --output android\app\release-key.keystore.gpg android\app\release-key.keystore
    ```

4. On the projects Github repo go to `Settings -> Secrets and variables -> Actions` and add these secrets
    - Name: `KEYSTORE_PASSWORD` | Secret: The password used to make the keystore file from step 2
    - Name: `KEYSTORE_ENCRYPT_PASSWORD` | Secret: The password used to encrypt/decrypt the keystore file from step 3

5. Open the `android\app\build.gradle` and add this above the defaultConfig
    - Replace `my-key-alias` with what you want you set your keys alias to
    ```gradle
    signingConfigs {
        release {
            keyAlias 'my-key-alias'
            storeFile file('release-key.keystore')
            keyPassword System.getenv("KEYSTORE_PASSWORD")
            storePassword System.getenv("KEYSTORE_PASSWORD")
        }
    }
    ```
    
6. Open the `android\app\build.gradle` and add this in the buildTypes release above the minifyEnabled false
    ```gradle
    signingConfig signingConfigs.release
    ```

7. Create a file named `release-build.yml` under `.github/workflows/` and set the contents to this

    ```yml
    name: Release build

    on:
      push:
        branches:
          - release
    
    permissions:
      contents: write
    
    jobs:
      release-android-build:
        runs-on: ubuntu-latest
    
        steps:
          - name: Checkout code
            uses: actions/checkout@v4
    
          - name: Setup node and pnpm
            uses: pnpm/action-setup@v3
            with:
              version: 8
    
          - name: Install Angular CLI
            run: pnpm i -g @angular/cli
    
          - name: Install project dependencies
            run: pnpm i
    
          - name: Setup Java
            uses: actions/setup-java@v4
            with:
              distribution: "adopt"
              java-version: "17"
    
          - name: Set execute permissions for Gradle wrapper
            run: chmod +x android/gradlew
    
          - name: Decrypt Keystore
            run: gpg --quiet --batch --yes --decrypt --passphrase="$KEYSTORE_ENCRYPT_PASSWORD" --output android/app/release-key.keystore android/app/release-key.keystore.gpg
            env:
              KEYSTORE_ENCRYPT_PASSWORD: ${{ secrets.KEYSTORE_ENCRYPT_PASSWORD }}
    
          - name: Build APK
            run: pnpm run build:android
            env:
              KEYSTORE_PASSWORD: ${{ secrets.KEYSTORE_PASSWORD }}
    
          - name: Get npm version
            id: package-version
            uses: martinbeentjes/npm-get-version-action@master
    
          - name: Create Release
            uses: ncipollo/release-action@v1
            with:
              tag: v${{ steps.package-version.outputs.current-version }}
              name: Version ${{ steps.package-version.outputs.current-version }}
              commit: ${{ github.sha }}
              draft: false
              artifacts: "builds/android/*.apk,builds/update/*.zip"
    ```

## Scripts

### utils/android-utils.js
```js
const fs = require("fs");

const updateAndroidVersion = async (version) => {
  const fileName = "./android/app/build.gradle";
  const replaceString = `versionName "${version}"`;
  const findString = /versionName "[\d|\.]+"/g;

  try {
    let data = await fs.promises.readFile(fileName, { encoding: "utf8" });
    const modifiedContent = data.replace(findString, replaceString);

    await fs.promises.writeFile(fileName, modifiedContent, {
      encoding: "utf8",
    });
    console.log(`Android has been updated to version ${version}`);
  } catch (error) {
    console.error(`Error reading or writing to file: ${error.message}`);
    process.exit(1);
  }
};

const updateAndroidName = async (name) => {
  const fileName = "./android/app/build.gradle";
  const replaceString = `archivesBaseName = "${name}-v" + versionName`;
  const findString = /archivesBaseName = .+/g;

  try {
    let data = await fs.promises.readFile(fileName, { encoding: "utf8" });

    if (findString.test(data)) {
      const modifiedContent = data.replace(findString, replaceString);

      await fs.promises.writeFile(fileName, modifiedContent, {
        encoding: "utf8",
      });
      console.log(`Android has been updated to name ${name}`);
    } else {
      const findVersionString = /versionName "[\d|\.]+"/g;
      const modifiedContent = data.replace(
        findVersionString,
        (match) => `${match}\n        ${replaceString}`
      );

      await fs.promises.writeFile(fileName, modifiedContent, {
        encoding: "utf8",
      });
      console.log(`Android has been updated to name ${name}`);
    }
  } catch (error) {
    console.error(`Error reading or writing to file: ${error.message}`);
    process.exit(1);
  }
};

module.exports = {
  updateAndroidVersion,
  updateAndroidName,
};
```

### utils/file-utils.js
```js
const fs = require("fs");
const path = require("path");
const archiver = require("archiver");
const packageJson = require("../../package.json");

const getPackageVersion = () => {
  try {
    return packageJson.version;
  } catch (error) {
    console.error("Error reading package.json:", error);
    return null;
  }
};

const getPackageName = () => {
  try {
    return packageJson.name;
  } catch (error) {
    console.error("Error reading package.json:", error);
    return null;
  }
};

const createFolder = async (folder) => {
  try {
    if (!fs.existsSync(folder)) {
      await fs.promises.mkdir(folder, { recursive: true });
      console.log(`Output folder created: ${folder}`);
    }
  } catch (error) {
    console.error(`Error creating output folders: ${error.message}`);
    process.exit(1);
  }
};

const deleteFileType = async (folder, fileType) => {
  try {
    const files = await fs.promises.readdir(folder);
    const filesWithType = files.filter(
      (file) => path.extname(file) === fileType
    );

    if (filesWithType.length === 0) {
      return;
    }

    await Promise.all(
      filesWithType.map(async (fileWithType) => {
        const filePath = path.join(folder, fileWithType);
        try {
          await fs.promises.unlink(filePath);
          console.log(`Deleted ${fileWithType}`);
        } catch (unlinkErr) {
          console.error(`Error deleting ${fileWithType}: ${unlinkErr.message}`);
        }
      })
    );
  } catch (err) {
    console.error(`Error reading folder: ${err.message}`);
  }
};

const copyFileType = async (outputPath, sourceFolder, fileType) => {
  try {
    const files = await fs.promises.readdir(sourceFolder);
    const fileWithType = files.find((file) => path.extname(file) === fileType);

    if (!fileWithType) {
      console.log(`No ${fileType} files found in the source folder.`);
      return;
    }

    const sourcePath = path.join(sourceFolder, fileWithType);
    const destinationPath = path.join(outputPath, fileWithType);

    try {
      const data = await fs.promises.readFile(sourcePath);

      try {
        await fs.promises.writeFile(destinationPath, data);
        console.log(`Copied ${fileWithType} to ${outputPath}`);
      } catch (err) {
        console.error(`Error writing file: ${err.message}`);
      }
    } catch (err) {
      console.error(`Error reading file: ${err.message}`);
    }
  } catch (err) {
    console.error(`Error reading folder: ${err.message}`);
  }
};

const createZipFile = (fileName, inputPath, outputPath) => {
  const outputZipPath = `${outputPath}/${fileName}.zip`;

  const output = fs.createWriteStream(outputZipPath);

  const archive = archiver("zip", {
    zlib: { level: 9 },
  });

  archive.pipe(output);

  archive.directory(inputPath, false);

  archive.finalize();

  output.on("close", () => {
    console.log(`Successfully created ${outputZipPath}`);
  });

  archive.on("error", (err) => {
    console.error("Error during zip:", err);
  });
};

module.exports = {
  getPackageVersion,
  getPackageName,
  createFolder,
  deleteFileType,
  copyFileType,
  createZipFile,
};
```

### build-android.js
```js
const { execSync } = require("child_process");
const os = require("os");

const buildMode = process.argv[2] ?? "build";

const buildDebug = () => {
  try {
    const gradlewCommand =
      os.platform() === "win32" ? "gradlew.bat" : "./gradlew";
    const command = `${gradlewCommand} ${buildMode}`;
    execSync(command, { cwd: "./android", stdio: "inherit" });
  } catch (error) {
    console.error("Error occurred during building debug APK:", error);
    process.exit(1);
  }
};

buildDebug();
```

### copy-apk-file.js
```js
const {
  createFolder,
  deleteFileType,
  copyFileType,
} = require("./utils/file-utils");

const buildMode = process.argv[2] ?? "build";

const moveApkFile = async () => {
  const fileType = ".apk";
  const outputFolder = "./builds/android";
  const debugSourceFolder = "./android/app/build/outputs/apk/debug";
  const releaseSourceFolder = "./android/app/build/outputs/apk/release";

  await createFolder(outputFolder);
  await deleteFileType(outputFolder, fileType);

  if (buildMode !== "assembleRelease") {
    await copyFileType(outputFolder, debugSourceFolder, fileType);
  }

  if (buildMode !== "assembleDebug") {
    await copyFileType(outputFolder, releaseSourceFolder, fileType);
  }
};

(async () => {
  await moveApkFile();
})();
```

### create-android-update.js
```js
const {
  createFolder,
  deleteFileType,
  createZipFile,
  getPackageVersion,
  getPackageName,
} = require("./utils/file-utils");

const createUpdateZip = async () => {
  const fileType = ".zip";
  const outputFolder = "./builds/update";
  const packageVersion = getPackageVersion();
  const packageName = getPackageName();
  const distPath = `./dist/${packageName}/browser`;
  const fileName = `${packageName}-v${packageVersion}`;

  await createFolder(outputFolder);
  await deleteFileType(outputFolder, fileType);
  createZipFile(fileName, distPath, outputFolder);
};

(async () => {
  await createUpdateZip();
})();
```

### update-android-version.js
```js
const { getPackageVersion, getPackageName } = require("./utils/file-utils");
const {
  updateAndroidVersion,
  updateAndroidName,
} = require("./utils/android-utils");

const updateNameAndVersion = async () => {
  const packageVersion = getPackageVersion();
  const packageName = getPackageName();

  if (packageVersion) {
    await updateAndroidVersion(packageVersion);
  } else {
    console.log(`Failed to get package.json version`);
    process.exit(1);
  }

  if (packageName) {
    await updateAndroidName(packageName);
  } else {
    console.log(`Failed to get package.json name`);
    process.exit(1);
  }
};

(async () => {
  await updateNameAndVersion();
})();
```
