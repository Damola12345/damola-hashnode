---
title: "Automate Android Build Using GitHub Actions"
datePublished: Mon Sep 19 2022 11:23:02 GMT+0000 (Coordinated Universal Time)
cuid: cl88ohyeu01flv6nvcv07ep7g
slug: automate-android-build-using-github-actions
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1663534537579/pft6q4Rv-.png
tags: app-development, react-native, android, hashnode, 2articles1week

---

This article walks you through how to automate, build, and distribute new versions of Android build to Google play  store using GitHub Action 

## GitHub Action 

GitHub provides a workflow automation feature named GitHub Actions and provides virtual machines such as Linux, Windows, and macOS to run your workflows. 

![GH_Andriod](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/ulibtugyhzwnoiw813z8.jpeg)

[GitHub Actions](https://docs.github.com/en/actions) allows you to create workflows that can be used to compile, test, and deploy code. Furthermore, it  gives the possibility to create integration flows and continuous deployment within our repository. Actions use code  packages in Docker containers, which run on GitHub servers and which, in turn, are compatible with any  programming language. 

## Important Terms Of GitHub Actions 

**Workflow** is a sequence of jobs that can run either in series or in parallel. A job usually contains more than one step,  where each step is a self-contained function. For more information, on related workflows visit [using workflows](https://docs.github.com/en/actions/using-workflows).

**Events** are specific activities that trigger the workflow. Define them using the `on` key(creating a pull request or  pushing a commit to a repository). 

**Artifacts** are files like APKs, screenshots, test reports, logs, which the workflow generates. You can upload and download artifacts to the current workflow using [actions/upload-artifact@v2](https://github.com/actions/upload-artifact) and [actions/download-artifact@v2](https://github.com/actions/download-artifact) respectively.

**Jobs** are a  set of steps that execute on a fresh instance of a virtual environment. You can have multiple jobs and run them  sequentially or in parallel by defining their dependency rules. For example, If one step tests your application then  another step will be used to build the application which was tested. 

**Runners** are machines that execute jobs defined in the workflow file. GitHub hosts Linux, Windows, and macOS  runners with commonly used software pre-installed, but you can also host custom runners as well. Basically, these  are equivalent to containers or virtual machines. For hosting custom runners check out [hosting your own runners](https://docs.github.com/en/actions/hosting-your-own-runners). 

**Actions** are the smallest portable building blocks of a workflow, which you include as a step. The popular one is  [actions/checkout@v3](https://github.com/actions/checkout), which you use to check out the current repository. 

To learn more important concepts, visit the [official documentation](https://docs.github.com/en/actions/learn-github-actions/understanding-github-actions).

## Benefits of GitHub Actions 
- Develop in GitHub 
- Attractive free plan 
- Wide variety of CI templates 

## Creating a Workflow 

In GitHub, each workflow is defined in a YAML syntax and this YAML file is stored inside the `.gitHub/workflows/` directory at the root of the project. Then, create a file named `andriod.yml` in  the `.github/workflows/`. 

I advocate running build and test scripts on a local machine to make sure it works as expected to  prevent debugging issues on the CI and if it doesn’t make sure to fix all issues before going further. 

Run the following command from the command line to run unit tests:

```
./gradlew test 
```

Run the following command to generate APK 

```
cd android && ./gradlew assembleRelease
```
 

Run the following command to generate bundle 

```
cd android && ./gradlew bundleRelease
```
Now that you know how to run the tests and build from the command line, add the following code to `.github/workflows/andriod.yml` 

```
# name of the workflow
name: Android Build CI/CD 

on:

  push:
    branches: [ staging ]
  # pull_request:
    # branches: [ staging ]
    tags:
      - 'v*' 

jobs:
  android-build:
    # The type of runner that the job will run on    
    runs-on: ubuntu-latest
    # Steps represent a sequence of tasks that will be executed as part of the job
    steps:
    - uses: actions/checkout@v3
    - uses: actions/setup-node@v3
    
    # Automatically overrides the version code and version name through the github actions
    - name: Bump version
      uses: chkfung/android-version-actions@v1.1
      with:
        gradlePath: android/app/build.gradle 
        versionCode: ${{github.run_number}}

    - name: Install Dependencies
      run: yarn install

    - name: Run Unit Test
      run: ./gradlew test

    ## cache Gradle dependencies and wrapper to reduce build time
    - name: Cache Gradle Wrapper
      uses: actions/cache@v3
      with:
        path: ~/.gradle/wrapper
        key: ${{ runner.os }}-gradle-wrapper-${{ hashFiles('gradle/wrapper/gradle-wrapper.properties') }}

    - name: Cache Gradle Dependencies
      uses: actions/cache@v3
      with:
        path: ~/.gradle/caches
        key: ${{ runner.os }}-gradle-caches-${{ hashFiles('gradle/wrapper/gradle-wrapper.properties') }}
        restore-keys: |
          ${{ runner.os }}-gradle-caches-
    - name: Make Gradlew Executable
      run: cd android && chmod +x ./gradlew
```

The code above does a few things: 
- Creates a workflow named `Android Build CI/CD`. 
- Creates parallel jobs named `android-build` which runs on an ubuntu runner, checks out the code and runs the unit tests. 
- `Bump version code` Automatically overrides the version code and version name through github actions 
- Set up dependencies and requirements for our build, configuring cache for yarn and Gradle build system, and making the gradlew executable. 

Next, you’ll see how to generate a secure release build on a remote system. 

## Generate a Signed Release Build 

To sign your release build, you first need a Keystore. I presume you already own your KeyStore and If you haven’t  generated a Keystore for your apps, I suggest you look at the [official documentation](https://developer.android.com/studio/publish/app-signing). This guides you through the process  of creating a new Keystore. 

For the sake of security, we must keep signing properties outside of the codebase. A good way to avoid this is by  using environment variables to refer to the secrets. GitHub Actions provides a similar tool.

Open your repository on GitHub and go to the Settings tab. On the left navigation bar, click Secrets, Click New repository secret and add the following four secrets: 


![secrets](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/8mkmopy30jftihfuym9y.png)

- **ALIAS**: Alias of your signing key. 
- **KEY_STORE_PASSWORD**: The password to your signing Keystore. 
- **KEY_PASSWORD**: The private key password for your signing Keystore. 
- **SIGNING_KEY**: The base 64-encoded signing key is used to sign your app. 

This task uses secrets from our project. We can generate `signingKeyBase64` using the popular `Base64` encoding scheme which allows us to store the file as text in our GitHub Secrets and later in the GitHub Workflow process  decode it back to our original KeyStore file. 

```
openssl base64 < my-upload-key.Keystore | tr -d '\n' | tee my-upload-key.keystore.base64.txt
```
If everything went right, you should see a newly created file `my-upload-key.keystore.base64.txt` which contains a cryptic text that represents your KeyStore file.

```
      # Building and signing App
    - name: Build Android App Bundle
      run: cd android && ./gradlew bundleRelease 

    - name: Sign ABB
      uses: r0adkll/sign-android-release@v1
      # ID used to access action output
      id: sign_app
      with:
        releaseDirectory: android/app/build/outputs/bundle/release
        signingKeyBase64: ${{ secrets.SIGNING_KEY }}
        alias: ${{ secrets.ALIAS }}
        keyStorePassword: ${{ secrets.KEY_STORE_PASSWORD }}
        keyPassword: ${{ secrets.KEY_PASSWORD }}

    - name: Upload Artifact
      uses: actions/upload-artifact@v2
      with:
        name: Signed app bundle
        path: ${{steps.sign_app.outputs.signedReleaseFile}}
        retention-days: 4

    - name: Create Release
      id: create_release
      uses: actions/create-release@v1
      env:
        GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }} # This token is provided by Actions, you do not need to create your own token
      with:
        tag_name: ${{ github.run_number }}
        release_name: Release V${{ github.run_number }}
        draft: false
        prerelease: false

    - name: Upload Release AAB
      id: upload-release-asset 
      uses: actions/upload-release-asset@v1
      env:
        GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
      with:
        upload_url: ${{ steps.create_release.outputs.upload_url }}
        asset_path: ${{steps.sign_app.outputs.signedReleaseFile}}
        asset_name: app-release-v${{ github.run_number }}.zip
        asset_content_type: application/zip

      # Distribute  App to google play
    - name: Publish to Play Store internal test track
      uses: r0adkll/upload-google-play@v1.0.15
      with:
        serviceAccountJsonPlainText: ${{ secrets.ANDROID_SERVICE_ACCOUNT_JSON }}
        packageName: com.artery
        releaseFiles: android/app/build/outputs/bundle/release/app-release.aab
        track: internal
        inAppUpdatePriority: 3
    
    - name: Notify slack success
      uses: craftech-io/slack-action@v1
      with:
        slack_webhook_url: ${{ secrets.SLACK_NOTIFY }}
        slack_channel: pipeline-ci-cd
        status: ${{ job.status }}
      if: always()
```

In the code above, the build job performs multiple steps: 

- Generates a release ABB using the `./gradlew bundleRelease`. 

- Signs the APK using the [r0adkll/sign-android-release action](https://github.com/r0adkll/sign-android-release), which is a third-party action available on the [GitHub marketplace](hhttps://github.com/marketplace). This step uses the four secrets you added in GitHub secrets.

**_Note_** comment out release section from `android/app/build.gradle` directory. Mainly, we just need to create an unsigned release before running signing job.

```
signingConfigs {
        // release {
            
        //     if (project.hasProperty('MYAPP_UPLOAD_STORE_FILE')) {
        //         storeFile file(MYAPP_UPLOAD_STORE_FILE)
        //         storePassword MYAPP_UPLOAD_STORE_PASSWORD
        //         keyAlias MYAPP_UPLOAD_KEY_ALIAS
        //         keyPassword MYAPP_UPLOAD_KEY_PASSWORD
        //     }
        // }
    }
```
- Uploads the signed APK as an artifact to GitHub. This step uses the ID from the previous step to access its output,  named `signedReleaseFile`. 

- `Create Release` This step contain the source code at the given tag, but it is also typical to deliver binary artifacts within releases themselves.

- `Upload Release` This step upload release so that users can download the new software, all you have to do is set `upload_url` to the `upload_url` in the output of the release step. Then likewise you set the `asset_path` to the artifact to upload, and `asset_name` to what you want it named in the release.

- `Publish to Play Store` This step is used to deploy a build to the Play Store. It requires setting up the [Google Play Developer API](https://developers.google.com/android-publisher/getting_started) and attaching it to Play Console. Once you've completed the setup, we’ll get a JSON file with all the information needed about the service account and place that in our project secret named SERVICE_ACCOUNT_JSON. To upload a build to the Play Store Console, use the open-source [r0adkll/upload-google-play](https://github.com/r0adkll/upload-google-play). 

- Notify on Slack using the [craftech-io/slack-action@v1](https://github.com/craftech-io/slack-action) This step Notify the result of Github Actions to a slack channel about the status of the workflows.

![App Bundle](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/425smhbbptfc9pvwsrxt.png)

![playstore](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/aql0phyusn0xes84u06g.png)

## Conclusion 
Congratulations, you've successfully built a complete CI/CD pipeline using GitHub Actions. If your code is already in GitHub, that is a good reason for using GitHub Actions to benefit from a very valuable integration with your code and release workflow. It is custom-built, providing APIs to create your own actions, as well as the option of obtaining them from the GitHub marketplace.

GitHub Actions is a powerful tool to simply add CI/CD to Android projects. Leveraging it saves loads of time that  we could have spent on manually building and distributing our applications.
