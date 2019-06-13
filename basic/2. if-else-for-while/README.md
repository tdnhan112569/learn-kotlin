# [if, when, for, while](https://kotlinlang.org/docs/reference/control-flow.html)
### 1. Câu lệnh if
```kotlin   
// Traditional usage 
var max = a 
if (a < b) max = b

// With else 
var max: Int
if (a > b) {
    max = a
} else {
    max = b
}
 
// As expression 
val max = if (a > b) a else b
```
* Trong kotlin, gán giá trị theo kiểu mới với lệnh if
```kotlin
val max = if (a > b) {
    print("Choose a")
    a
} else {
    print("Choose b")
    b
}
```


### 2. Cậu lệnh When
> **When** mạnh mẽ hơn và đã khắc phục nhược điểm của switch của các ngôn ngữ khác
```kotlin
when (x) {
    1 -> print("x == 1")
    2 -> print("x == 2")
    else -> { // Note the block
        print("x is neither 1 nor 2")
    }
}
```

> Với những case cùng chung cách xử lý
```kotlin
when (x) {
    0, 1 -> print("x == 0 or x == 1")
    else -> print("otherwise")
}
```

> Có thể sử dụng cả biểu thức hay hàm số thay cho cách dùng hằng số ở switch
```kotlin
when (x) {
    parseInt(s) -> print("s encodes x")
    else -> print("s does not encode x")
}
// hoặc
when (x) {
    in 1..10 -> print("x is in the range")
    in validNumbers -> print("x is valid")
    !in 10..20 -> print("x is outside the range")
    else -> print("none of the above")
}

// Check is hay !is và smart cast có thể gọi các method của kiểu đó
fun hasPrefix(x: Any) = when(x) {
    is String -> x.startsWith("prefix")
    else -> false
}
```

### 3. For loop
```kotlin
for (item in collection) print(item)

for (item: Int in ints) {
    // ...
}
```
> Keyword **step** trong loop, range
```kotlin
for (i in 1..3) {
    println(i)
}
for (i in 6 downTo 0 step 2) {
    println(i)
}
```

> Duyệt theo index 
```kotlin
fun main() {
    val array = arrayOf("a", "b", "c")
        for (i in array.indices) {
            println(array[i])
        }
}
```

> Sử dụng withIndex()
```kotlin
fun main() {
    val array = arrayOf("a", "b", "c")
    for ((index, value) in array.withIndex()) {
        println("the element at $index is $value")
    }
}
```

### 4. While loop

```kotlin
while (x > 0) {
    x--
}

do {
    val y = retrieveData()
} while (y != null) // y is visible here!
```

### 5. Break và Continue
> Kotlin vẫn giữ cách truyền thống như các ngôn ngữ truyền thống như Java, C, C++

