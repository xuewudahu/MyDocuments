### Fragment

#### 简单用法

​	在layout文件中添加左和右布局
​	添加类继承Fragment类，加载左和右布局 

```kotlin
 override fun onCreateView(inflater: LayoutInflater, container: ViewGroup?, savedInstanceState: Bundle?): View? {
    return inflater.inflate(R.layout.left_fragment, container, false)
}
```
​	在activity_main.xml 中的<fragment>标签中添加左和右类

```kotlin
  	 <fragment
        android:id="@+id/leftFrag"
        android:name="com.example.activitytest.LeftFragment"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:layout_weight="1"/>
```

​	在MainActivity中加载activity_main.xml布局

#### 动态加载Fragment

​	layout文件中添加左、右和另一个右布局
​	Fragment类中添加左、右和另一个右的类
​	Activity_main中动态映射到布局

```kotlin
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
android:layout_width="match_parent"
android:layout_height="match_parent"
android:orientation="horizontal">
<!--使用Fragment标签在布局中添加Fragment，通过android：name来显式声明要添加的Fragment名，注意加上包名-->
<fragment
    android:id="@+id/left_frag"
    android:name="com.example.myapplication.LeftFragment"
    android:layout_width="0dp"
    android:layout_height="match_parent"
    android:layout_weight="1" />
<!--将右侧的Fragment替换为FrameLayout，默认摆在左上角，无需任何定位。-->
<FrameLayout
    android:id="@+id/frame_layout"
    android:layout_width="0dp"
    android:layout_height="match_parent"
    	android:layout_weight="1" />
	</LinearLayout>
```
​	在MainActivity类中实现逻辑来控制布局

```kotlin
class MainActivity : AppCompatActivity() {
 
override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    setContentView(R.layout.activity_main)
    //第一步，创建添加Fragment的实例
    left_button.setOnClickListener{
        replaceFragment(AnotherRightFragment())
    }
    replaceFragment(RightFragment())
}
 
fun replaceFragment(fragment:Fragment){
    //第二步，获取FragmentManager
    val fragmentManager = supportFragmentManager
    //第三步，开启一个事务
    val transaction = fragmentManager.beginTransaction()
    //第四步，向容器内添加或者替换Fragment，一般使用replace来代替，传递id和添加的Fragment实例
    transaction.replace(R.id.frame_layout,fragment)
    //第五步，提交事务
    transaction.commit()
}
}
```
#### Fragment与Activity之间的交互

​	**Activity获取Fragment的实例**
​		 val fragment = supportFragmentManager.findFragmentById(R.id.left_frag) as LeftFragment
​		  val fragment = left_frag as LeftFragment 
​	**Fragment获取Activity的实例**
​		getActivity

```kotlin
    //MainActivity可能为null，因此需要判空处理
    if(activity!=null){
        //获得Activity实例，当需要context对象时，也可以使用getActivity方法
        val mainActivity = activity as MainActivity
    }
```
#### Fragment的生命周期

​	**四种状态**
​		1.运行状态
​			Fragment相关联的Activity运行
​		2.暂停状态
​			当一个Activity进入暂停状态，与之相关的Fragment也暂停
​		3.停止状态
​			Activity停止或者FragmentTransaction的remove和replace移除被调用，但在此之间调用了addToBackStack方法
​		4.销毁状态
​			Activity被销毁或者FragmentTransaction的remove和replace移除被调用。未调用addToBackStack方法
​	**回调方法**
​		1.onAttach()
​			Fragment与Activity()建立联系时
​		2.onCreateView()
​			为Fragment创建视图时
​		3.onActivityCreated()
​			确保与Fragment相关联的Activity已经创建完毕时
​		4.onDestroyView()
​			与Fragment相关联的视图被移除时
​		5.onDetach()
​			两者解除关联时