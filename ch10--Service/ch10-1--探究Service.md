



## 探究Service

##### 1、新建Service，new->Service->Service  自动在Androidmanifest.xml中注册

```kotlin
public class MyService extends Service {
    public MyService() {
    }
    @Override
    public IBinder onBind(Intent intent) {
}
//服务创建时调用
    @Override
    public void onCreate() {
        super.onCreate();
}
//每次服务启动时调用
    @Override
    public int onStartCommand(Intent intent, int flags, int startId) {
      return super.onStartCommand(intent, flags, startId);
}
//服务销毁时调用
    @Override
    public void onDestroy() {
        super.onDestroy();
    }
}
```

##### 2、启动和停止service

使用intent。启动：val intent =Intent (this,MyService:class.java)   startService(intent)

停止：val intent =Intent (this,MyService:class.java)   stopService(intent)

Android8.0以后，只有当应用保持在前台可见状态下，Service才能稳定运行，一旦进入后台随时可能被回收。



##### 3、Activity和Service进行通信

Activity和Service



## 探究Service

##### 1、新建Service，new->Service->Service  自动在Androidmanifest.xml中注册

```kotlin
public class MyService extends Service {
    public MyService() {
    }
    @Override
    public IBinder onBind(Intent intent) {
}
//服务创建时调用
    @Override
    public void onCreate() {
        super.onCreate();
}
//每次服务启动时调用
    @Override
    public int onStartCommand(Intent intent, int flags, int startId) {
      return super.onStartCommand(intent, flags, startId);
}
//服务销毁时调用
    @Override
    public void onDestroy() {
        super.onDestroy();
    }
}
```

##### 2、启动和停止service

使用intent。启动：val intent =Intent (this,MyService:class.java)   startService(intent)

停止：val intent =Intent (this,MyService:class.java)   stopService(intent)

Android8.0以后，只有当应用保持在前台可见状态下，Service才能稳定运行，一旦进入后台随时可能被回收。



##### 3、Activity和Service进行通信

在Service中Activity和Service的通信主要依赖onBind()方法

在Activity中通过 ServiceConnection匿名类实现，重写方法，onServiceConnected()方法是在两者成功绑定时调用，而onServiceDisconnected()方法是在Service的创建进程崩溃时或者被杀掉时调用，不常用。

在点击事件中创建intent。val intent=Intent(this,MyService::class.java)

绑定Service            bindService(intent,connection,Context.BIND_AUTO_CREATE)  

（只启动oncreate() 

解绑Service                           unbindService（connection）

##### 4、Service的生命周期

​    调用startService()方法，Service方法就会调用，回调到onStartCommand()方法，如果Service没有创建就会先回调到onCreate()然后回调onStartCommand()方法，直到被stopService()或者stopSelf()方法调用或者是被系统回收。

  还可以调用Context的bindService()来绑定，这时会回调到onBind()方法，如果Service之前没有创建，oncreate()方法会先于onBind()方法，通过unbindService()来结束。

startService()和bindService()都启动了，要各个结束。

##### 5、使用前台Service

```kotlin
override fun onCreate() {
    super.onCreate()

    val manager = getSystemService(Context.NOTIFICATION_SERVICE) as NotificationManager
    if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.O) {
        val channel = NotificationChannel("my_service", "前台Service通知", NotificationManager.IMPORTANCE_DEFAULT)
        manager.createNotificationChannel(channel)
        Log.d("MyServiceEEE", "onCreate executed")
    }
    val intent = Intent(this, MainActivity::class.java)
    val pi = PendingIntent.getActivity(this, 0, intent, 0)
    val notification = NotificationCompat.Builder(this, "my_service")
        .setContentTitle("This is content title")
        .setContentText("This is content text")
        .setSmallIcon(R.drawable.small_icon)
        .setLargeIcon(BitmapFactory.decodeResource(resources, R.drawable.large_icon))
        .setContentIntent(pi)
        .build()
    startForeground(1, notification)
}
```

6、使用IntentService

```kotlin
class MyIntentService : IntentService("MyIntentService") {

    override fun onHandleIntent(intent: Intent?) {
        // 打印当前线程的id
        Log.d("MyIntentService", "Thread id is ${Thread.currentThread().name}")
    }

    override fun onDestroy() {
        super.onDestroy()
        Log.d("MyIntentService", "onDestroy executed")
    }

}
```

onHandleIntent()方法在子线程中运行