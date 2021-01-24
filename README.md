# Android_Review_11
DB model, Room and Repo for App

Repository acts as a mediators for different data souces from following data soreces such as (1) + (2)

    (1) Retrofit, a web service. see Android_Review_10      (2) Room, a persistent data models
    
  
                             |                                          |
                             |                                          |
                             V                                          V
  
                                  (3) Repository, Caches of the app. 
                                  
                                                  |
                                                  |
                                                  |
                                                  V
                          
  
                                           (4) ViewModel
                                              to bind (3) with UI element. 
                                              M + V = C, 
                                              see Android_Review_12 using coroutines as workManager.
  
  
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
           - domain (see Android_Review_10, data src fm web server)
           - repo (mediators for diff data src)
           - viewmodels (see Android_Review_12, to resolve the problems from I/O threads and UI main thread using coroutines instead of threads)
           
 

3. create a data model for data sorce from Web Server, see (4).

         // go to app/src/main/java/..../katesvideoapp/domain/Models.kt
         
         package com.example.android.katesvideoapp.domain
         
         [customed truncate util module for byte streaming live data]
         import com.example.android.katesvideoapp.util.Truncator
         
         //data class Video(param){body}
         
         data class Video(
            val title: String,
            val des: String,
            val url: String,
            val updated: String,
            val thumbnail: String
         
         ){
         
           get() = des.Truncator(200)
           val shortDes: String
         
         }


4. for data sorce from Web Server, see Android_Review_10.

        // app/..../katesvideoapp/network/DataConverter.kt
        // httpResult - dbObj converter

        /* fun methodCalled(): List<Video> {

              return videos.map {

                  Video(

                      title = it.title
                      des = it.des
                      url = it.url
                      updated = it.updated
                      thumbnail = it.thumbnail

                  )

              }


        }*/
        
 
 

to be continued....
