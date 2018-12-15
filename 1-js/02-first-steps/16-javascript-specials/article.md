# Những đặc biệt của JavaScript

Chương này tóm tắt ngắn gọn các tính năng của JavaScript mà chúng ta đã học, đặc biệt chú ý đến những khoảnh khắc tinh tế.

## Code structure (Cấu trúc mã)

Các câu được phân định bằng dấu chấm phẩy:

```js
      alert('Hello'); alert('World');
```

Thông thường, ngắt dòng cũng được coi là dấu phân cách, do đó như này cũng sẽ hoạt động:

```js
      alert('Hello')
      alert('World')
```

Đó gọi là "chèn dấu chấm phẩy tự động". Đôi khi, nó không hoạt động, ví dụ:

```js
      alert("There will be an error after this message")

      [1, 2].forEach(alert)
```

Hầu hết các hướng dẫn về lối viết đều đồng ý rằng chúng ta nên đặt dấu chấm phẩy sau mỗi câu.

Dấu chấm phẩy không bắt buộc sau các khối mã `{...}` và các cấu trúc cú pháp với chúng như các vòng lặp:

```js
      function f() {
        // no semicolon needed after function declaration
      }

      for(;;) {
        // no semicolon needed after the loop
      }
```

...Nhưng ngay cả khi chúng ta có thể đặt dấu chấm phẩy "phụ" ở đâu đó, đó không phải là một lỗi. Nó sẽ bị bỏ qua.

Xem thêm tại: **structure**.

## Chế độ nghiêm ngặt (Strict mode)

Để kích hoạt đầy đủ tất cả các tính năng của JavaScript hiện đại, chúng ta nên bắt đầu các tập lệnh với `"use strict"`.

```js
      'use strict';

      ...
```

Lệnh phải ở đầu tập lệnh hoặc ở đầu hàm.

Không có `"use strict"`, mọi thứ vẫn hoạt động, nhưng một số tính năng hoạt động theo kiểu cũ, "compatible" way. Chúng ta thường thích hành vi hiện đại.

Một số tính năng hiện đại của ngôn ngữ (như các class mà chúng ta sẽ học trong tương lai) bật chế độ nghiêm ngặt hoàn toàn.

Xem thêm tại: **strict-mode**.

## Variables (Biến)

Có thể được khai báo bằng cách sử dụng:

- `let`
- `const` (constant, can't be changed)
- `var` (old-style, will see later)

Một tên biến có thể bao gồm:
- Chữ cái và chữ số, nhưng ký tự đầu tiên không thể là một chữ số.
- Các ký tự `$` và `_` là bình thường, ngang bằng với các chữ cái.
- Bảng chữ cái và chữ tượng hình phi Latinh cũng được cho phép, nhưng thường không được sử dụng.

Các biến là kiểu động. Chúng có thể lưu trữ bất kỳ giá trị:

```js
      let x = 5;
      x = "John";
```

Có 7 loại dữ liệu:

- `number` cho cả số nguyên và số thập phân,
- `string` cho chuỗi,
- `boolean` cho các giá trị logic: `true/false`,
- `null` -- một kiểu có một giá trị duy nhất `null`, có nghĩa là "empty" hoặc "does not exist",
- `undefined` -- một kiểu có một giá trị duy nhất `undefined`, có nghĩa là "not assigned",
- `object` và `symbol` - dành cho các cấu trúc dữ liệu phức tạp và các định danh duy nhất, chúng ta chưa học đến chúng.

Toán tử `typeof` trả về kiểu cho một giá trị, với hai ngoại lệ:

```js
      typeof null == "object" // error in the language
      typeof function(){} == "function" // functions are treated specially
```

Xem thêm tại: **variables** và **types**.

## Sự tương tác (Interaction)

Chúng ta đang sử dụng trình duyệt làm môi trường làm việc, vì vậy các chức năng UI cơ bản sẽ là:

**[`prompt(question[, default])`](mdn:api/Window/prompt)**: Hỏi một `câu hỏi` và trả lại những gì khách truy cập đã nhập hoặc `null` nếu họ nhấn "cancel".

**[`confirm(question)`](mdn:api/Window/confirm)**: Hỏi một `câu hỏi` và đề nghị chọn giữa Ok và Cancel. Sự lựa chọn được trả về là `true/false`.

**[`alert(message)`](mdn:api/Window/alert)**: Xuất ra một `message`.

Tất cả các chức năng này là *modal*, chúng tạm dừng thực thi mã và ngăn khách truy cập tương tác với trang cho đến khi họ trả lời.

Ví dụ:

```js
      let userName = prompt("Your name?", "Alice");
      let isTeaWanted = confirm("Do you want some tea?");

      alert( "Visitor: " + userName ); // Alice
      alert( "Tea wanted: " + isTeaWanted ); // true
```

Xem thêm tại: **alert-prompt-confirm**.

## Operators (Toán tử)

JavaScript hỗ trợ các toán tử sau:

**Số học (Arithmetical)**

Thường là: `* + - /`, cũng là `%` cho phần còn lại và `**` cho sức mạnh của một số.

Chuỗi nhị phân cộng (binary plus) `+` nối chuỗi. Và nếu bất kỳ toán hạng nào là một chuỗi, thì một toán hạng khác cũng được chuyển đổi thành chuỗi:

```js
      alert( '1' + 2 ); // '12', string
      alert( 1 + '2' ); // '12', string
```

**Phép gán**: Có một phép gán đơn giản `a = b` và các phép gán kết hợp như `a *= 2`.

**Bitwise**: Toán tử bitwise hoạt động với các số nguyên ở cấp độ bit, xem [docs](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Bitwise_Operators) để biết khi nào thì cần.

**Ternary**: Toán tử duy nhất có ba tham số: `cond ? resultA : resultB`. Nếu `cond` là đúng, trả về `resultA`, nếu không thì `resultB`.

**Toán tử logic (Logical operators)**: Logic AND `&&` và OR `||` thực hiện đánh giá ngắn mạch và sau đó trả về giá trị khi nó dừng lại.

**So sánh (Comparisons)**: Kiểm tra đẳng thức `==` cho các giá trị của các kiểu khác nhau chuyển đổi chúng thành một số (ngoại trừ `null` và `undefined` bằng nhau và không có gì khác), vì vậy các giá trị này bằng nhau:

```js
      alert( 0 == false ); // true
      alert( 0 == '' ); // true
```

Các so sánh khác cũng sẽ chuyển đổi thành một số như vậy.

Toán tử đẳng thức nghiêm ngặt `===` không thực hiện chuyển đổi: các kiểu khác nhau luôn có nghĩa là các giá trị khác nhau, vì vậy:

Các giá trị `null` và `undefined` là đặc biệt: chúng bằng `==` lẫn nhau và không bằng bất cứ thứ gì khác.

So sánh lớn hơn/ít hơn (Greater/less) so sánh các chuỗi ký tự theo từng ký tự, các loại khác được chuyển đổi thành một số.

Toán tử logic: Có một vài toán tử khác, như toán tử dấu phẩy.

Xem thêm tại: **operators**, **comparison**, **logical-operators**.

## Vòng lặp (Loops)

Chúng ta có bao gồm 3 loại loop:

```js
      // 1
      while (condition) {
        ...
      }

      // 2
      do {
        ...
      } while (condition);

      // 3
      for(let i = 0; i < 10; i++) {
        ...
      }
```

- Biến được khai báo trong vòng lặp `for(let...)` chỉ hiển thị bên trong vòng lặp. Nhưng chúng ta cũng có thể bỏ qua `let` và sử dụng lại một biến hiện có.

- Chỉ thị `break/continue` cho phép thoát toàn bộ vòng lặp hiện tại. Sử dụng labels để break các vòng lặp lồng nhau.

Chi tiết tại: **while-for**.

Sau này chúng ta sẽ nghiên cứu thêm các loại vòng lặp để đối phó với các đối tượng.

## Cấu trúc "switch"

Cấu trúc "switch" có thể thay thế nhiều kiểm tra `if`. Nó sử dụng `===` (đẳng thức nghiêm ngặt) để so sánh.

Ví dụ:

```js
      let age = prompt('Your age?', 18);

      switch (age) {
        case 18:
          alert("Won't work"); // the result of prompt is a string, not a number

        case "18":
          alert("This works!");
          break;

        default:
          alert("Any value not equal to one above");
      }
```

Chi tiết tại: **switch**.

## Functions

Chúng ta đã trình bày ba cách để tạo một hàm trong JavaScript:

1. Khai báo hàm (Function Declaration): hàm trong luồng mã chính

    ```js
    function sum(a, b) {
      let result = a + b;

      return result;
    }
    ```

2. Biểu thức hàm (Function Expression): hàm trong ngữ cảnh của biểu thức

    ```js
    let sum = function(a, b) {
      let result = a + b;

      return result;
    }
    ```

    Các function expression có thể có một tên, như `sum = function name(a, b)`, nhưng `name` chỉ hiển thị bên trong hàm đó.

3. Hàm mũi tên (Arrow functions)

    ```js
    // expression at the right side
    let sum = (a, b) => a + b;

    // or multi-line syntax with { ... }, need return here:
    let sum = (a, b) => {
      // ...
      return a + b;
    }

    // without arguments
    let sayHi = () => alert("Hello");

    // with a single argument
    let double = n => n * 2;
    ```

- Hàm có thể có các biến cục bộ: những biến được khai báo bên trong body của nó. Các biến như vậy chỉ được nhìn thấy bên trong hàm.
- Tham số có thể có các giá trị mặc định: `function sum(a = 1, b = 2) {...}`.
- Các hàm luôn trả về một cái gì đó. Nếu không có câu lệnh `return`, thì kết quả là `undefined`.

| Function Declaration            | Function Expression                               |
|---------------------------------|---------------------------------------------------|
| hiển thị trong toàn bộ khối mã  | được tạo khi thực thi gặp đến nó                  |
| -                               | có thể có tên, chỉ hiển thị bên trong hàm         |

Xem thêm tại: **function-basics**, **function-expressions-arrows**.

## More to come

Đó là một danh sách ngắn gọn các tính năng của JavaScript. Cho đến bây giờ chúng ta mới chỉ nghiên cứu cơ bản. Còn nữa trong hướng dẫn, bạn sẽ tìm thấy nhiều tính năng đặc biệt và nâng cao hơn của JavaScript.
