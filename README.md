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

fun getStringLength(obj: Any): Int? {
    if (obj !is String) return null

    // `obj` is automatically cast to `String` in this branch
    return obj.length
}

// output

/*
    'Incomprehensibilities' string length is 21 
    '1000' string length is ... err, not a string 
    '[java.lang.Object@3af49f1c]' string length is ... err, not a string 
*/
```

### 7. Loop 
```kotlin
fun main() {
    val items = listOf("apple", "banana", "kiwifruit")
    for (item in items) {
        println(item)
    }
}

/*
 - Output
        apple
        banana
        kiwifruit
*/
```
Hoặc là
```kotlin
fun main() {
    val items = listOf("apple", "banana", "kiwifruit")
    for (index in items.indices) {
        println("item at $index is ${items[index]}")
    }
}

/*
 - Output
        item at 0 is apple
        item at 1 is banana
        item at 2 is kiwifruit
*/
```

### 8. Sử dụng While
```kotlin
val items = listOf("apple", "banana", "kiwifruit")
var index = 0
while (index < items.size) {
    println("item at $index is ${items[index]}")
    index++
}

/*
    item at 0 is apple
    item at 1 is banana
    item at 2 is kiwifruit
*/
```

### 9. Sử dụng biểu When
* When là thứ được cải tiến rất hay so với switch case phiên bản cũ
```kotlin
fun describe(obj: Any): String =
    when (obj) {
        1          -> "One"
        "Hello"    -> "Greeting"
        is Long    -> "Long"
        !is String -> "Not a string"
        else       -> "Unknown"
    }

fun main() {
    println(describe(1))
    println(describe("Hello"))
    println(describe(1000L))
    println(describe(2))
    println(describe("other"))
}
/*
    Output
        One
        Greeting
        Long
        Not a string
        Unknown
*/
```

### 10. Sử dụng Ranges
> Sử dụng range trong vòng lặp sử dụng toán tử **in**
```kotlin
fun main() {
    val x = 10
    val y = 9
    if (x in 1..y+1) {
        println("fits in range")
    }
}

//output: fits in range
```


> Kiểm soát over index size
```kotlin
fun main() {
    val list = listOf("a", "b", "c")

    if (-1 !in 0..list.lastIndex) {
        println("-1 is out of range")
    }
    if (list.size !in list.indices) {
        println("list size is out of valid list indices range, too")
    }
}

/*
    -1 is out of range
    list size is out of valid list indices range, too
*/
```

> Lập trong khoảng
```kotlin
fun main() {
    for (x in 1..5) {
        print(x)
    }
}
// output: 12345
```

> Lặp kết hợp với **step**
* x **in** 1..10: nghĩa là x chạy trong range từ [1:10]
* x **in** 9 **downTo** 0: nghĩa là chạy giảm dần trong range [9:0]
* **step** là bước nhảy

```kotlin
fun main() {
    for (x in 1..10 step 2) {
        print(x)
    }
    println()
    for (x in 9 downTo 0 step 3) {
        print(x)
    }
 
/*
  // output
        13579
        9630
*/ 
}
```

### 11. Collection

> Lặp với Collection
```kotlin
fun main() {
    val items = listOf("apple", "banana", "kiwifruit")
    for (item in items) {
        println(item)
    }
}
/*
  //output:
        apple
        banana
        kiwifruit
*/
```

> Check collection chứa trong 1 object sử dụng toán tử **in**
```kotlin
fun main() {
    val items = setOf("apple", "banana", "kiwifruit")
    when {
        "orange" in items -> println("juicy")
        "apple" in items -> println("apple is fine too")
    }
}

// output: banana
```

> Loop, filter, map, sort với collection
```kotlin
fun main() {
  val fruits = listOf("banana", "avocado", "apple", "kiwifruit")
  fruits
    .filter { it.startsWith("a") }
    .sortedBy { it }
    .map { it.toUpperCase() }
    .forEach { println(it) }
}
// output: APPLE 
//         AVOCADO
```

### 12. Khai báo class và khởi tạo instance của class đó
```kotlin

abstract class Shape (val sides : List<Double>) {
    abstract fun calculateArea(): Double
    val perimeter : Double get() = sides.sum()
}

interface RetangleProperties {
    val isSquare : Boolean
}

class Retangle (
   var height : Double,
   var width  : Double 
) : Shape (listOf(height, width, height, width)), RetangleProperties {
    overide fun calculateArea () : Double = height * width
    overide val isSquare : Boolean get() = height == width
}

class Triangle (
   var sideA : Double,
   var sideB : Double,
   var sideC : Double
) : Shape (listOf(sideA, sideB, sideC)) {
    overide fun calculateArea() : Double {
        var s = perimeter / 2
        return Math.sqrt( s * (s - sideA) * s * (s - sideB) * s * (s - sideC))
    }
}

fun main() {
    // Không cần phải dùng từ khóa new 
    val retangle = Retangle( 5.0, 2.0 ) // Khởi tạo 1 intance
    println("Area : ${retangle.calculateArea()}") // 10.0
    println("perimeter : ${retangle.perimeter}")  // 14.0

    val triangle = Triangle( 3.0, 4.0, 5.0 )
    println("Area : ${retangle.calculateArea()}") // 6.0
    println("perimeter : ${retangle.perimeter}")  // 12.0
}
```



