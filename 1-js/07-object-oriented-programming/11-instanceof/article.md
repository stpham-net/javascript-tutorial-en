# Class checking: "instanceof"

Toán tử `instanceof` cho phép kiểm tra xem một đối tượng có thuộc về một lớp nhất định hay không. Nó cũng có tính kế thừa.

Việc kiểm tra như vậy có thể cần thiết trong nhiều trường hợp, ở đây chúng ta sẽ sử dụng nó để xây dựng hàm *đa hình (polymorphic)*, một hàm xử lý các đối số khác nhau tùy thuộc vào kiểu của chúng.

## The instanceof operator

Cú pháp là:

```js
obj instanceof Class
```

Nó trả về `true` nếu `obj` thuộc về `Class` (hoặc một class kế thừa từ nó).

Ví dụ:

```js
class Rabbit {}
let rabbit = new Rabbit();

// is it an object of Rabbit class?
alert( rabbit instanceof Rabbit ); // true
```

Nó cũng hoạt động với các constructor functions:

```js
// instead of class
function Rabbit() {}

alert( new Rabbit() instanceof Rabbit ); // true
```

...Và với các built-in classes như `Array`:

```js
let arr = [1, 2, 3];
alert( arr instanceof Array ); // true
alert( arr instanceof Object ); // true
```

Xin lưu ý rằng `Array` cũng thuộc về lớp `Object`. Đó là bởi vì `Array` được kế thừa nguyên mẫu từ `Object`.

Toán tử `instanceof` xem xét chuỗi nguyên mẫu (prototype chain) để kiểm tra và cũng có thể tinh chỉnh bằng cách sử dụng static method `Symbol.hasInstance`.

Thuật toán của `obj instanceof Class` hoạt động đại khái như sau:

1. Nếu có một phương thức tĩnh `Symbol.hasInstance`, thì hãy sử dụng nó. Như thế này:

    ```js
    // assume anything that canEat is an animal
    class Animal {
      static [Symbol.hasInstance](obj) {
        if (obj.canEat) return true;
      }
    }

    let obj = { canEat: true };
    alert(obj instanceof Animal); // true: Animal[Symbol.hasInstance](obj) is called
    ```

2. Hầu hết các classes không có `Symbol.hasInstance`. Trong trường hợp đó, kiểm tra nếu `Class.prototype` bằng một trong các prototypes trong `obj` prototype chain.

    Nói cách khác, so sánh:
    
    ```js
    obj.__proto__ === Class.prototype
    obj.__proto__.__proto__ === Class.prototype
    obj.__proto__.__proto__.__proto__ === Class.prototype
    ...
    ```

    Trong ví dụ trên `Rabbit.prototype === rabbit.__proto__`, do đó sẽ đưa ra câu trả lời ngay lập tức.

    Trong trường hợp thừa kế, `rabbit` cũng là một thể hiện của lớp cha:

    ```js
    class Animal {}
    class Rabbit extends Animal {}

    let rabbit = new Rabbit();
    alert(rabbit instanceof Animal); // true
    // rabbit.__proto__ === Rabbit.prototype
    // rabbit.__proto__.__proto__ === Animal.prototype (match!)
    ```

Dưới đây là minh họa về những gì `rabbit instanceof Animal` so sánh với `Animal.prototype`:

![](instanceof.png)

Nhân tiện, cũng có một phương thức [objA.isPrototypeOf(objB)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/object/isPrototypeOf), trả về `true` nếu `objA` nằm ở đâu đó trong chuỗi các nguyên mẫu (chain of prototypes) của `objB`. Vì vậy, bài kiểm tra `obj instanceof Class` có thể được viết lại thành `Class.prototype.isPrototypeOf(obj)`.

Điều đó thật buồn cười, nhưng bản thân `Class` constructor không tham gia kiểm tra! Chỉ có chuỗi các nguyên mẫu (chain of prototypes) và `Class.prototype` tham gia.

Điều đó có thể dẫn đến những hậu quả thú vị khi `prototype` bị thay đổi.

Giống như ở đây:

```js
function Rabbit() {}
let rabbit = new Rabbit();

// changed the prototype
Rabbit.prototype = {};

// ...not a rabbit any more!
alert( rabbit instanceof Rabbit ); // false
```

Đó là một trong những lý do để tránh thay đổi `prototype`. Chỉ để giữ an toàn.

## Bonus: Object toString for the type

Chúng ta đã biết rằng các đối tượng đơn giản được chuyển đổi thành chuỗi dưới dạng `[object Object]`:

```js
let obj = {};

alert(obj); // [object Object]
alert(obj.toString()); // the same
```

Đó là việc chúng thực hiện `toString`. Nhưng có một tính năng ẩn làm cho `toString` thực sự mạnh hơn thế nhiều. Chúng ta có thể sử dụng nó như một extended `typeof` và một thay thế cho `instanceof`.

Nghe lạ nhỉ? Đúng vậy. Hãy làm sáng tỏ.

Theo [đặc tả](https://tc39.github.io/ecma262/#sec-object.prototype.tostring), the built-in `toString` có thể được trích xuất từ đối tượng và được thực thi trong ngữ cảnh của bất kỳ giá trị nào khác. Và kết quả của nó phụ thuộc vào giá trị đó.

- For a number, it will be `[object Number]`
- For a boolean, it will be `[object Boolean]`
- For `null`: `[object Null]`
- For `undefined`: `[object Undefined]`
- For arrays: `[object Array]`
- ...etc (customizable).

Hãy chứng minh:

```js
// copy toString method into a variable for convenience
let objectToString = Object.prototype.toString;

// what type is this?
let arr = [];

alert( objectToString.call(arr) ); // [object Array]
```

Ở đây, chúng ta đã sử dụng [call](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/function/call) như được mô tả trong chương **call-apply-decorators** để thực thi hàm `objectToString` trong ngữ cảnh `this=arr`.

Trong nội bộ, thuật toán `toString` kiểm tra `this` và trả về kết quả tương ứng. Ví dụ khác:

```js
let s = Object.prototype.toString;

alert( s.call(123) ); // [object Number]
alert( s.call(null) ); // [object Null]
alert( s.call(alert) ); // [object Function]
```

### Symbol.toStringTag

Hành vi của Object `toString` có thể được tùy chỉnh bằng cách sử dụng một thuộc tính đối tượng đặc biệt `Symbol.toStringTag`.

Ví dụ:

```js
let user = {
  [Symbol.toStringTag]: "User"
};

alert( {}.toString.call(user) ); // [object User]
```

Đối với hầu hết các đối tượng môi trường cụ thể (environment-specific objects), có một thuộc tính như vậy. Dưới đây là một vài ví dụ cụ thể về trình duyệt:

```js
// toStringTag for the envinronment-specific object and class:
alert( window[Symbol.toStringTag]); // window
alert( XMLHttpRequest.prototype[Symbol.toStringTag] ); // XMLHttpRequest

alert( {}.toString.call(window) ); // [object Window]
alert( {}.toString.call(new XMLHttpRequest()) ); // [object XMLHttpRequest]
```

Như bạn có thể thấy, kết quả chính xác là `Symbol.toStringTag` (nếu tồn tại), được gói vào `[object ...]`.

Cuối cùng, chúng ta có "typeof on steroid" không chỉ hoạt động cho các loại dữ liệu nguyên thủy, mà còn cho các built-in objects và thậm chí có thể được tùy chỉnh.

Nó có thể được sử dụng thay vì `instanceof` cho các built-in objects khi chúng ta muốn lấy kiểu dưới dạng chuỗi thay vì chỉ để kiểm tra.

## Tóm lược

Chúng ta hãy tóm tắt lại các type-checking methods mà chúng ta biết:

|               | works for                                                       |  returns      |
|---------------|-----------------------------------------------------------------|---------------|
| `typeof`      | primitives                                                      | string        |
| `{}.toString` | primitives, built-in objects, objects with `Symbol.toStringTag` | string        |
| `instanceof`  | objects                                                         | true/false    |

Như chúng ta có thể thấy, `{}.toString` về mặt kỹ thuật là một `typeof` "tiên tiến hơn".

Và toán tử `instanceof` thực sự tỏa sáng khi chúng ta đang làm việc với hệ thống phân cấp class và muốn kiểm tra class có tính kế thừa.
