# RoomDatabseLiveDta and ViewModel
## Overview

![overview](https://user-images.githubusercontent.com/21328787/81494560-66427500-92c7-11ea-8f08-a51e008f47a1.png)

During this tutorial we will create a complete Kotlin android CRUD application.We will use Kotlin programming language. And we will  follow the MVVM architecture. This app will allow users to add, update and delete subscriber details. List of available subscribers will be displayed in a RecycleView.

You will be able to learn about practical applications of these areas during this tutorial.

Room data persistence library
#### Data Binding
#### ViewModel and LiveData
#### Kotlin Coroutines
#### RecyclerView
#### MVVM best practices
#### Data Verification
#### Data Validation

#### INTRODUCTION
The purpose of Architecture Components is to provide guidance on app architecture, with libraries for common tasks like lifecycle management and data persistence. That's why to facilitate it I made an example in Kotlin 100%.

In your build.gradle (Module: app) make the following changes:

On top:

apply plugin: 'kotlin-kapt'
In the dependencies:

    // ViewModel
    implementation "androidx.lifecycle:lifecycle-viewmodel-ktx:2.2.0"
    // LiveData
    implementation "androidx.lifecycle:lifecycle-livedata-ktx:2.2.0"
    // Annotation processor
    kapt "androidx.lifecycle:lifecycle-compiler:2.2.0"

    implementation "androidx.room:room-runtime:2.2.5"
    kapt "androidx.room:room-compiler:2.2.5"

    // optional - Kotlin Extensions and Coroutines support for Room
    implementation "androidx.room:room-ktx:2.2.5"

    //coroutines
    implementation 'org.jetbrains.kotlinx:kotlinx-coroutines-core:1.3.5'
    implementation 'org.jetbrains.kotlinx:kotlinx-coroutines-android:1.3.5'

    testImplementation 'junit:junit:4.13'
    androidTestImplementation 'androidx.test.ext:junit:1.1.1'
    androidTestImplementation 'androidx.test.espresso:espresso-core:3.2.0'
    implementation 'androidx.recyclerview:recyclerview:1.1.0'
    implementation 'androidx.cardview:cardview:1.0.0'
