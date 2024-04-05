# Marathon Cloud GitHub Action

With this action you can easily run your Android instrumentation tests.

[Marathon Cloud](https://marathonlabs.io) is designed to run all your automated tests in just 15 minutes,
no matter how many tests you have.

## Quick example

A really basic example building an app apk, test apk and running tests:

```yaml
name: Run tests
on: push
jobs:
  run-tests:
    runs-on: ubuntu-latest
  steps:
    - uses: actions/checkout@v4
    - name: Build app
      run: ./gradlew assembleDebug assembleAndroidTest
    - name: Run tests
      uses: MarathonLabs/action-test@1.0.3
      with:
        apiKey: ${{ secrets.MARATHON_CLOUD_API_TOKEN }}
        application: app/build/outputs/apk/debug/app-debug.apk
        testApplication: app/build/outputs/apk/androidTest/debug/app-debug-androidTest.apk
        platform: android
        output: "./results"
        version: "1.0.7"
```

## Inputs

|             Name             | Description                                                                                                                                                                                                                                          | Default                                      | Example                                                                                                                                                                                          |
| :--------------------------: | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
|     `apiKey` (required)      | Marathon Cloud API key                                                                                                                                                                                                                               |                                              | `cafebabe`                                                                                                                                                                                       |
|     `version` (required)     | marathon-cloud cli version to use                                                                                                                                                                                                                    |                                              | `1.0.0`                                                                                                                                                                                          |
|   `application` (required)   | Application binary path. <br>**Android**: `application` should point to the APK file. <br>**iOS**: `application` should point to an ARM compatible Simulator build packaged in an ipa format or a zip archive.                                       |                                              | **Android**: `app/build/outputs/apk/debug/app-debug.apk` <br>**iOS**: `/home/user/workspace/sample.zip` or `/home/user/workspace/sample.ipa`                                                     |
| `testApplication` (required) | Test application binary path. <br>**Android**: `test_application` should point to the test .apk file for your app. <br>**iOS**: `test_application` should point to an ARM compatible iOS Test Runner app packaged in an ipa format or a zip archive. |                                              | **Android**: `app/build/outputs/apk/androidTest/debug/app-debug-androidTest.apk` <br>**iOS**: `/home/user/workspace/sampleUITests-Runner.zip` or `/home/user/workspace/sampleUITests-Runner.ipa` |
|    `platform` (required)     | Testing platform                                                                                                                                                                                                                                     | ``                                           | `Android` or `iOS`                                                                                                                                                                               |
|    `osVersion` (optional)    | Android or iOS OS version. For Android one of [10, 11, 12, 13, 14]. For iOS one of [16.4, 17.2]                                                                                                                                                      | **Android**: `11`; **iOS**: `16.4`           | `12`, `17.2`, etc.                                                                                                                                                                               |
|   `systemImage` (optional)   | OS-specific system image. For Android only                                                                                                                                                                                                           | ``                                           | `default`, `google_apis`, etc.                                                                                                                                                                   |
|     `output` (optional)      | Output folder path                                                                                                                                                                                                                                   | ``                                           | ``                                                                                                                                                                                               |
|      `link` (optional)       | Link to commit                                                                                                                                                                                                                                       | ``                                           | ``                                                                                                                                                                                               |
|    `isolated` (optional)     | Run each test in isolation, i.e. isolated batching                                                                                                                                                                                                   | `false`                                      | `true`, `false`                                                                                                                                                                                  |
|     `flavor` (optional)      | Type of tests to run                                                                                                                                                                                                                                 | `native`                                     | `native`, `js-test-appium`, `python-robotframework-appium`                                                                                                                                       |
|   `filterFile` (optional)    | File containing test filters in YAML format, following the schema described at https://docs.marathonlabs.io/runner/configuration/filtering/#filtering-logic. For iOS see also https://docs.marathonlabs.io/runner/next/ios#test-plans.               | ``                                           | ``                                                                                                                                                                                               |
|      `wait` (optional)       | Wait for test run to finish if true, exits after triggering a run if false.                                                                                                                                                                          | ``                                           | `true`                                                                                                                                                                                           |
|      `name` (optional)       | Name for run, for example it could be description of commit.                                                                                                                                                                                         | ``                                           | AmazingRun                                                                                                                                                                                       |
|     `device` (optional)      | Device type. For Android one of [phone, tv, watch]. For iOS one of [iPhone-14, iPhone-15]                                                                                                                                                            | **Android**: `phone`; **iOS**: `iPhone-14`   | `phone`, `tv`, `watch`, `iPhone-14`, `iPhone-15`                                                                                                                                                 |
|  `xcodeVersion` (optional)   | Xcode version. Only for iOS. Possible values: [14.3.1, 15.2]                                                                                                                                                                                         | `14.3.1`                                     | `14.3.1`, `15.2`                                                                                                                                                                                 |


## marathon-cloud version

For action version `0` the latest supported version is 0.3.11. Any version starting with 1.0.0 will require action version `1` to work.

Support matrix:
| action version | cli version supported | `latest` version |
|--------------- | ---------------------- | ---------------- |
| 1 | 1.0.0<=..<2.0.0 | not supported |
| 0 | <1.0.0 | 0.3.11 |

## Developing

The action source is located at [/src](/src). The action is written in TypeScript and compiled to a single javascript file with [`ncc`][ncc]. It's expected to checkin `lib/index.js` to the repository.

To setup the development environment, run the following commands:

```bash
$ npm install
```

To build the action script, run the following command:

```bash
$ npm run build
```

To test the action, we can use the workflow [Test workflow](https://github.com/MarathonLabs/setup-marathon-cloud/actions/workflows/test-marathon-cloud.yaml) to trigger a build.

[ncc]: https://github.com/vercel/ncc
[marathon-cloud]: https://github.com/MarathonLabs/marathon-cloud-cli

## LICENSE

MIT
