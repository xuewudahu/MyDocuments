### kotlin基础知识（第四章）

##### 延迟初始化

```kotlin
class MainActivity : AppCompatActivity(), View.OnClickListener {
private lateinit var adapter: MsgAdapter

override fun onCreate(savedInstanceState: Bundle?) {
...
        adapter = MsgAdapter(msgList)
....
}

override fun onClick(v: View?) {
...
                adapter.notifyItemInserted(msgList.size - 1) 
...
                }
            }
        }
    }
```

adapter就使用了lateinit修饰变成了延迟初始化，可以不赋初值

使用adapter的方法时也不需要使用？.来调用

使用了lateinit就确定在调用方法之前一定会赋值



##### 密封类

```kotlin
sealed class Result

class Success(val msg: String) : Result()

class Failure(val error: Exception) : Result()

fun getResultMsg(result: Result) = when (result) {
    is Success -> result.msg
    is Failure -> "Error is ${result.error.message}"
}
fun main() {
     val s=Success("3")
    val e=Failure(java.lang.RuntimeException)
    println(getResultMsg(s))
    println(getResultMsg(e))
}
```

使用了sealed class修饰Result 变成了一个可继承的类（密封类）

成为密封类后使用when判断是可以不用else，否则会报错

但是又有一个类继承此密封类，在when中就必须添加条件分支，否则会报错。