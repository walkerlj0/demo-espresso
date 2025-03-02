# Espresso with Android real devices
This repo presents an example of executing Espresso test cases on Sauce Labs Real Devices as part
of the native framework capability.

## Table of Contents
1. [Prerequisites](#prerequisites)
1. [Setup](#setup)
1. [Execution](#execution)
1. [Examples](#examples)
    1. [Example 1 - Minimal configuration for all tests](#example-1---minimal-configuration-for-all-tests)
    1. [Example 2 - Minimal configuration for 2 tests](#example-2---minimal-configuration-for-2-tests)
    1. [Example 3 - Run each test on its own device](#example-3---run-each-test-on-its-own-device)
    1. [Example 4 - Run in parallel on hard-coded devices](#example-4---run-in-parallel-on-hard-coded-devices)
    1. [Example 5 - Parallel execution using dynamic devices](#example-5---parallel-execution-using-dynamic-devices)
    1. [Example 6 - Parallel execution by platform version](#example-6---parallel-execution-by-platform-version)
    1. [Example 7 - Single file test annotation](#example-7---single-file-test-annotation)
    1. [Example 8 - Multi file test annotation](#example-8---multi-file-test-annotation)
    1. [Example 9 - Single execution by using an already uploaded app](#example-9---single-execution-by-using-an-already-uploaded-app)

## Prerequisites
The test runner used by Sauce Labs to execute the Espresso tests is a downloadable Java jar file.
The current release is provided in this release but can also be downloaded from the 
[Sauce Labs Wiki](https://wiki.saucelabs.com/display/DOCS/Using+Espresso+for+Real+Device+Testing).

The scripts in this repo were developed on macOS and expect JDK 8 or higher to be installed and available.

## Setup
A prebuilt native Android application and it's Espresso test cases are included in this repo. Source to this application 
can be found on GitHub in this [folder](https://github.com/saucelabs/sample-app-mobile/tree/master/android/app/src/androidTest/java/com/swaglabsmobileapp).
The two test-classes can be found here:

- [com.swaglabsmobileapp.LoginTest](https://github.com/saucelabs/sample-app-mobile/blob/master/android/app/src/androidTest/java/com/swaglabsmobileapp/LoginTest.kt)
- [com.swaglabsmobileapp.SwagLabsFlow](https://github.com/saucelabs/sample-app-mobile/blob/master/android/app/src/androidTest/java/com/swaglabsmobileapp/SwagLabsFlow.kt)

The application is bundled in [`SauceLabs.Mobile.Sample.Espresso.App.apk`](./SauceLabs.Mobile.Sample.Espresso.App.apk) 
and the tests cases are bundled in [`SauceLabs.Mobile.Sample.Espresso.Tests.apk`](./SauceLabs.Mobile.Sample.Espresso.Tests.apk).

Screenshots will automatically be uploaded to Sauce Labs and are added by [Spoon](https://github.com/square/spoon) and 
the [spoon-gradle-plugin](https://github.com/stanfy/spoon-gradle-plugin). See also the above mentioned classes for the 
implementation of Spoon.

### Running on Sauce Labs
> When you start running Espresso tests for the first time on Sauce Labs you will need to create a new project. This 
>only needs to be done once!

The [`SauceLabs.Mobile.Sample.Espresso.App.apk`](./SauceLabs.Mobile.Sample.Espresso.App.apk) file needs to be uploaded 
to Sauce Labs and an "App Project" needs to be created to reference the app from our test runner.
Follow these steps to upload the APK and create an App Project:

1. Sign into Sauce Labs at [https://app.saucelabs.com](https://app.saucelabs.com)
1. Navigate to SAUCE APPS &rarr; Legacy RDC &rarr; + New App &rarr; Android/IOS App.
1. Follow the prompts to upload the `SauceLabs.Mobile.Sample.Espresso.App.apk` file.
1. The `App name` will be the name of the project. Give it a descriptive name and replace spaces with for example dashes. 
1. Once created, click on the new `Espresso-Swag-Labs-Mobile-App` project.
1. Navigate to AUTOMATED TESTING &rarr; Espresso/Robotium &rarr; Setup Instructions.
1. Copy the value of the `testobject_api_key` and set it as an environment variable in the terminal you are executing 
your tests from:

    ```
    # For OSX/Linux
    export ESPRESSO_API_KEY='xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx'
    ```

## Execution
There are 2 ways of executing Espresso tests on Sauce Labs Real Devices:

1. Through the command line, see this [Sauce Labs Wiki page](https://wiki.saucelabs.com/display/DOCS/Command+Reference+for+Sauce+Runner+for+Real+Devices)
1. Through a configuration file, see this [Sauce Labs Wiki page](https://wiki.saucelabs.com/display/DOCS/Creating+a+Sauce+Runner+for+Real+Devices+Configuration+File)

This project has been set up in such a way that it will use `yaml`-configuration files which will be explained in the 
[Examples](#examples)-section

Executing a test from this repository is a matter of running `run-runner.sh` and passing in the example number (1 ... 9):

```bash
$ ./run-runner.sh 1
```

## Examples
There are eight examples provided starting from the most basic and moving up to more advanced approaches.
The mindset behind the progression is the desire to shard multiple tests cases across multiple devices using parallel 
execution to ultimately reduce the amount of time needed to execute all tests.

### Example 1 - Minimal configuration for all tests
`runner-ex1.yml` provides a minimum configuration needed to run the Espresso tests.
In this example, all test cases are executed on a single, available device in sequential order.

### Example 2 - Minimal configuration for 2 tests
`runner-ex2.yml` provides a minimum configuration needed to run the Espresso tests.
In this example, there are two test cases executed on a single, available device in sequential order.

### Example 3 - Run each test on its own device
`runner-ex3.yml` breaks apart the execution of the two test cases such that they can run in parallel on separate devices
that are available (ie., not in use) in the pool.

### Example 4 - Run in parallel on hard-coded devices
`runner-ex4.yml` modifies the second example by specifying which device in the pool to execute each test on.
Still uses parallel execution.

### Example 5 - Parallel execution using dynamic devices
`runner-ex5.yml` uses the `deviceNameQuery` capability to look for available devices using wildcard names.
This example demonstrates the ability to run a specified test(s) on a pool devices that are configured the same but have
different names for parallel processing.

### Example 6 - Parallel execution by platform version
`runner-ex6.yml` uses only the `platformVersion` field to select an available device.
Demonstrates the ability to pick a specific version of Android from the pool of devices for executing tests in parallel.

### Example 7 - Single file test annotation
`runner-ex7.yml` uses Espresso test annotation. 
Demonstrates the ability to run four tests based on the `@ErrorFlow`-annotation in a single class,
see [here](https://github.com/saucelabs/sample-app-mobile/blob/master/android/app/src/androidTest/java/com/swaglabsmobileapp/LoginTest.kt#L54).

### Example 8 - Multi file test annotation
`runner-ex8.yml` uses Espresso test annotation. 
Demonstrates the ability to run two tests based on the `@HappyFlow`-annotation which are found in two classes,
see [here](https://github.com/saucelabs/sample-app-mobile/blob/master/android/app/src/androidTest/java/com/swaglabsmobileapp/LoginTest.kt#L34)
and [here](https://github.com/saucelabs/sample-app-mobile/blob/master/android/app/src/androidTest/java/com/swaglabsmobileapp/SwagLabsFlow.kt#L35).

### Example 9 - Single execution by using an already uploaded app
`runner-ex9.yml` uses only uses the appId of an already uploaded app.
This can be retrieved by going to:

1. Sign into Sauce Labs at [https://app.saucelabs.com](https://app.saucelabs.com)
1. Navigate to **SAUCE APPS &rarr; Legacy RDC &rarr; Your App Project**
1. When you are on the dashboard go to **All Versions &rarr; Select a number from the app**
