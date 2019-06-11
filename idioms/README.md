# Idioms
## Những idiom trong Kotlin

### 1. Tạo CTO ( POJOs / POCOs )
* **POJOs** là Plain Old Java Object nghĩa là nó là 1 kiểu pure data stucture. Nó chỉ có các thuộc tính và các phương thức getter, setter và các phương thức mặc định và có thể overide một số phương thức mặc định của object ( equal(), hashCode(), toString()...) hoặc 1 số interface khác như là Serializable, nhưng nó không chứa bất kỳ các method khác ngoài các phương thức mặc định
> Cách tạo với Java
```java
class Point {
    private double x;
    private double y;
	public double getX() { return x; }
    public double getY() { return y; }
    public void setX(double v) { x = v; }
    public void setY(double v) { y = v; }
    public boolean equals(Object other) {...}
}
```

> Cách tạo siêu ngắn gọn với Kotlin

```kotlin
data class Customer(val name: String, val email: String)
```

### 2. Gán giá trị mặc định cho tham số của hàm
```kotlin
fun foo(a: Int = 0, b: String = "") { ... }
```

### 3. Filter trong collection 
```kotlin
// Cách 1

val positives = list.filter { x -> x > 0 }

// Cách 2: ngắn hơn

val positives = list.filter { it > 0 }
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
	println("${s1}") // output: a is 1
    a = 2
    // arbitrary expression in template:
    val s2 = "${s1.replace("is", "was")}, but now is $a"
    println(s2) // output: a was 1, but now is 2
}
```


### 5. Kiểm tra kiểu instance 
```kotlin
when (x) {
    is Foo -> ...
    is Bar -> ...
    else   -> ...
}
```

### 6. Duyệt một cái map/list 
```kotlin
for ((k, v) in map) {
    println("$k -> $v")
}

// k, v có thể là 
```

### 6. Range
```kotlin
for (i in 1..100) { ... }  // closed range: [1:100] includes 100 
for (i in 1 until 100) { ... } // range:  [1:100) does not include 100
for (x in 2..10 step 2) { ... } 
for (x in 10 downTo 1) { ... }
if (x in 1..10) { ... }
```

### 7. Read only List, Map

* List

```kotlin
val list = listOf("a", "b", "c")
```

* Map

```kotlin
val map = mapOf("a" to 1, "b" to 2, "c" to 3)
```

### 8. Lazy 
```kotlin
val p: String by lazy {
    // compute the string
}
```

### 9. Function mở rộng
* When là thứ được cải tiến rất hay so với switch case phiên bản cũ
```kotlin
fun String.spaceToCamelCase() { ... }

"Convert this to camelcase".spaceToCamelCase()
```

### 10. Tạo 1 Singleton
```kotlin
object Resource {
    val name = "Name"
}
//output: fits in range
```

### 11. Cách check not null ngắn gọn

> Thay vì phải if(files != null) thì
```kotlin
val files = File("Test").listFiles()

println(files?.size)
```

### 12. Cách check not null và else ngắn gọn
```kotlin
val files = File("Test").listFiles()

println(files?.size ?: "empty")
```


### 13. Thực thi câu lệnh if null
```kotlin
val values = ...
val email = values["email"] ?: throw IllegalStateException("Email is missing!")
```

### 14. Lấy item đầu tiên nếu   

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



