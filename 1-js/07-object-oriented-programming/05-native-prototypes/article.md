# Nguyên mẫu riêng (Native prototypes)

Thuộc tính `"prototype"` được sử dụng rộng rãi bởi chính core của JavaScript. Tất cả các built-in constructor functions sử dụng nó.

Chúng ta sẽ xem nó như thế nào đối với các đối tượng đơn giản trước, và sau đó đối với các đối tượng phức tạp hơn.

## Object.prototype

Giả sử chúng ta xuất ra một đối tượng trống:

```js
let obj = {};
alert( obj ); // "[object Object]" ?
```

Mã tạo ra chuỗi `"[object Object]"` ở đâu? Đó là một built-in `toString` method, nhưng nó ở đâu? The `obj` is empty!

...Nhưng ký hiệu ngắn `obj = {}` giống như `obj = new Object()`, trong đó `Object` -- là một built-in object constructor function. Và hàm đó có `Object.prototype` tham chiếu một đối tượng lớn với `toString` và các hàm khác.

Như thế này (tất cả được built-in):

![](object-prototype.png)

Khi `new Object()` được gọi (hoặc một literal object `{...}` được tạo), `[[Prototype]]` của nó được đặt thành `Object.prototype` theo quy tắc mà chúng ta đã đã thảo luận trong chương trước:

![](object-prototype-1.png)

Sau đó khi `obj.toString()` được gọi -- phương thức được lấy từ `Object.prototype`.

Chúng ta có thể kiểm tra nó như thế này:

```js
let obj = {};

alert(obj.__proto__ === Object.prototype); // true
// obj.toString === obj.__proto__.toString == Object.prototype.toString
```

Xin lưu ý rằng không có thêm `[[Prototype]]` trong chuỗi (chain) ở trên `Object.prototype`:

```js
alert(Object.prototype.__proto__); // null
```

## Other built-in prototypes

Các built-in objects khác như `Array`, `Date`, `Function` và các đối tượng khác cũng giữ các phương thức trong các prototypes.

Chẳng hạn, khi chúng ta tạo một mảng `[1, 2, 3]`, `new Array()` constructor mặc định được sử dụng bên trong. Vì vậy, array data được ghi vào đối tượng mới và `Array.prototype` trở thành prototype của nó và cung cấp các phương thức. Đó là bộ nhớ rất hiệu quả (very memory-efficient).

Theo đặc tả, tất cả các built-in prototypes đều có `Object.prototype` ở trên cùng. Đôi khi mọi người nói rằng "mọi thứ kế thừa từ các đối tượng".

Đây là bức tranh tổng thể (cho 3 built-ins để phù hợp (to fit)):

![](native-prototypes-classes.png)

Hãy kiểm tra các prototypes bằng tay:

```js
let arr = [1, 2, 3];

// it inherits from Array.prototype?
alert( arr.__proto__ === Array.prototype ); // true

// then from Object.prototype?
alert( arr.__proto__.__proto__ === Object.prototype ); // true

// and null on the top.
alert( arr.__proto__.__proto__.__proto__ ); // null
```

Một số phương thức trong nguyên mẫu có thể trùng lặp, ví dụ, `Array.prototype` có `toString` riêng liệt kê các phần tử được phân cách bằng dấu phẩy:

```js
let arr = [1, 2, 3]
alert(arr); // 1,2,3 <-- the result of Array.prototype.toString
```

Như chúng ta đã thấy trước đây, `Object.prototype` cũng có `toString`, nhưng `Array.prototype` gần hơn trong chuỗi (chain), vì vậy biến thể mảng được sử dụng.

![](native-prototypes-array-tostring.png)

Các công cụ trong trình duyệt như Chrome developer console cũng có thể hiển thị tính kế thừa (có thể cần sử dụng `console.dir` cho các built-in objects):

![](console_dir_array.png)

Các built-in objects khác cũng hoạt động theo cách tương tự. Ngay cả các functions. Chúng là các đối tượng của built-in `Function` constructor, và các phương thức của chúng: `call/apply` và các phương thức khác được lấy từ `Function.prototype`. Các hàm cũng có `toString` của riêng chúng.

```js
function f() {}

alert(f.__proto__ == Function.prototype); // true
alert(f.__proto__.__proto__ == Object.prototype); // true, inherit from objects
```

## Primitives

Điều phức tạp nhất xảy ra với strings, numbers và booleans.

Như chúng ta nhớ, chúng không phải là objects. Nhưng nếu chúng ta cố gắng truy cập các thuộc tính của chúng, thì các wrapper objects tạm thời được tạo bằng các built-in constructors `String`, `Number`, `Boolean`, chúng cung cấp các phương thức và biến mất.

Các đối tượng này được tạo ra vô hình cho chúng ta và hầu hết các engines tối ưu hóa chúng, nhưng đặc tả mô tả chính xác theo cách này. Các phương thức của các đối tượng này cũng nằm trong các nguyên mẫu, có sẵn như `String.prototype`, `Number.prototype` và `Boolean.prototype`.

<br>

> ---

**📌 Các giá trị `null` và `undefined` không có các object wrappers**

Các giá trị đặc biệt `null` và `undefined` đứng riêng biệt. Chúng không có object wrappers, vì vậy các phương thức và thuộc tính không có sẵn cho chúng. Và cũng không có nguyên mẫu tương ứng.

> ---

<br>

## Changing native prototypes

Native prototypes có thể được sửa đổi. Chẳng hạn, nếu chúng ta thêm một phương thức vào `String.prototype`, nó sẽ có sẵn cho tất cả các strings:

```js
String.prototype.show = function() {
  alert(this);
};

"BOOM!".show(); // BOOM!
```

Trong quá trình phát triển, chúng ta có thể có ý tưởng về những built-in methods mới mà chúng ta muốn có. Và có thể có một chút cám dỗ để thêm chúng vào các native prototypes. Nhưng đó thường là một ý tưởng tồi.

Prototypes là global, vì vậy thật dễ dàng để có được một cuộc xung đột. Nếu hai thư viện thêm một phương thức `String.prototype.show`, thì một trong số chúng sẽ ghi đè lên một thư viện khác.

Trong lập trình hiện đại, chỉ có một trường hợp khi sửa đổi native prototypes được phê duyệt. Đó là polyfills. Nói cách khác, nếu có một phương thức trong đặc tả JavaScript chưa được hỗ trợ bởi JavaScript engine của chúng ta (hoặc bất kỳ phương thức nào chúng ta muốn hỗ trợ), thì có thể triển khai thủ công và gắn vào built-in prototype với nó.

Ví dụ:

```js
if (!String.prototype.repeat) { // if there's no such method
  // add it to the prototype

  String.prototype.repeat = function(n) {
    // repeat the string n times

    // actually, the code should be more complex than that,
    // throw errors for negative values of "n"
    // the full algorithm is in the specification
    return new Array(n + 1).join(this);
  };
}

alert( "La".repeat(3) ); // LaLaLa
```

## Mượn từ các nguyên mẫu (Borrowing from prototypes)

Trong chương **call-apply-decorators#method-borrowing** chúng ta đã nói về mượn phương thức:

```js
function showArgs() {
  // borrow join from array and call in the context of arguments
  alert( [].join.call(arguments, " - ") );
}

showArgs("John", "Pete", "Alice"); // John - Pete - Alice
```

Vì `join` nằm trong `Array.prototype`, chúng ta có thể gọi nó trực tiếp từ đó và viết lại thành:

```js
function showArgs() {
  alert( Array.prototype.join.call(arguments, " - ") );
}
```

Điều đó hiệu quả hơn, vì nó tránh được việc tạo ra thêm một đối tượng mảng `[]`. Mặt khác, nó dài hơn để viết.

## Tóm lược

- Tất cả các built-in objects theo cùng một mẫu (pattern):
    - Các phương thức được lưu trữ trong prototype (`Array.prototype`, `Object.prototype`, `Date.prototype` v.v.).
    - Bản thân đối tượng chỉ lưu trữ dữ liệu (array items, object properties, the date).
- Primitives cũng lưu trữ các phương thức trong các prototypes của wrapper objects: `Number.prototype`, `String.prototype`, `Boolean.prototype`. Không có wrapper objects cho `undefined` và `null`.
- Các built-in prototypes có thể được sửa đổi hoặc được gắn bằng các phương thức mới. Nhưng không nên thay đổi chúng. Có lẽ lý do được phép duy nhất là khi chúng ta bổ sung một tiêu chuẩn mới, nhưng chưa được hỗ trợ bởi engine JavaScript method.
