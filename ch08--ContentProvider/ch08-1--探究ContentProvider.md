## 探究ContentProvider

#### 1.运行时的权限

 在android6.0 之后加入运行时权限，android100多权限共有30个危险权限，需要运行时权限处理，就是在使用到该权限是用户进行选择。运行时权限不仅需要在androidmani.xml 中注册还需要在代码中体现。

先是需要判断是否授权，确保第二次使用时不需要再次判断。

```kotlin
   make_call.setOnClickListener {
        //1.首先判断用户是不是已经授权过了，借助checkSelfPermission函数，第一个参数是context，第二个是权限名，然后该返回值与PERMISSION_GRANTED比较。
        if (ContextCompat.checkSelfPermission(this, android.Manifest.permission.CALL_PHONE) != PackageManager.PERMISSION_GRANTED) {
            //未曾授权
            //ActivityCompat.requestPermissions申请授权，第一个参数是Activity实例，第二个String数组，申请的权限名放入数组即可；第三个参数是请求码，确保唯一即可。
            ActivityCompat.requestPermissions(this, arrayOf(android.Manifest.permission.CALL_PHONE), 1)
        } else {
            //已经授权
            call()
        }
    }
}
//调用完requestPermissions自动弹出对话框申请权限，无论同意与否都会调用onRequestPermissionsResult方法
//grantResults是授权结果
override fun onRequestPermissionsResult(requestCode: Int, permissions: Array<out String>, grantResults: IntArray) {
    super.onRequestPermissionsResult(requestCode, permissions, grantResults)
    when (requestCode) {
        1 -> {
            if (grantResults.isNotEmpty() && grantResults[0] == PackageManager.PERMISSION_GRANTED) {
                call()
            } else {
                Toast.makeText(this, "You denied the permission", Toast.LENGTH_SHORT).show()
            }
        }
    }
}
 
private fun call() {
    try {
        //构建隐式Intent，Intent的action指定为系统内置打电话工作，data部分指定了协议为tel，号码是10086.
        //实现拨打电话的功能。ACTION_CALL是直接拨打电话，需要权限，ACTION_DIAL的是到拨号界面，无需权限。
        val intent = Intent(Intent.ACTION_CALL)
        intent.data = Uri.parse("tel:10086")
        startActivity(intent)
    } catch (e: SecurityException) {
        e.printStackTrace()
    }
}
}
```
#### 2.访问其他程序的数据   ContentResolver

使用了内容URl

比如读取联系人的姓名和手机号，需要使用运行时权限，不仅需要在androidmina.xml 中注册权限还需要在代码中体现。

在Activity中调用

```kotlin
contentResolver.query(ContactsContract.CommonDataKinds.Phone.CONTENT_URI, null, null, null, null)?.apply {

​            //通过移动游标来遍历Cursor的所有行，取出每一列中相应行的数据

​            while (moveToNext()) {

​               //获取联系人姓名

​               val displayname = getString(getColumnIndex(ContactsContract.CommonDataKinds.Phone.DISPLAY_NAME))

​                //获取联系人号码

​                val number =  getString(getColumnIndex(ContactsContract.CommonDataKinds.Phone.NUMBER))

​                contactsList.add("$displayname\n$number")

​            }

​            //刷新一下ListView

​            adapter.notifyDataSetChanged()

​            //将Cursor对象关闭

​            close()
```

#### 3.创建自己的ContentProvider

创建之后可以供其他程序通过ContentResolver来查询到程序的数据，

创建MyProvity类

需要重写oncreate() insert() query() delete() update()  gettype()  方法