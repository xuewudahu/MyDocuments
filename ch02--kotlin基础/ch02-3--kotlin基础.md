kotlin基础（第五章）

**扩展函数**

可以在不修改类的源码的情况下添加函数（方法）

使用ClassName.的方式来定义。可以放在同类名的文件夹中，便于以后的查找。

比如在String.kt文件中

```kotlin
fun String.lettersCount(): Int{

   var count=0

   for(char in this){

   if(char.isletter()){

        count++

      }

    }

    return count

}
```

**运算符重载**

像+   -   *   \   ++  %  >   <  in  .. 等运算符和关键字可以重载，在使用该关键字的类中重载，需要添加operator

```kotlin
class Money(val value: Int ){
  operator fun plus(money:Money):Money{
       val sum=value+money.value
       return Money(sum)
   }
  operator fun plus(newValue:In):Money{
      val sum=value+newValue
      return Money(sum)
}
```

Money对象就可以使用+来运算

也可以使用Money和数字使用+运算符