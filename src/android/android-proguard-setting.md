---
description: >-
  Setting of RemonSDK is required when setting ProGuard. It summarizes the
  options and types used in the SDK.
---

# Android - ProGuard Setting

## gradle.build <a id="gradle-build"></a>

gradle.build

{% code-tabs %}
{% code-tabs-item title="gradle.build" %}
```text
buildTypes {
        debug {
            minifyEnabled false
            proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'
        }
        release {
            minifyEnabled false
            proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'
        }
    }
```
{% endcode-tabs-item %}
{% endcode-tabs %}

## proguard-rules.pro <a id="proguard-rules-pro"></a>

proguard-rules.pro

{% code-tabs %}
{% code-tabs-item title="proguard-rules.pro" %}
```text
# By default, the flags in this file are appended to flags specified
# in /home/calmglow/dev/Android/tools/proguard/proguard-android.txt
# You can edit the include path and order by changing the proguardFiles
# directive in build.gradle.
#
# For more details, see
#   http://developer.android.com/guide/developing/tools/proguard.html

# Add any project specific keep options here:

# If your project uses WebView with JS, uncomment the following
# and specify the fully qualified class name to the JavaScript interface
# class:
#-keepclassmembers class fqcn.of.javascript.interface.for.webview {
#   public *;
#}
-keepattributes SourceFile,LineNumberTable, InnerClasses, Exceptions, Signature, EnclosingMethod
-keepattributes *Annotation*
-dontoptimize
-dontwarn org.**
-dontwarn retrofit2.**

-keep class com.remotemonster.sdk.* { *;}
-keep class com.remotemonster.sdk.data.** { *;}
-keep class com.remotemonster.sdk.util.Logger{ *;}
-keep class com.remotemonster.sdk.data.Channel{ *;}
-keep class com.remotemonster.sdk.data.InitMessage{ *;}
-keep enum com.remotemonster.sdk.data.ChannelStatus{ *;}
-keep enum com.remotemonster.sdk.data.ChannelType{ *;}
-keep class org.** { *;}
-keep interface org.** { *;}

-keepclasseswithmembers class * {@retrofit2.http.* <methods>;}
-keep class retrofit2.** { *; }

# ignore netty lib warning
-dontwarn io.netty.**

# netty 4.0
-keep class io.netty.** {*;}
-keep interface io.netty.** {*;}

# Slf4j for android
-keep class org.slf4j.** {*;}
-keep interface org.slf4j.** {*;}

# Jzlib
-keep class com.jcraft.jzlib.** {*;}
-keep interface com.jcraft.jzlib.** {*;}


# fasterxml
-keepnames class com.fasterxml.jackson.** { *; }
-dontwarn com.fasterxml.jackson.databind.**
-keep class org.codehaus.** { *; }
-keepclassmembers public final enum org.codehaus.jackson.annotate.JsonAutoDetect$Visibility {
    public static final org.codehaus.jackson.annotate.JsonAutoDetect$Visibility *; }

# okio
-dontwarn okhttp3.**
-dontwarn okio.**
-dontnote okhttp3.**
```
{% endcode-tabs-item %}
{% endcode-tabs %}

`proguard-rules.pro` is a single file. However, we recommend that you set the `netty`, `fasterxml`, and `okio` files separately.

