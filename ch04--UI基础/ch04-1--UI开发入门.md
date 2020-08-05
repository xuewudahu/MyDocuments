# UI开发入门

#### **常用的控件**

​	1、TextView
​		match_parent ->当前控件大小与父布局相同    android:gravity->指明文字对齐方式
	2、Button
		Android默认将英文字母转换成大写，android:textAllCaps设置为false保留指定原始文件内容
	3、EditText
​		输入和编辑文本  Android：hint->提示性文本  Android：maxLInes ->最大行数

获取文本的内容方式：id名.text.toString 

​	4、ImageView
		图片通常放在Drawable开头的目录中，并附上分辨率，图片的命名必须以字母开头

```kotlin
 imageView.setOnClickListener{
            imageView.setImageResource(R.drawable.w)
        }
```


​        可以在activity中替换图片	

##### **PrograssBar**

​		进度条 Android：max -> 给进度条的最大值

```kotlin
        //android:visibility有三个可选值：visiable（控件可见，默认）、invisible（空间不可见，仍占据原来未知）
        // 和gone（控件不仅不可见，也不再占用之前的屏幕），在使用setVisibility使用View.可选值
        R.id.btn_click03 -> {
            //使用getVisibility判断prograssBar是否可见，如果可见隐藏，否则显示
            if (progress_bar01.visibility == View.VISIBLE) {
                //调用了setVisibility
                progress_bar01.visibility = View.GONE
            } else {
                progress_bar01.visibility = View.VISIBLE
            }
        }
        //进度条增加
        R.id.btn_click04 -> {
            progress_bar01.progress += 10
        }
```
​	AlertDialog
​		对话框，屏蔽所有控件的交互

#### 基本布局

​	1、LinearLayout
​		线性布局 Android：orientation=“horiziontal”时（默认值）只有垂直方向的对齐方式可以改变

​        Android：gravity-> 文字的对齐方式
​        Android：layout_gravity -> 布局对齐方式

​	2、RelativeLayout
​		相对定位方式
	3、FrameLayout
​		帧布局
​			所有的控件都是在布局的左上角

#### 自定义布局

​	1、没有相应事件时
​		先在title.xml 中制作一个布局然后在activity.xml中引入title布局

```kotlin
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
android:layout_width="match_parent"
android:layout_height="match_parent">

<include layout="@layout/title" />
 
</LinearLayout>
```
​	2、有相应事件时
​		有title.xml 和 TitleLayout类 （继承LinearLayout类）在类中书写相应事件，在Activity_main.xml中引入TitleLayout类

```kotlin
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout 				      xmlns:android="http://schemas.android.com/apk/res/android"
android:layout_width="match_parent"
android:layout_height="match_parent">
 
<!--    <include layout="@layout/title" />-->
<com.example.myapplication.TitleLayout
    android:layout_width="match_parent"
    android:layout_height="wrap_content" />
</LinearLayout>
```