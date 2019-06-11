# Học Kotlin
## Những syntax mới trong Kotlin

### 1. Khai báo hàm 
> Hàm sẽ truyền vào 2 tham số kiểu **Int** và _return_ kiểu **Int**
```kotlin
fun sum(bienA : Int, bienB : Int) : Int {
    return bienA + bienB
}

fun main() {
    print("Tổng của 5 + 5 là: ")
    println(sum(3, 5))
}
```

> Nếu như trong thân hàm chỉ có 1 statement thì ta có thể viết ngắn gọn như sau
```kotlin
fun sum(a: Int, b: Int) = a + b

fun main() {
    /*
      Ở đây có 1 cú pháp mới truy xuất được tới biến trong chuỗi 
      thông qua qua syntax ${ten_bien hoặc ten_ham}
    */
    println("Tổng của 19 vs 23 là: ${sum(19, 23)}")
}
```

> Hàm _**return**_ về giá trị không có ý nghĩa ( _Data type:_ **Unit** )
```kotlin
fun printSum(a: Int, b: Int): Unit {
    println("Tổng của $a và $b là: ${a + b}")
}

fun main() {
    printSum(-5, 15)
}
``` 
* Nếu như hàm _**return**_ về giá trị với kiểu **Unit** chúng ta có thể bỏ qua việc định nghĩa kiểu trả về thân hàm, mặc định kiểu dữ liệu trả về của hàm sẽ là **Unit**
```kotlin
fun printSum(a : Int, b: Int) : { 
    println("Tổng của $a và $b là: ${a + b}")
}

fun main() {
    printSum(-5, 15)
}
```


### 2. Khai báo biến
> Để định nghĩa 1 _hàng số_ ta sử dụng keyword **val** _(val => value)_
```kotlin
fun main() {

// Cách 1: Gán kiểu và giá trị trực tiếp
    val a: Int = 1 

/* 
   Cách 2: Gán giá trị và kiểu sẽ được compiler tự hiểu dựa vào giá trị của biến
*/
    val b = 2       

 // Cách 3: Khai báo trước và gán giá trị sau
    val c: Int     
    c = 3      
    println("a = $a, b = $b, c = $c")
}
```

> Để định nghĩa 1 biến có thể gán lại giá trị của biến đó thì sử dụng keyword **var** _(var => variable)_
```kotlin
fun main() {
    var x = 5 
    x += 1
    println("x = $x")
}
```


### 3. Comments 
```kotlin
// Comment trên 1 dòng

/* 
  Comment trên nhiều dòng
 */
```


### 4. String template 
> Giờ chúng ta có thể truy xuất được biến hay cả hàm trong chuỗi
> * Sủ dụng **_$ten_bien_** dành cho truy xuất biến trong chuỗi
> * Sử dụng **_${biểu thức} hoặc ${ten_bien}_** dùng cho cả biểu thức và biến
```kotlin
fun main() {
    var a = 1
    // simple name in template:
    val s1 = "a is $a" 
	println("${s1}")
    a = 2
    // arbitrary expression in template:
    val s2 = "${s1.replace("is", "was")}, but now is $a"
    println(s2)
}
```


### 5. Sử dụng những giá trị _nullable_ và checking **null**
> Hàm parseInt sẽ _return **null**_ nếu giá trị **String** input không thể parse sang kiểu **Int**
```kotlin
fun parseInt(str: String): Int? {
    // ...
}
```
> Ví dụ về việc check null 
```kotlin
fun printProduct(arg1: String, arg2: String) {
    val x = parseInt(arg1)
    val y = parseInt(arg2)

    // Sử dụng `x * y` có thể gây lỗi bởi vì chúng có thể đang bị null.
    if (x != null && y != null) {
        // x và y sẽ tự động được cast về thành kiểu non-nullable sau khi 
        //check null
        println(x * y)
    }
    else {
        println("either '$arg1' or '$arg2' is not a number")
    }    
}

// Hoặc

// ...
if (x == null) {
    println("Wrong number format in arg1: '$arg1'")
    return
}
if (y == null) {
    println("Wrong number format in arg2: '$arg2'")
    return
}

// x và y sẽ tự động được cast về thành kiểu non-nullable sau khi 
//check null
println(x * y)
```

### 6. Sử dụng toán tử check Data type và auto cast type
> Toán tử **is** để check 
> * Nếu là 1 expression hoặc là property của kiểu dữ liệu cụ thể nào đó thì nó sẽ hiểu là 1 instance của 1 kiểu đó và nếu thỏa thì auto cast về kiểu đó
```kotlin
fun getStringLength(obj: Any): Int? {
    if (obj is String) {
    // `obj` tự động cast về kiểu `String` nếu nó là 1 instance của `String`
        return obj.length
    }

    // `obj` này vẫn là kiểu `Any`
    return null
}


fun main() {
    fun printLength(obj: Any) {
        println("'$obj' string length is ${getStringLength(obj) ?: "... err, not a string"} ")
    }
    printLength("Incomprehensibilities")
    printLength(1000)
    printLength(listOf(Any()))
}

// output
/*
    'Incomprehensibilities' string length is 21 
    '1000' string length is ... err, not a string 
    '[java.lang.Object@3af49f1c]' string length is ... err, not a string 
*/

fun getStringLength(obj: Any): Int? {
    if (obj !is String) return null

    // `obj` is automatically cast to `String` in this branch
    return obj.length
}
```

### Loop 




