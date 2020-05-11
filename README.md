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

Here are the links to copy the latest versions of those dependencies.
ViewModel and LiveData
https://developer.android.com/jetpack/androidx/releases/lifecycle#kotlin

 Room
https://developer.android.com/jetpack/androidx/releases/room

Coroutines
https://github.com/Kotlin/kotlinx.coroutines


#### Create A Room Entity Classes
For our CRUD application, we need to create this table in our database.
In this Entity Class we have declare at list one primary key

Create a new Kotlin class and name it as Subscriber. We should add data modifier in front of the “class” to make this a “data class”.

Room data persistence library  read the instructions provided by us and create the table inside the database. We provide instructions to Room using annotations.

    @Entity(tableName = "student_entity")
    data class StudentsEntity (
        @PrimaryKey(autoGenerate = true)
        @ColumnInfo(name="student_id")
        val id:Int,
        @ColumnInfo(name = "subscriber_name")
        var name:String,
        @ColumnInfo(name="student_email")
        var email:String
    )
@Entity – Room library creates a table for each class annotated with “@Entity” annotation.Given value of tableName property will be the name of the table
@ColumnInfo – Specify the name of the corresponding column in the table,if you want it to be different from the name of the member variable
@PrimaryKey – Set the primary key for the database table. Set the autoGenerate property as true if you want this primary key to auto generate.
 

Here is the complete list of Room annotations:
https://developer.android.com/reference/android/arch/persistence/room/package-summary#annotations

#### Create A Data Access Object(DAO Class)
Data access Object interface, or DAO interface is where we define the functions that access the database.

When we were using Sqlite directly, we had to write larger database queries in a separate file. We had to write complex code logic to work with cursor objects. But now room does all the hard work for us reading the information we provide using annotations.
We need create a new Kotlin interface. Let’s name this as StudentDao.
   
       @Dao
        interface StudentDAO {
            @Insert
            fun insetstudent(studentsEntity: StudentsEntity)
            @Update
            fun updatestudent(studentsEntity: StudentsEntity)
            @Delete
            fun delectstudent(studentsEntity: StudentsEntity)
            @Query("DELETE FROM student_entity")
            fun deletall()
            @Query("SELECT * FROM student_entity")
            fun getAllStudents():LiveData<List<StudentsEntity>>


        }   

