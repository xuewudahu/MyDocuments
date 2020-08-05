

#### Activity的主题

*在AndroidManifest.xml中的代码*

```kotlin
<activity android:name=".DialogActivity"
    android:theme="@style/Theme.AppCompat.Dialog">
</activity>
```

其中第二行---->指定Activity的主题，该主题为对话框式



#### Activity被回收是保存数据

*在activity中的代码*

```kotlin
override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    Log.d(tag,"onCreate")
    setContentView(R.layout.activity_main)
    if (savedInstanceState != null) {
        val tempData=savedInstanceState.getString("data_key")
        if (tempData != null) {
            Log.d(tag,tempData)
        }
    }
```

if判断savedInstanceState是否存在数据

*在activity中的代码*

```kotlin
override fun onSaveInstanceState(outState: Bundle, outPersistentState: PersistableBundle) {
    super.onSaveInstanceState(outState, outPersistentState)
    val tempData="Something you just typed"
    outState.putString("data_key",tempData)
}
```

在activity被回收时肯定会调用的方法，用此方法来保存被回收时的数据

#### activity的启动模式

**1、standard模式**

```kotlin
startNormalActivity.setOnClickListener {
    val intent=Intent(this,NormalActivity::class.java)
    startActivity(intent)
}
```

默认就是使用standard模式

创建一个activity就是一个窗口需要通过返回来返回

**2、singleTop模式****

*在AndroidManifest中的代码*

```kotlin
<activity android:name=".NormalActivity"
    android:theme="@style/Theme.AppCompat.Dialog.MinWidth"
    android:launchMode="singleTop"
    />
```

第三行------->声明启动方式

如果栈顶是该activity则不会创建新的activity

**3、singleTask模式**

*在AndroidManifest中的代码*

```kotlin
<activity android:name=".NormalActivity"
    android:theme="@style/Theme.AppCompat.Dialog.MinWidth"
    android:launchMode="singleTask"
    />
```

第三行------->声明启动方式

当显示该activity时会先在返回栈中查找是否有该activity，如果有则把在返回栈上面的其他activity都统统出栈，显示该activity

**4、singleInstance模式**

在AndroidManifest中的代码*

```kotlin
<activity android:name=".NormalActivity"
    android:theme="@style/Theme.AppCompat.Dialog.MinWidth"
    android:launchMode="singleInstance"
    />
```

第三行------->声明启动方式

该activity在成为栈顶时会在另一个返回栈显示，1->2,2->3;之后返回会是3—>1->2    3和1在一个栈 2在另一个栈，栈空的时候就会显示另一个栈的activity

通常用于共享activity的实例的

#### Activity的生命周期

**1、Activity的状态：**运行状态、暂停状态、停止状态和销毁状态

**2、回调方法：******

1. onCreate()  第一次创建时
2. onStart()   不可见到可见时
3. onResume()  处于栈顶时
4. onPause()  由栈顶到不是栈顶时
5. onStop()  可见到不可见时
6. onDestroy()  销毁时
7. onRestart()  停止状态到运行状态