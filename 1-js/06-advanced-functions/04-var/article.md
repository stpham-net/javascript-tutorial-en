
# The old "var"

Trong chương về **variables**, chúng ta đã đề cập đến ba cách khai báo biến:

1. `let`
2. `const`
3. `var`

`let` và `const` hành xử giống hệt nhau về mặt Lexical Environments.

Nhưng `var` là một con quái vật rất khác, bắt nguồn từ thời rất cũ. Nó thường không được sử dụng trong các kịch bản hiện đại, nhưng vẫn ẩn trong các kịch bản cũ.

Nếu bạn không có kế hoạch gặp gỡ các kịch bản như vậy, bạn thậm chí có thể bỏ qua chương này hoặc hoãn lại, nhưng sau đó có khả năng nó sẽ cắn bạn sau đó.

Từ cái nhìn đầu tiên, `var` hành xử tương tự như `let`. Đó là, khai báo một biến:

```js
function sayHi() {
  var phrase = "Hello"; // local variable, "var" instead of "let"

  alert(phrase); // Hello
}

sayHi();

alert(phrase); // Error, phrase is not defined
```

...Nhưng đây là sự khác biệt.

## "var" không có block scope

Các biến `var` là function-wide hoặc global, chúng được hiển thị thông qua các blocks.

Ví dụ:

```js
if (true) {
  var test = true; // use "var" instead of "let"
}

alert(test); // true, the variable lives after if
```

Nếu chúng ta đã sử dụng `let test` trên dòng thứ 2, thì nó sẽ không hiển thị với `alert`. Nhưng `var` bỏ qua các khối mã, vì vậy chúng ta đã có một `test` global.

Điều tương tự cho các vòng lặp: `var` không thể là block-local hoặc loop-local:

```js
for (var i = 0; i < 10; i++) {
  // ...
}

alert(i); // 10, "i" is visible after loop, it's a global variable
```

Nếu một code block nằm trong một function, thì `var` trở thành biến cấp độ hàm:

```js
function sayHi() {
  if (true) {
    var phrase = "Hello";
  }

  alert(phrase); // works
}

sayHi();
alert(phrase); // Error: phrase is not defined
```

Như chúng ta có thể thấy, `var` xuyên qua `if`, `for` hoặc các khối mã khác. Đó là bởi vì một thời gian dài trước đây trong các khối JavaScript không có Lexical Environments. Và `var` là một kỷ niệm về điều đó.

## "var" được xử lý khi function bắt đầu

Khai báo `var` được xử lý khi function starts (hoặc script bắt đầu cho globals).

Nói cách khác, các biến `var` được định nghĩa từ đầu hàm, bất kể định nghĩa ở đâu (giả sử rằng định nghĩa không nằm trong hàm lồng nhau).

Mã này:

```js
function sayHi() {
  phrase = "Hello";

  alert(phrase);

  var phrase;
}
```

...Về mặt kỹ thuật có giống như thế này không (moved `var phrase` above):

```js
function sayHi() {
  var phrase;

  phrase = "Hello";

  alert(phrase);
}
```

...Hoặc thậm chí như thế này (hãy nhớ, các khối mã bị bỏ qua):

```js
function sayHi() {
  phrase = "Hello"; // (*)

  if (false) {
    var phrase;
  }

  alert(phrase);
}
```

Mọi người cũng gọi hành vi đó là "cẩu (hoisting)" (sự nâng lên (raising)), bởi vì tất cả các `var` đều được "nâng lên" trên cùng của hàm.

Vì vậy, trong ví dụ trên, nhánh `if (false)` không bao giờ thực thi, nhưng điều đó không thành vấn đề. `var` bên trong nó được xử lý ở đầu hàm, vì vậy tại thời điểm `(*)` biến tồn tại.

**Khai báo (declarations) được nâng lên, nhưng phép gán (assignments) thì không.**

Điều đó tốt hơn để chứng minh bằng một ví dụ, như thế này:

```js
function sayHi() {
  alert(phrase);  

  var phrase = "Hello";
}

sayHi();
```

Dòng `var phrase = "Hello"` có hai hành động trong đó:

1. Variable declaration `var`
2. Variable assignment `=`.

Khai báo được xử lý khi bắt đầu thực thi function ("hoisted"), nhưng phép gán luôn hoạt động tại nơi nó xuất hiện. Vì vậy, mã hoạt động về cơ bản như thế này:

```js
function sayHi() {
  var phrase; // declaration works at the start...

  alert(phrase); // undefined

  phrase = "Hello"; // ...assignment - when the execution reaches it.
}

sayHi();
```

Bởi vì tất cả các khai báo `var` được xử lý tại function start, chúng ta có thể tham chiếu chúng ở bất kỳ đâu. Nhưng các biến undefined cho đến khi được gán.

Trong cả hai ví dụ trên `alert` chạy không có lỗi, vì biến `phrase` tồn tại. Nhưng giá trị của nó chưa được gán, vì vậy nó hiển thị `undefined`.

## Tóm lược

Có hai điểm khác biệt chính của `var`:

1. Các biến không có phạm vi khối, chúng có thể nhìn thấy tối thiểu ở cấp hàm.
2. Khai báo biến được xử lý khi bắt đầu function.

Có thêm một sự khác biệt nhỏ liên quan đến global object, chúng ta sẽ đề cập đến điều đó trong chương tiếp theo.

Những sự khác biệt này thực sự là một điều xấu hầu hết thời gian. Đầu tiên, chúng ta không thể tạo các block-local variables. Và hoisting chỉ tạo thêm không gian cho lỗi. Vì vậy, đối với các scripts mới, đặc biệt hiếm khi `var` được sử dụng.
