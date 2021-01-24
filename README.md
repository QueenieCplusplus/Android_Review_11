# Android_Review_11
DB model, Room and Repo for App

Repository acts as a mediators for different data souces from following data soreces such as (1) + (2)

  (1) Room, a persistent data models
  
  (2) Retrofit, a web service. see Android_Review_10
  
  (3) Repository, Caches of the app. 
  
  (4) ViewModel, to bind (3) with UI element. M + V = C, see Android_Review_12 using coroutines as workManager.
  
  
  ![](https://raw.githubusercontent.com/QueenieCplusplus/Android_Review_11/main/Architecture.png)
  
  
 1. add dependencies using implementation method called in path app/build.gradle

        dependencies {

            // architecture components
            def lifecycle_version = "2.2.0"
            implementation "android.lifecycle:lifecycle-extensions:$lifecycle_version"
            implementation "android.lifecycle:lifecycle-viewmodel-ktx:$lifecycle_version"
            
            // Room dependencies
            def room_version = "2.2.0"
            implementation "androidx.room-runtime:$room_version"
            kapt "androidx.room:room-compiler:$room_version"

        }


2. app's architecture =>


       app - db (data src fm Room)
           - domain (data src fm web server)
           - repo (mediators for diff data src)
           - viewmodels (see Android_Review_12)

to be continued....
