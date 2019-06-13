# [Class và Inheritane](https://kotlinlang.org/docs/reference/classes.html)
### 1. Class 
Trong Kotlin, Class được định nghĩa bằng sử dụng keyword **class**
```kotlin
class Invoice { ... }
```
Class định nghĩa bao gồm 
* tên class
* class header(các parameter và constructor...)
* class body được định nghĩa trong {...}
* class header và class body là _optional_

Chúng ta vẫn có thể khai báo 1 class non-header non-body
```kotlin
class Empty
```

### 2. Constructor
Một class có 1 hàm khởi tạo chính và một hoặc nhiều hàm khởi tạo phụ

> Hàm khởi tạo chính là một phần của Class Header
- Cách khai báo hàm khởi tạo chính
```kotlin
class Person constructor(firstName: String) { ... }
```
Nếu hàm khởi tạo chính không có bất kỳ các annotation hoặc modifier, thì từ khóa **constructor** có thể được bỏ đi vẫn không làm thay đổi bản chất của nó
```kotlin
class Person(firstName: String) { ... } 
```

Hàm khởi tạo chính không thể chứa bất kỳ code nào. Code dùng để khởi tạo có thể đặt trong **initializer blocks** với _prefix_ là **init** keyword

Trong lúc đang khởi tạo 1 instance thì _initializer blocks_ được thực thi cùng với lúc chúng xuất hiện trong class body, và nếu có nhiều hơn 1 _initializer blocks_ thì nó sẽ xếp chồng lên nhau để khởi tạo
```kotlin
class InitOrderDemo(name: String) {
    val firstProperty = "First property: $name".also(::println)
    
    init {
        println("First initializer block that prints ${name}")
    }
    
    val secondProperty = "Second property: ${name.length}".also(::println)
    
    init {
        println("Second initializer block that prints ${name.length}")
    }
}   

/*
output: 
        First property: hello
        First initializer block that prints hello
        Second property: 5
        Second initializer block that prints 5
*/
```
> Ngoài cách init trong block, thì còn 1 cách để init các properties của class được định nghĩa trong class body

```kotlin
class Customer(name: String) {
    val customerKey = name.toUpperCase()
}
```

> Các _properties_ định nghĩa trong hàm khởi tạo chính có thể mutable (**var**) hoặc read-only(**val**)
```kotlin
class Person(val firstName: String, val lastName: String, var age: Int) { ... }
```

* Nếu _constructor_ có những _annotation_ hoặc _visibility modifier_, thì từ khóa **constructor** là bắt buộc

```kotlin
class Customer public @Inject constructor(name: String) { ... }
```

### 3. Secondary Constructors (Hàm khởi tạo phụ)
> Ví dụ về cách khai báo 1 secondary constructor, với prefix là **constructor** định nghĩa trong body class

```kotlin
class Person {
    constructor(parent: Person) {
        parent.children.add(this)
    }
}
```

> Nếu class có hàm khởi tạo chính và mỗi hàm khởi tạo phụ cần được delegate (ủy quyền) tới hàm khởi tạo chính, cách trực tiếp hoặc gián tiếp truyền hàm khởi tạo phụ. Để mà delegate các param từ hàm khởi tạo chính xuống hàm khởi tạo phụ sử dụng từ khóa **_this_**

```kotlin
class Person(val name: String) {
    constructor(name: String, parent: Person) : this(name) {
        parent.children.add(this)
    }
}
```
> Private constructor
```kotlin
class DontCreateMe private constructor () { ... }
```

> Default param cho constructor 
```kotlin
class Customer(val customerName: String = "")
```

### 3. Tạo instance của class
> Tạo 1 instance không cần thiết phải dùng từ khóa **new**
```kotlin
val invoice = Invoice()

val customer = Customer("Joe Smith")
```

### 4. Class member
> Classes can contain:
- Constructors and initializer blocks
- Functions
- Properties
- Nested and Inner Classes
- Object Declarations

### 5. Inheritance
> Mặc định tất cả các class kế thừa class **Any**
* Cách định nghĩa kế thừa 1 class
```kotlin
open class Base(p: Int)

class Derived(p: Int) : Base(p)
```




