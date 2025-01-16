# Android-SDK
使用Android SDK的编辑包及其使用说明

# 安装Android-SDK

安装sdk依赖有两种方式，一种是从maven库中获取sdk依赖，另一种是使用jar包获取依赖，下面分别展示两种方式安装sdk依赖

## 从maven库中获取sdk依赖

在项目的app目录下的build.gradle文件中填写以下代码，并点击同步即可引入依赖

```
dependencies {

    // Use the Maven library
    implementation "com.eyeofcloud.ab:android-sdk:3.13.4"
    implementation("com.eyeofcloud.ab:core-api:3.1.1"){
        exclude group: 'javax.annotation'
        exclude module: 'annotations'
    }

    api "org.slf4j:slf4j-api:1.7.30"

    implementation 'androidx.appcompat:appcompat:1.4.1'
    implementation 'com.google.android.material:material:1.5.0'
    implementation 'androidx.constraintlayout:constraintlayout:2.1.3'
    implementation "androidx.work:work-runtime:2.7.1"
    testImplementation 'junit:junit:4.13.2'
    androidTestImplementation 'androidx.test.ext:junit:1.1.3'
    androidTestImplementation 'androidx.test.espresso:espresso-core:3.4.0'
}
```

## 从本地包中引入sdk依赖

1. 在app目录中创建libs目录

2. 将仓库中的jar包放入libs目录

3. 在项目的app目录下的build.gradle文件中填写以下代码，并点击同步即可引入本地sdk依赖

```
//  Use local packages
    implementation fileTree(dir: 'libs', include: ['*.aar','*.jar'], exclude:[])

    api "org.slf4j:slf4j-api:1.7.30"

    implementation 'androidx.appcompat:appcompat:1.4.1'
    implementation 'com.google.android.material:material:1.5.0'
    implementation 'androidx.constraintlayout:constraintlayout:2.1.3'
    implementation "androidx.work:work-runtime:2.7.1"
    testImplementation 'junit:junit:4.13.2'
    androidTestImplementation 'androidx.test.ext:junit:1.1.3'
    androidTestImplementation 'androidx.test.espresso:espresso-core:3.4.0'
```

# 使用Android-sdk

## 示例用法

1. 将仓库中的datafile.json文件放在res/raw目录中

2. 下面的示例演示了如何使用构建器选项初始化云眼特性标帜

```
    // change datafileHost
    DatafileConfig.defaultHost = "https://cdn.eyeofcloud.com";
    // change eventHost
    EventFactory.EVENT_ENDPOINT = "https://event.eyeofcloud.com/v1/events";

    EyeofcloudManager eyeofcloudManager = EyeofcloudManager.builder()
        .withEventDispatchInterval(1L, TimeUnit.SECONDS)
        .withDatafileDownloadInterval(15, TimeUnit.MINUTES)
        .withSDKKey("Your SDKKey")  //change sdk
        .build(getApplicationContext());

    WorkerScheduler.requestOnlyWhenConnected = false;
    Map<String, Object> attributes = new HashMap<>();
    EyeofcloudClient eyeofcloudClient = eyeofcloudManager.initialize(getApplicationContext(), R.raw.datafile);

    //用户id，实际使用时，用真实用户的id替换
    String userId = "androiddemouser1";  //change userId
    user = eyeofcloudClient.createUserContext(userId, attributes);

    EyeofcloudDecision decision = user.decide("Your flagKey"); //change flagKey

    String variationKey = decision.getVariationKey();

    // Track an event
    user.trackEvent("Your eventName")
```