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

### 14. Lấy item đầu tiên nếu không có trả về null
```kotlin
val emails = ... // might be empty
val mainEmail = emails.firstOrNull() ?: ""
```

### 15. Thực thi nếu not null
```kotlin
val value = ...

value?.let {
    ... // execute this block if not null
}
```

### 16. Map nullable value if not null
```kotlin
val value = ...

val mapped = value?.let { transformValue(it) } ?: defaultValueIfValueIsNull
```

### 17. Return với câu lệnh When
```kotlin
fun transform(color: String): Int {
    return when (color) {
        "Red" -> 0
        "Green" -> 1
        "Blue" -> 2
        else -> throw IllegalArgumentException("Invalid color param value")
    }
}
```

### 18. Gán nhanh giá trị cùng câu lệnh if
```kotlin
fun foo(param: Int) {
    val result = if (param == 1) {
        "one"
    } else if (param == 2) {
        "two"
    } else {
        "three"
    }
}
```

### 19. Builder-style usage of methods that return Unit
```kotlin
fun arrayOfMinusOnes(size: Int): IntArray {
    return IntArray(size).apply { fill(-1) }
}
```

### 20. Hàm single-expression
```kotlin
fun theAnswer() = 42
```
> Nó sẽ tương đương với
```kotlin
fun theAnswer(): Int {
    return 42
}
```

### 21. Có thể kết hợp với 1 số idioms, ví dụ như When-expression
```kotlin
fun transform(color: String): Int = when (color) {
    "Red" -> 0
    "Green" -> 1
    "Blue" -> 2
    else -> throw IllegalArgumentException("Invalid color param value")
}
```

### 22. Gọi nhiều hàm object với cú pháp tuyệt cú mèo
```kotlin
class Turtle {
    fun penDown()
    fun penUp()
    fun turn(degrees: Double)
    fun forward(pixels: Double)
}

val myTurtle = Turtle()
with(myTurtle) { //draw a 100 pix square
    penDown()
    for(i in 1..4) {
        forward(100.0)
        turn(90.0)
    }
    penUp()
}
```

### 23. Thao tác với file 
```kotlin
val stream = Files.newInputStream(Paths.get("/some/file.txt"))
stream.buffered().reader().use { reader ->
    println(reader.readText())
}
```

### 24. Convert json
```kotlin
/*
public final class Gson {
     ...
public <T> T fromJson(JsonElement json, Class<T> classOfT) throws JsonSyntaxException {
     ...
*/

inline fun <reified T: Any> Gson.fromJson(json: JsonElement): T = this.fromJson(json, T::class.java)
```

### 25. Tạo 1 biến boolean nullable
```kotlin
val b: Boolean? = ...
if (b == true) {
    ...
} else {
    // `b` is false or null
}
```

### 26. Swap với siêu clean code
```kotlin
var a = 1
var b = 2
a = b.also { b = a }
```

