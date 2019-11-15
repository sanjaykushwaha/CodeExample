###### Custom Configuration and intialization of Work Manager 
1. For remove default initilizer update Android-manifest using the merge rule tools:node="remove"
   
   <provider android:name="androidx.work.impl.WorkManagerInitializer"
              android:authorities="${applicationId}.workmanager-init"
              tools:node="remove" />

2. Use custom application class and override Configuration.Provider class
         class MyApplication extends Application implements Configuration.Provider {
                onCreate(){
                    // provide custom configuration
                     Configuration myConfig = new Configuration.Builder()
                    .setMinimumLoggingLevel(android.util.Log.INFO)
                    .build();

                    /*initialize WorkManager*/
                    WorkManager.initialize(this, myConfig);
                }
         
         
         
                @Override
                public Configuration getWorkManagerConfiguration() {
                    return Configuration.Builder()
                            .setMinimumLoggingLevel(android.util.Log.INFO)
                            .build();
                }
}

3. Access custom initilized work manager
Use WorkManager.getInstance(Context) when accessing WorkManger (NOT WorkManager.getInstance())


# Threading in Worker: Bydefault Worker run on different thread that is backed by Executer defined in default inilized worker. For use own executer you have to first remove default initilizer and then use manually initilizer.

        WorkManager.initialize(
            context,
            new Configuration.Builder()
                .setExecutor(Executors.newFixedThreadPool(8))
                .build());


     
