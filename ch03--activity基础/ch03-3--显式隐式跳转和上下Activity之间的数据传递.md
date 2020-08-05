### 在activity中来回穿梭

1、显示Intent  

*在activity中的组件代码*

```kotlin
button2.setOnClickListener{
    val intent=Intent(this,SecondActivity::class.java)
    startActivity(intent)
}
```

2、隐式Intent

*需要在AndroidManifest.xml中配置*

```kotlin
<activity android:name=".SecondActivity">
    <intent-filter>
        <action android:name="com.example.activitytest.ACTION_START"/>
        <category android:name="android.intent.category.DEFAULT"/>
    </intent-filter>
</activity>
```

*在activity中配置*

```kotlin
button3.setOnClickListener{
    val intent=Intent("com.example.activitytest.ACTION_START")
    //会有默认的category
    startActivity(intent)
}
```

####  数据传递

**1、向下一个Activity传递数据**

按钮点击事件的代码

```kotlin
    button1.setOnClickListener {
        val data = "Hello SecondActivity"
        val intent = Intent(this, SecondActivity::class.java)
        //putExtra接受的是键值对，第一个参数是键，用于后面取值；第二个是真正要传递的数据
        intent.putExtra("extra_data", data)
        startActivity(intent)
    }
```
第二个activity如何接收的代码


```kotlin
override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    setContentView(R.layout.layout_second)
    //intent调用的是getIntent方法,会获取用于启动SecondActivity的Intent,getStringExtra获取到传递的数据
    //getIntExtra拿到的是整形；getBooleanExtra拿到的是布尔类型
    val extradata = intent.getStringExtra("extra_data")
    Log.d("SecondActivity", "extra data is $extradata")
}
```
}

**2、向上一个Activity传递数据**

在第一个Activity中接收下一个Activity发送过来的数据

```kotlin
    button1.setOnClickListener {
        val intent = Intent(this, SecondActivity::class.java)
        startActivityForResult(intent, 1)
    }
```
在第二个Activity中输入发送给第一个Activity的数据（如果用户不是通过点击返回的而是通过back键返回）

```kotlin
override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    setContentView(R.layout.layout_second)
    button2.setOnClickListener {
        //Intent传递数据，没有任何意图，只需要将数据存放进去
        val intent = Intent()
        intent.putExtra("data_return", "Hello FirstActivity")
        //setResult接受两个参数：第一个是用于向上一个Activity返回处理结果，一般是RESULT_OK或者RESULT_CANCELED
        //第二个是带有数据的intent传递过去。最后调用finish销毁。
        setResult(Activity.RESULT_OK, intent)
        finish()
    }
}

}
```

```kotlin
override fun onBackPressed() {
    val intent = Intent()
    intent.putExtra("data_return", "Hello FirstActivity")
    setResult(Activity.RESULT_OK, intent)
    finish()
 }
```
在第一个Activity中书写方法接收数据

```kotlin
//第一个参数requestCode是启动Activity时传入的请求码；第二个参数resultcode是返回数据传入的处理结果
//第三个参数是data数据，携带intent。
override fun onActivityResult(requestCode: Int, resultCode: Int, data: Intent?) {
    super.onActivityResult(requestCode, resultCode, data)
    //requestcode是判断数据来源
    when (requestCode) {
        //resultcode是判断处理结果是否成功与否。
        1 -> if (resultCode == Activity.RESULT_OK) {
            val returnedData = data?.getStringExtra("data_return")
            //将data值打印出来
            Log.d("MainActivity", "returned data is $returnedData")
        }
    }
 }
```