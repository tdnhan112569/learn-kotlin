# [Kiểu dữ liệu](https://kotlinlang.org/docs/reference/basic-types.html)
## Trong hệ sinh thái Kotlin, mọi thứ đều là Object đó là lý do vì sao chúng ta có thể gọi các method và các thuộc tính của bất kỳ kiểu dữ liệu gì. Trong section này chúng ta sẽ tìm hiểu về các kiểu dữ liệu numbers, characters, booleans, arrays và string
### 1. Numbers
* Kotlin handle number giống như Java, nhưng không giống nhau hoàn toàn.

> Các kiểu number trong Kotlin
```java
/*
    Type        Bit   
  --------- ---------
   Double       64 
   Float        32
   Long         64
   Int          32
   Short        16
   Byte          8
*/
```

### 2. Ký tự khởi tạo các kiểu dữ liệu
```kotlin
Decimals: 123
Longs are tagged by a capital L: 123L
Hexadecimals: 0x0F
Binaries: 0b00001011
NOTE: Octal literals are not supported.

Kotlin also supports a conventional notation for floating-point numbers:

Doubles by default: 123.5, 123.5e10
Floats are tagged by f or F: 123.5f
```

### 3. Chúng ta có thể thêm **underscore** để tăng tính dễ đọc cho hằng số
```kotlin
val oneMillion = 1_000_000
val creditCardNumber = 1234_5678_9012_3456L
val socialSecurityNumber = 999_99_9999L
val hexBytes = 0xFF_EC_DE_5E
val bytes = 0b11010010_01101001_10010100_10010010
```

### 4. Representation
> Trên nền tảng Java, Number được lưu trữ vật lý trên JVM theo dạng biến primite.

> Trừ khi chúng ta cần reference tới **1 nullable Number** hoặc kiểu **generics**.

> Với kotlin, biến nullable Number được **Boxed** 

> Boxing của Number không cần thiết quan tâm đến tính _(preserve)_ bảo toàn  _(identity)_ đồng nhất 

> toàn tử **===** ở đây là so sánh boxing của 2 biến nullable number
```kotlin
fun main() {
    val a: Int = 10000
    println(a === a) // Prints 'true'
    val boxedA: Int? = a
    val anotherBoxedA: Int? = a
    println(boxedA === anotherBoxedA) // !!!Prints 'false'!!!
}
```
> Nó bảo toàn sự cần bằng giá trị của biến nullable number, toán tử **==** ở đây là so sánh giá trị của biến nullable number
```kotlin
fun main() {
    val a: Int = 10000
    println(a == a) // Prints 'true'
    val boxedA: Int? = a
    val anotherBoxedA: Int? = a
    println(boxedA == anotherBoxedA) // Prints 'true'
}
```

### 5. Explicit Conversions (Convert biến một cách rõ ràng)
> Bởi vì chúng khác nhau Representation, kiểu có số bit nhỏ không phải là subtype của một kiểu có số bit lớn. Nghĩa là chúng ta không thể assign giá trị từ kiểu Byte sang 1 kiểu Int mà không dùng theo cách Explicit Conversions


```kotlin
// wrong way
/*
 fun main() {
    val b: Byte = 1 // OK, literals are checked statically
    val i: Int = b // ERROR
 }
*/

//true way
fun main() {
    val b: Byte = 1
    val i: Int = b.toInt() // OK: explicitly widened
    print(i)
}
```

> Các kiểu number đều support theo explicitly conversion
*  toByte(): Byte
*  toShort(): Short
*  toInt(): Int
*  toLong(): Long
*  toFloat(): Float
*  toDouble(): Double
*  toChar(): Char

> Ngoài ra vẫn có implicit conversions (convert ngầm định) trong toán tử số học
```kotlin
val l = 1L + 3 // Long + Int => Long
```

### 6. Character
```kotlin
fun decimalDigitValue(c: Char): Int {
    if (c !in '0'..'9')
        throw IllegalArgumentException("Out of range")
    return c.toInt() - '0'.toInt() // Explicit conversions to numbers
}
```

### 6. Boolean
> Kiểu **Boolean** biểu diễn giá trị true, false
> **Boolean** là một boxed
```kotlin
val a: Boolean = true
val b: Boolean? = true
```

### 7. Array 
> Cấu trúc của class Array
```kotlin
class Array<T> private constructor() {
    val size: Int
    operator fun get(index: Int): T
    operator fun set(index: Int, value: T): Unit

    operator fun iterator(): Iterator<T>
    // ...
}
```

* Để tạo 1 array dùng arrayOf() và pass value vào đó => arrayOf(1, 2, 3)
* Để tạo 1 array có thể put giá trị null => arrayOfNulls(1, null, 2)

```kotlin
fun main() {
    // Creates an Array<String> with values ["0", "1", "4", "9", "16"]
    val asc = Array(5) { i -> (i * i).toString() }
    asc.forEach { println(it) }
    for( item in asc) {

    }
}
```

 Ngoài ra Kotlin còn hỗ trợ tạo mảng theo các kiểu biến primitive ByteArray, ShortArray, IntArray  
```kotlin
val x: IntArray = intArrayOf(1, 2, 3)
x[0] = x[1] + x[2]
```

### 8. String 
* String là immutable
* String có thể thao tác như một mảng ký tự, truy cập vào phần tử tại index s[i]
* String có thể nối chuỗi bằng toán tử **+** và cũng có thể nối với các giá trị của kiểu dữ liệu khác

```kotlin
fun main() {
val str = "abcd"
    for (c in str) {
        print(c) // output: a b c d
    }

    val s = "abc" + 1
    print(s + "def") // output: abc1def
}
```

> Khởi tao 1 cái String có thể chứa nhiều nhiều dòng dùng toán tử ***"""***

```kotlin
fun main() {
val text = """
    for (c in "foo")
        print(c)
"""
}
```

### 9. String Templates
* When là thứ được cải tiến rất hay so với switch case phiên bản cũ
```kotlin
fun main() {
    val i = 10
    println("i = $i") // prints "i = 10"
}
```
hoặc
```kotlin
fun main() {
    val s = "abc"
    println("$s.length is ${s.length}") // prints "abc.length is 3"

    val price = """
            ${'$'}9.99
            """

}
```
