# kotlin基础知识（第九章）

#### **infix函数**

```kotlin
infix fun String.beginsWith(prefix: String)=startsWith(prefix)
```

使用了infix函数，定义了beginsWith函数，具有了startsWith函数的特性，除了可以使用**字符串.beginsWith("asd")**的方法外还可以使用**字符串  beginsWith  “asd”**的方式

使用infix函数的两个限制条件是：

1、不可以是顶层函数（就是在kotlin中直接定义，没有在类中定义的函数），必须是类中的成员函数，可以使用扩展函数来定义。

2、只能接受一个参数，参数类型没有限制。

```kotlin
//判断字符串是否以某个参数开头
if ("Hello Kitty".startsWith("Hello")) {
           //处理逻辑  
}
//将其改写为infix函数形式：
//借助infix函数，使用更可读的写法
if ("Hello Kitty" beginsWith "hello"){
}
//String类的扩展函数，添加一个beginsWith，内部实现基于startsWith方法。
// 加上infix后beginsWith变为infix函数，除了传统的调用方式，还有特殊语法糖格式
infix fun String.beginsWith(prefix: String) = startsWith(prefix)
```

举例：mapof中的to的方式就是使用了infix函数的特性