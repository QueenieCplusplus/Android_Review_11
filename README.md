# Android_Review_11
converter for DB model - Room as cache in App.

系統中模組之間的關係僅能為 V-C-M 。

             ui.xml acts as UI View ------ UI.kt acts as Controller ------ViewModel.kt acts as Model

Repository acts as a mediators for different data souces from following data soreces such as (1) + (2)

Room helps user to persist info even they closed app for several days.

    (1) 遠端資料處理                                  (2) 手機系統內部緩存
    Retrofit, a web service. see Android_Review_10    Room, a persistent data models saved in Caches of the app.
    
    
                        Domain                 DB                             Cache
    JSON obj -----     List<Video>  ----    List<DBVideo>   ---  Dao   ----   VideosDB  ---  Room
    
    
                                           
                                           List<Video>
    
  
                             |                                          |
                             |                                          |
                             V                                          V
                             
                             
  
                                 (3) 暫存器，扮演記憶體使用佔用資料的協調者角色
                                     Repository, a Mediator for data src from remote or local.
                                     If this data is stale, the app's repository module starts updating the data in the background.
                                  
                                                  |
                                                  |
                                                  
                                                  
                                      LiveData acts as an Event Observer
                                                  
                                                  
                                                  
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


       app 
           - network obj (data src fm Remote)
           - domain (related with network obj)
           - db (data src fm Room, to convert data from db to room-cache)
           
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

        

4. create a data model for persistent data sorce using Room Module. It's responsible for R/W from DB.

       // go to app/src/main/java/..../katesvideoapp/db/DBEntities.kt 資料實體模組
       
       package com.example.android.devbyteviewer.db
       
       import androidx.room.Entity
       import androidx.room.PrimaryKey
       import (3) module 
       
       @Entity
       data class DBVideo constructor(
       
            @PrimaryKey
            val title: String,
            val des: String,
            val url: String,
            val updated: String,
            val thumbnail: String
       
       )
       
       
       // Public Fun
       // Mapper from DBVideo to DomainModel called Video
       fun List<DBVideo>.asDomainModel(): List<Video> {
           
           return map{
           
                Video(
                  
                     title = it.title
                      des = it.des
                      url = it.url
                      updated = it.updated
                      thumbnail = it.thumbnail
                
                )
           
           }
       
       
       }
       

5.
  to create a Dao, also known as Data Access Object between DBVideo and VideosDB.
  to create a persistent DB model using Room. R/W from DBVideo to VideosDB.

       // go to app/src/main/java/..../katesvideoapp/db/Room.kt 持續性資料庫
       
       // TODO
       // see Android_Review_12
       
       
 
6. today's tip (PrimaryKey and Index)

    主鍵與索引鍵

   https://blog.niclin.tw/2018/06/09/sql-基本觀念-primary-key-/-index-/-unique-差別/
   
7. android's tip (MVC in Kotlin)

    https://developer.android.com/jetpack/guide

8. android's tip (MVC in Java)

    https://github.com/android/architecture-components-samples/tree/main/BasicSample/app/src/main/java/com/example/android/persistence (sample code)
