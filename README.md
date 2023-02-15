 <h1 align="center">Huawei-OpenMediation Mediation Github Documentation</h3>
 
 
 ![Latest Version](https://img.shields.io/badge/latestVersion-2.6.5-yellow)
<br>
![Supported Platforms](https://img.shields.io/badge/Supported_Platforms:-Native_Android_,_Unity_-orange)


# Introduction

In this documentation we explained how to use Huawei-OpenMediation mediation plugin developed by OpenMediation. 
OpenMediation repository link is [here](https://github.com/OpenMediationProject/OpenMediation-Android/tree/master/adapter/hwads/src/main/java/com/openmediation/sdk/mobileads)


# Compatibility

|   | Banner Ad | Interstitial Ad | Rewarded Video Ad | Native Ad |
| --- | --- | --- | --- | --- |
| Native (Java/Kotlin) | ✅ | ✅ | ✅ | ✅ | 
| Unity | ✅ | ✅ | ✅ | ❌ |



# How to start?

## Create an ad unit on Huawei Publisher Service

1. Sign in to [Huawei Developer Console](https://developer.huawei.com/consumer/en/console) and create an AdUnit

## Create an App

1. Log in to OpenMediation and navigate to the Overview interface. You will see "**Add App**" at the top of the application list, click to start adding an APP.
2. Firstly, select  the platform of your app, and select App has been listed on a supported app store.
3. Find your app on the selected Store.

For details, please refer to this [link](https://support.openmediation.com/hc/en-us/articles/360049637033-Add-and-manage-your-apps#step3-get-your-app-key-0-3)

## Add and Manage Ad Placements

1. The placement needs to match the added application, and you need to select the corresponding app in the application list of "**Overview**". After entering Monetize, click "**Setup**", click "**Placement**" in the navigation bar, and click "**Add Placement**" to add placement.
2. After customizing the name of the placement in the Name column, select the ad type you want and click Save on the top of the page.
3. In the Operations section, you can control the status of each placements, "**Enable**" or "**Disable**".

**NOTE**: Only one rewarded and interstitial placement is allowed to be created in the OpenMediation platform. Multi-ad scene requirements can be implemented by automatically calling logic and scene functions through the OpenMediation SDK.So, after perfom the adding placement steps for Rewarded and Interstitial Ads, you can also add Placement Scenes for them. For details about the Scenes please refer to this [link](https://support.openmediation.com/hc/en-us/articles/360051163693-Add-and-manage-your-placements#2-add-placements-0-1).

For details, please refer to this [link](https://support.openmediation.com/hc/en-us/articles/360051163693-Add-and-manage-your-placements#1-ad-type-0-0)

## Configure Ad Instance for Ad Placement

1. Click on "**Mediation**" in the left navigation bar "**Setup**" to go to the monetize function page.
2. Select the placement that needs to be configured, and click "**+Add Instance**" in the instance sub-interface to configure instance information.
3. You can get the ad network drop-down menu by clicking on the "**Ad Network**", and then click to select the Huawei Ads ad network.
4. When you click on the ad network list to select Huawei Ads, you'll see a list of instance additions, which you can start configuring by clicking on.
5. Configure Basic Informations of Instances. Note that, Unit ID must be same with the ID you got from Huawei Ads Platform
6. You can configure Manual eCPM for instance.
7. Enable instances.

For details, please refer to this [link](https://support.openmediation.com/hc/en-us/articles/360050626074-Add-Instance)

## Set Mediation Rule

1. In the Setup>Mediation Tab, select the placement that needs to be configured, and click "**+Add Mediation Rule**" in the "**Mediation Rule**" sub-interface to configure mediation rule.
2. Enter basic information.
3. Click Advanced options.
4. Enter Advanced Options information.
5. Enable Mediation Rule

For details, please refer to this [link](https://support.openmediation.com/hc/en-us/articles/360050663934-Mediation-rule-settings)



<h1 id="integrate-huawei-sdk">
Integrate the Huawei-OpenMediation Mediation SDK
</h1>

In the **project-level** build.gradle, include necessary Maven repositories.

```groovy
allprojects {
    repositories {
        google()
        jcenter()     
        maven {url 'https://developer.huawei.com/repo/’}
        maven { url 'https://dl.openmediation.com/omcenter/' }   
        }
}

```

<h1 id="app-level">
</h1>
In the app-level build.gradle, include necessary dependencies

```groovy
implementation 'com.openmediation:test-suite:2.0.2’   
implementation 'com.huawei.hms:ads-prime:3.4.56.302’   
implementation 'com.openmediation.adapters:hwads:2.6.0@aar'    
implementation 'com.openmediation:om-android-sdk:2.6.5@aar'

```

<h1 id="proguard-config">
</h1>
You must add the following to your Proguard configuration file (Android Studio: proguard-rules.pro or Eclipse: proguard-project.txt) if you are using Porguard in your application. Failed to do so will lead to errors.

```groovy
-dontwarn com.openmediation.sdk.**.*
-keep class com.openmediation.sdk.**{*;}

```

## Initialization
Before you can fetch ads from OpenMediation Platform, you should first initialize the OpenMediation SDK when your app started. To do so, we suggest to call OpenMediation SDK's init() method within onCreate() of an Application or Activity as below:

```groovy
import com.openmediation.sdk.InitConfiguration;
import com.openmediation.sdk.OmAds;
import com.openmediation.sdk.InitCallback;
import com.openmediation.sdk.utils.error.Error;
...

InitConfiguration configuration = new InitConfiguration.Builder()
          .appKey("Your AppKey")
          .logEnable(false)
          .build();
OmAds.init(configuration, new InitCallback() {

    // Invoked when the initialization is successful.
    @Override
    public void onSuccess() {
    }

    // Invoked when the initialization is failed.
    @Override
    public void onError(Error message) {
    }
});

```
**NOTE:** "**APP KEY**" can only be obtained by creating apps on OpenMediation



## **Permissions**
The HUAWEI Ads SDK (com.huawei.hms:ads) has integrated the required permissions. Therefore, you do not need to apply for these permissions. <br />

**android.permission.ACCESS_NETWORK_STATE:** Checks whether the current network is available.   <br/>

**android.permission.ACCESS_WIFI_STATE:** Obtains the current Wi-Fi connection status and the information about WLAN hotspots. <br />

**android.permission.BLUETOOTH:** Obtains the statuses of paired Bluetooth devices. (The permission can be removed if not necessary.) <br />

**android.permission.CAMERA:** Displays AR ads in the Camera app. (The permission can be removed if not necessary.) <br />

**android.permission.READ_CALENDAR:** Reads calendar events and their subscription statuses. (The permission can be removed if not necessary.) <br />

**android.permission.WRITE_CALENDAR:** Creates a calendar event when a user clicks the subscription button in an ad. (The permission can be removed if not necessary.) <br />

## **Configuring Obfuscation Scripts**
Before building the APK, configure the obfuscation configuration file to prevent the HUAWEI Ads SDK () from being obfuscated.

Open the obfuscation configuration file proguard-rules.pro in the app-level directory of your Android project, and add configurations to exclude the HUAWEI Ads SDK from obfuscation.

```groovy
-keep class com.huawei.openalliance.ad.** { *; }
-keep class com.huawei.hms.ads.** { *; }
```

## **Configuring Network Permissions**
To allow HTTP and HTTPS network requests on devices with targetSdkVersion 28 or later, configure the following information in the AndroidManifest.xml file :

```groovy
<activity
    ...
    android:usesCleartextTraffic="true"
    >
    ...
</activity>
```



# Version Change History

## 2.6.5
Latest version



# Platforms

## Native

This section demonstrates how to use OpenMediation mediation feature with Huawei Ads Kit on Native android app.

Firstly, integrate the OpenMediation Mediation SDK for Android

[OpenMediation Mediation SDK](https://support.openmediation.com/hc/en-us/articles/360045968373-Get-Started) can be used for all ad types.

**Note** : 
1) Developers can find app level build.gradle in their project from __**"app-folder/app/build.gradle"**__
2) If you use the native ad format in your application, please submit a ticket [here](https://developer.huawei.com/consumer/en/support/feedback) to get support from Huawei. 

### **Banner Ad**

To use _Banner_ ads in Native android apps, please check the OpenMediation Mediation SDK. Click [here](https://support.openmediation.com/hc/en-us/articles/360044290574-Banner) to get more information about OpenMediation Mediation SDKs Banner Ad development.

### **Interstitial Ad**

To use _Interstitial_ ads in Native android apps, please check the OpenMediation Mediation SDK. Click [here](https://support.openmediation.com/hc/en-us/articles/360044811373-Interstitial) to get more information about OpenMediation Mediation SDKs Interstitial Ad development.

### **Rewarded Video Ad**

To use _Rewarded Video_ ads in Native android apps, please check the OpenMediation Mediation SDK. Click [here](https://support.openmediation.com/hc/en-us/articles/360044290194-Rewarded-Video) to get more information about OpenMediation Mediation SDKs Rewarded Ad development.

### **Native Ads**

To use _Native_ ads in Native android apps, please check the OpenMediation Mediation SDK. Click [here](https://support.openmediation.com/hc/en-us/articles/360044811433-Native) to get more information about OpenMediation Mediation SDKs Native Ad development.


## **Unity**

This section demonstrates how to use OpenMediation feature with Huawei Ads Kit on Unity.

Make sure to check the article on [How to use Huawei Ads with Supported Ad Platforms in Unity ?](https://medium.com/huawei-developers/how-to-use-huawei-ads-with-supported-ad-platforms-in-unity-2be08c943a7f)

**Supported Ad Formats are:** Banner Ads, Interstitial Ads and Rewarded Ads.

Firstly, integrate the OpenMediation Unity Plugin to Unity.

For more details on OpenMediation Unity Plugin visit [here](https://support.openmediation.com/hc/en-us/articles/360025308573-Unity-Plugin-Integration#before-you-start-0-0)

### **Banner Ads**
To use Banner ads in Unity , please check the OpenMediation Unity Plugin. Click [here](https://support.openmediation.com/hc/en-us/articles/360026339613-Ad-Unit#banner-ad-0-3) to get more information about OpenMediation Unity Plugin Banner Ad development.

### **Interstitial Ads**
To use Interstitial ads in Unity, please check the OpenMediation Unity Plugin. Click [here](https://support.openmediation.com/hc/en-us/articles/360026339613-Ad-Unit#interstitial-ad-0-2) to get more information about OpenMediation Unity Plugin Interstitial Ad development.

### **Rewarded Ads**
To use Rewarded ads in Unity, please check the OpenMediation Unity Plugin. Click [here](https://support.openmediation.com/hc/en-us/articles/360026339613-Ad-Unit#rewarded-video-ad-0-1) to get more information about OpenMediation Unity Plugin Banner Ad development.

#### **Step 1:**
Make sure to switch to the Android Platform from **Build Settings -> Android -> Switch Platform**
#### **Step 2:**
**Edit -> Project Settings ->  Player -> Other Settings**<br>
In Other Settings set minimum API level to at least **16**.
#### **Step 3:**
**Edit -> Project Settings ->  Player -> Publishing Settings**<br>
In Publishing Settings select **“Custom Main Gradle Template”** , **“Custom Base Gradle Template”** and **“Custom Greadle Properties Template”** <br>
This will let you override **mainTemplate.gradle** , **baseProjectTemplate.gradle** and **gradleTemplate.properties** files in the project.
#### **Step 4:**
**baseProjectTemplate.gradle** is equal to **project-level gradle** so you have to include **Huawei's Maven repositories** from the Integrate the Huawei Mediation SDK section from [**here**](#integrate-huawei-sdk). <br>
**mainTemplate.gradle** is equal to **app-level build.gradle** so you have to include **dependencies** from the Integrate the Huawei Mediation SDK section from [**here**](#app-level).
#### **Step 5:**
Open **gradleTemplate.properties** and add the following lines
```groovy
android.useAndroidX=true
android.enableJetifier=true
```

After these configurations is completed you can display Huawei Ads.

# Screenshots

<table>
<tr>
<td>
<img alt="banner" src="https://user-images.githubusercontent.com/44730864/204800119-6c31a173-6502-40cd-ac10-7634057a7d50.png" width="200">

Banner Ad
</td>

<td>
<img alt="interstitial" src="https://user-images.githubusercontent.com/44730864/204799931-f9168239-b55a-458e-b723-580459d2c7d2.png" width="200">


Interstitial Ad
</td>

<td>
<img src="https://user-images.githubusercontent.com/44730864/204792774-7fd0c999-7b5a-4211-9a8c-ab7ff6182a76.png" width="200">
 
Rewarded Ad
</td>

<td>
<img src="https://user-images.githubusercontent.com/44730864/204792908-006c044e-1960-40f1-9d24-9529d7a713a6.png" width="200">

Native Ad
</td>
</tr>
</tr>
</table>

