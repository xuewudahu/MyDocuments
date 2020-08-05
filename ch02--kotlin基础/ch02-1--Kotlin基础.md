# Kotlin基础

#### 1、变量与函数

##### 	变量

​		1、变量的两种关键字：val和var。val：初始值赋值后不能改变，默认使用val关键字。
		2、val a=10  a自动推导为Int类型  这就是kotlin的类型推导机制。
		3、在kotlin中Int等类型变成了一个类，拥有自己的方法和继承结构。
		4、if（(nextLine = bufferedReader.readLine()) != null ）Kotlin不提倡将赋值运算符当做表达式，所以会禁止此举动。

<img src="https://s1.ax1x.com/2020/08/05/ayOBCD.png" style="zoom:80%">

                   Kotlin具有出色的类型推导机制。需要注意的是，Kotlin代码结尾是不加分号的。 

##### 	函数

​                函数的格式：

```
  fun 函数名（参数名：参数类型）：参数类型{
    函数体
  }
```

​		1、函数使用fun关键字定义，参数格式：参数名：参数类型。返回类型在括号后加：返回类型。
		2、Kotlin函数的语法糖，当函数中只有一行代码时，可以省略函数体，用=号连接，可以省略return。由于kotlin的类型推导机制还可以省略返回值
		3、有待确定的点：函数中的参数在函数体内不能赋值。

#### 2、逻辑控制

##### 	if语句

​		1、if语句可以有返回值，每个条件的最后一行。
		2、当函数中只有if语句时，可以使用语法糖，使用=号，省略返回值。

```kotlin
fun largerNumber(num1: Int, num2: Int): Int {`
    var value = 0
    if (num1 > num2) {
        value = num1
    } else {
        value = num2
    }
    return value
}
```

##### 	when语句

​		1、when语句也有返回值，函数只有when语句也可以使用语法糖。
​		2、when的格式：when（参数）{ “ ”-> 返回的值 }
​		3、is可以判断类型，可以和when搭配使用，Number是Int、Float、Double等数字相关的类的子类。
		4、另一种表达式：when{ name.startsWith("Tom") ->78 name=="jim" ->23   } 无参数的表达，某种情况必须使用。

```kotlin
//is 语句判断型
fun checkNum(num:Number){
    when(num){
        is Int -> println("number is int")
        is Double -> println("number is double")
        else -> println("not support")
    }
}
```

```kotlin
//无参数型
fun getScore2(name: String) = when {
    name == "Tom" -> 86
    name == "Jim" -> 77
    name == "Jack" -> 95
    name == "Lily" -> 100
    else -> 0
}
```

##### 	for语句和while语句

             Java提供了while循环和for循环两种。Kotlin也提供了这两种：前者与Java用法相同，for循环有所不同。     

             Kotlin提出了区间的概念，val range = 0..10。创建了一个0-10的区间，数学表示为[0,10]，两个端点包含其中。最简单的for循环遍历如下所示。 

```kotlin
 fun main() {
    for (i in 0..10) {
        println(i)
    }
}
```

#### 3、面向对象

##### 	继承

​		1、kotlin中的类默认不可以被继承，需要添加open关键字才可以继承。
​		2、继承时需要注意父类添加括号，必要时加入参数。

##### 	构造函数

​		1、分为主构造函数和次构造函数 参数可以用val和var，当父类不是无参构造函数时，需要在主函数中就需要添加父类的参数，这时就不需要加val和var
​		2、需要在主构造函数添加逻辑时可以在init中添加。
​		3、次级构造函数可以有参数默认值取代。当没有主构造函数只有次级构造函数时不需要再父类添加括号。

##### 	接口

​		1、实现接口和继承父类都是使用：号多个使用，隔开
​		2、接口中的函数可以有默认实现，实现者可以自由选择实现和不实现。
	可见修饰符
​		1、四种：public、private、protected和internal。
​		2、kotlin默认使用public修饰符。

##### 	数据类和单例类

​		数据类
​			1、加上data，可以自动重写了equals（）、hashCode（）和toString（）方法。
		单例类
​			1、加入object，该类就是单例类了，使用类.方法来使用。

#### 4、集合

​	ListOf（）
		1、ListOf（）不可变集合，mutableListOf（）可变集合

```kotlin
 val list2 = listOf("Apple", "Banana", "Orange", "Pear")
```

​	SetOf（）
		1、setOf（）不可变集合，mutableSetOf（）可变集合

​	HashMap（）
​		1、map["apple"]=1  1、mapOf（）不可变集合，mutableMapOf（）可变集合
		2、mapof（“apple”to 1，“banana”to 2 ）使用了infix函数。

```kotlin
val map = mapOf("Apple" to 1, "Banana" to 2, "Pear" to 3)
for ((fruit, number) in map) {
    println("fruit is " + fruit + ",number is " + number)
}
```
##### **空指针检查**

​	1、kotlin默认所有的参数和变量都不可为空，想要为空，可以加入？号
	2、let函数

#### 5、lambda表达式

##### 	集合的函数式API

​		1、lambda表达式格式：{参数名1：参数类型，参数名2：参数类型->函数体}     作为一小段可以作为参数传递的代码。
​		list.maxBy{it.length}  集合中长度最长的字符    maxBy函数遍历集合来找到该条件的最大值。
​		list.map{it.toUpperCase()}  集合中的字符全部变成大写。  map函数将集合中的每个元素都映射到另外一个值。
		list.filter{it.length<=5} 集合中的字符长度小于5的过滤掉    filter过滤集合数据

```kotlin
val maxLengthFruit = list.maxBy { it.length }  
```



##### 	java函数式API

​		1、一个接口只有一个抽象方法，称为函数式接口，SAM接口（SAM代表单抽象方法）                                                                                                                                                                                                                                                                                                                                                                                                                                       
		2、举例：Thread{ println("Thread is runing")}.start()

```kotlin
public interface Runnable{
    void run();
}
```

#### 6、字符串内嵌表达式和默认参数

##### 	字符串内嵌表达式

​		${}当只有一个变量时，可以省略{}符号

##### 	默认参数

​		1、当有参数有默认值有参数没有默认值时，可以使用键值对的方式传参，str=“world”