# **kotlin的高阶函数**（第六章）

#### 函数类型

1、一个函数接收另一个函数作为参数或返回值的类型是另一个参数

涉及到了一个概念：函数类型    基本规则：（string,Int) ->Unit       

（string,Int) 是函数类型的参数部分           Unit 是参数类型的返回值类型



2、函数体要加入函数类型参数的具体值。函数类型的实现是在调用的时候进行实现

实现的方法可以是Lambda表达式和匿名函数和成员引用等，常用的是lambda表达式

```kotlin
fun StringBulider.bulid(block: Stringbulid.() -> Unit): stringBulider{

     block()

     return this

}
```

和apply函数相似，其中用到了lambda表达式中的上下文都是StringBulid类。

#### **内联函数**

1、加入inline，可以使运行时的开销完全消失

```kotlin
inline fun num1andnum2_cross(num1: Int, num2: Int, operation: (Int, Int) -> Int): Int {
    val result = operation(num1, num2)
    return result
}
```

 内联函数的原理如下：首先将Lambda表达式的代码替换到函数类型参数调用的地方；其次，在将内联函数中的全部代码替换到函数调用的地方。这样就能完全消除开销。 



noinline和crossinline

1、内联的函数类型参数只允许传递给另外一个内联函数

2、内联函数所引用的Lambda表达式中可以使用return关键字进行函数返回

noinline可以取消内联的函数类型

crossinline可以保证内联的函数Lambda表达式一定不使用return关键字