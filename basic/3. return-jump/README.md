# [Return, Jump](https://kotlinlang.org/docs/reference/return.html)
### 1. Kotlin có 3 expresssion jump
 - **return**: mặc định sẽ return ở trong thân hàm của function gần nhất
 - **break**: Mặc định kết thúc khỏi vòng lập gần nhất
 - **continue**: Thực hiện bước lặp tiếp theo trong thân vòng lặp gần nhất

### 2. Break và Continue với lable@
Bất kỳ expression trong kotlin đều có thể được mark với 1 cái lable.

> Lable như là một định dạnh cho expression, [cách đặt tên cho đúng](https://kotlinlang.org/docs/reference/grammar.html#label) 
* Ex: abc@ , loop@...
>  Để đặt lable cho 1 expression chỉ cần đặt lable đó trước expression
```kotlin
loop@ for (i in 1..100) {
    // ...
}
```
> Điều thú vị ở đây là ví dụ bạn có thể _break_ hay _continue_ với lable 
```kotlin
loop@ for (i in 1..100) {
    for (j in 1..100) {
        if (...) break@loop
    }
}
```
Với vòng loop trên chỉ cần thỏa điều kiện để _**break**_ với lable là **@loop** thì nó sẽ break đến đúng với cấp expression đang có lable **@loop** 

### Return với lable
Với function literal ,local function, object expression, các function có thể được định nghĩa lồng vào nhau trong kotlin. Hạn chế này là **return** mặc định sẽ thực thi với scope từ 1 outer function.

>Để hiểu về outter ta sẽ có 1 ví dụ như sau:
```kotlin
fun foo() {
    listOf(1, 2, 3, 4, 5).forEach {
        if (it == 4) return 
      println("in the loop()")
    }
    println("Complete")
}

fun main() {
    foo()
}

/*
    Output: 
       in the loop()
       in the loop()
       in the loop()
*/
```

> **return** expression sẽ return ở scope hàm gần nhất chứa nó

Vậy để giới hạn hay cân chỉnh scope của return ta có những solution như sau
* Return với lable trong lambda expression
```kotlin
fun foo() {
    listOf(1, 2, 3, 4, 5).forEach lit@{
        if (it == 3) return@lit 
        // local return to the caller of the lambda, i.e. the forEach loop
        print(it)
    }
    print(" done with explicit label")
}

fun main() {
    foo()
}
// output: 1245 done with explicit label 
```
* Hoặc có thể dùng implicit labels: như là 1 cái lable có cùng tên  function có lambda expression được truyền vào
```kotlin
fun foo() {
    listOf(1, 2, 3, 4, 5).forEach {
        if (it == 3) return@forEach 
        // local return to the caller of the lambda, i.e. the forEach loop
        print(it)
    }
    print(" done with implicit label")
}
```

* Cách khác ta có thể dùng anonymous function. 
> **Một _return_ trong 1 anonymous function sẽ return trong bản thân nó**
```kotlin
fun foo() {
    listOf(1, 2, 3, 4, 5).forEach(fun(value: Int) {
        if (value == 3) return  
    // local return to the caller of the anonymous fun, i.e. the forEach loop
        print(value)
    })
    print(" done with anonymous function")
}
```
* 3 cách ở trên cũng giống chức năng như là lệnh continue của các vòng loop thông thường
> Cách sau không tương đương như **_break_**, nhưng nó có thể tương tự như cách thêm 1 cái biểu thức lambda lồng bên ngoài và non-locally return từ trong biểu thức lambda trong cùng

```kotlin
fun foo() {
    run loop@{
        listOf(1, 2, 3, 4, 5).forEach {
            if (it == 3) return@loop 
            // non-local return from the lambda passed to run
            print(it)
        }
    }
    print(" done with nested loop")
}

fun main() {
    foo()
}

//output: 12 done with nested loop
```




