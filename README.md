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
        
 This interface should be annotated with @Dao annotation. Room will recognize this interface as a Data Access Object interface after seeing this @Dao annotation.
Function names are not important. Room library generates corresponding codes to implement the functions by reading its annotation(@Insert,@Update,@Delete,@Query), its parameters and return types.
Functions can be defined with or without return types. (Return values are useful for verification)
Since room provides direct support for Kotlin coroutines, room facilitates us to write these functions as suspending functions with suspend modifier.
getAllSudents() funciton returns a LiveData of list of Student. Therefore we don’t need to use a background thread to execute that function. So we don’t neeed to write it as a suspending funciton.

#### Create A RoomDatabase class
    @Database(entities = [StudentsEntity::class],version = 1)
        abstract class StudentDatabase:RoomDatabase() {
        abstract val studentDAO:StudentDAO
        companion object{
            @Volatile
            private var INSTANCE:StudentDatabase?=null
            fun getInstance(context: Context):StudentDatabase{
                synchronized(this){
                    var instance= INSTANCE
                    if(instance==null){
                        instance=Room.databaseBuilder(context.applicationContext,StudentDatabase::class.java,"Student_database").build()
                    }
                    return instance
                }
            }
        }
    }

We should annotate this with @Database annotation. Here we need to provide the list of entity classes(in this project we have only one entity class). Then provide the version number of the database. This version number is important when we are migrating the data base from one version to another.

Then , we need to declare an abstract reference for the DAO interface. In this small demo project we have only one entity class and a corresponding DAO interface. If we had more entity classes, we would have listed them all here and defined the references for corresponding DAOs here.

Actually this is a boilerplate code part. You don’t have to remember this. You can copy paste this code part to all your room database classes and change the names.

#### Repository class

In this project , We are following MVVM architecture. MVVM is the recommended best architecture for Android Development by Google.

MVVM stand for Model, View and View Model. Model means all data management related components. Model has local database related components, remote data sources related components and a repository. For this small project we are not going to use remote data sources. We have already created an entity class, a DAO interface and a database class which completes this part of the MVVM architecture.

Now , We are going to create a repository class for this project.

The purpose of a repository class is to provide a clean API for view models to easily get and send data.

As Google documentation says , You can consider repositories to be mediators between different data sources, such as local databases, web services, and caches.

In our simple project we only have a local database, some of you might think, why should we create an intermediate repository class? Can’t we directly communicate with the DAO from the view model. But my goal here is to teach you about MVVM architecture.

    class StudentRepository(private val dao: StudentDAO) {
        val student=dao.getAllStudents()
        fun insert(studentsEntity: StudentsEntity)
        {
            return dao.insetstudent(studentsEntity)
        }

        fun update(studentsEntity: StudentsEntity){
            return dao.updatestudent(studentsEntity)
        }
        fun delete(studentsEntity: StudentsEntity){
            return dao.delectstudent(studentsEntity)
        }
        fun clearAll(studentsEntity: StudentsEntity){
            return dao.deletall()
        }
    }
  #### View Mode Class
  
