
# The "new Function" syntax

Có thêm một cách để tạo function. Nó hiếm khi được sử dụng, nhưng đôi khi không có sự thay thế.

## Syntax

Cú pháp tạo hàm:

```js
let func = new Function ([arg1[, arg2[, ...argN]],] functionBody)
```

Nói cách khác, các tham số function (hay chính xác hơn là tên của chúng) đi trước và phần thân là cuối cùng. Tất cả các đối số là chuỗi.

Nó dễ hiểu hơn bằng cách nhìn vào một ví dụ. Đây là một hàm có hai đối số:

```js
let sum = new Function('a', 'b', 'return a + b'); 

alert( sum(1, 2) ); // 3
```

Nếu không có đối số, thì chỉ có một đối số duy nhất, thân hàm:

```js
let sayHi = new Function('alert("Hello")');

sayHi(); // Hello
```

Sự khác biệt chính so với các cách khác mà chúng ta đã thấy là hàm được tạo theo nghĩa đen (literally) từ một chuỗi, được truyền (passed) vào run time. 

Tất cả các khai báo trước đó yêu cầu chúng ta, các lập trình viên, viết function code trong script.

Nhưng `new Function` cho phép biến bất kỳ chuỗi nào thành hàm. Ví dụ: chúng ta có thể nhận một function mới từ một máy chủ và sau đó thực thi nó:

```js
let str = ... receive the code from a server dynamically ...

let func = new Function(str);
func();
```

Nó được sử dụng trong các trường hợp rất cụ thể, như khi chúng ta nhận được mã từ máy chủ hoặc để tự động biên dịch (dynamically compile) một hàm từ một template. Nhu cầu đó thường phát sinh ở các giai đoạn phát triển tiên tiến.

## Closure

Thông thường, một hàm ghi nhớ nơi nó được sinh ra trong thuộc tính đặc biệt `[[Environment]]`. Nó tham chiếu Lexical Environment từ nơi nó được tạo ra.

Nhưng khi một hàm được tạo bằng cách sử dụng `new Function`, thì các `[[Environment]]` của nó không phải là Lexical Environment hiện tại, mà thay vào đó là global.

```js

function getFunc() {
  let value = "test";

  let func = new Function('alert(value)');

  return func;
}

getFunc()(); // error: value is not defined
```

So sánh nó với hành vi thông thường:

```js
function getFunc() {
  let value = "test";

  let func = function() { alert(value); };

  return func;
}

getFunc()(); // "test", from the Lexical Environment of getFunc
```

Tính năng đặc biệt này của `new Function` trông lạ, nhưng có vẻ rất hữu ích trong thực tế.

Hãy tưởng tượng rằng chúng ta phải tạo một hàm từ một chuỗi. Mã của hàm đó không được biết tại thời điểm viết script (đó là lý do tại sao chúng ta không sử dụng các hàm thông thường), nhưng sẽ được biết đến trong quá trình thực thi. Chúng ta có thể nhận nó từ máy chủ hoặc từ một nguồn khác.

Function mới của chúng ta cần tương tác với main script.

Có lẽ chúng ta muốn nó có thể truy cập các outer local variables?

Vấn đề là trước khi JavaScript được xuất bản để sản xuất, nó đã được nén bằng cách sử dụng một *minifier* -- một chương trình đặc biệt thu nhỏ mã bằng cách xóa extra comments, spaces và -- điều quan trọng, đổi tên local variables thành biến ngắn hơn.

Chẳng hạn, nếu một hàm có `let userName`, minifier sẽ thay thế nó thành `let a` (hoặc một chữ cái khác nếu chữ này đã được dùng) và thực hiện nó ở mọi nơi. Đó thường là một điều an toàn để làm, bởi vì biến là local, không có gì outside the function có thể truy cập nó. Và bên trong function, minifier thay thế mỗi đề cập đến nó. Minifiers rất thông minh, chúng phân tích cấu trúc mã, vì vậy chúng không phá vỡ bất cứ thứ gì. Chúng không chỉ là một công cụ tìm và thay thế ngu ngốc.

Nhưng, nếu `new Function` có thể truy cập các outer variables, thì nó sẽ không thể tìm thấy `userName`, vì điều này được truyền vào dưới dạng một chuỗi *sau khi* mã được rút gọn.

**Ngay cả khi chúng ta có thể truy cập outer lexical environment trong `new Function`, chúng ta sẽ gặp vấn đề với các minifiers.**

"Tính năng đặc biệt" của `new Function` giúp chúng ta tránh khỏi những sai lầm.

Và nó thực thi mã tốt hơn. Nếu chúng ta cần truyền một cái gì đó cho một hàm được tạo bởi `new Function`, chúng ta nên chuyển nó một cách rõ ràng như là một đối số.

Hàm "sum" của chúng ta thực sự làm đúng:

```js
let sum = new Function('a', 'b', 'return a + b');

let a = 1, b = 2;

// outer values are passed as arguments
alert( sum(a, b) ); // 3
```

## Tóm lược

Cú pháp:

```js
let func = new Function(arg1, arg2, ..., body);
```

Vì lý do lịch sử, các đối số cũng có thể được đưa dưới dạng danh sách được phân tách bằng dấu phẩy. 

Ba nghĩa này giống nhau:

```js 
new Function('a', 'b', 'return a + b'); // basic syntax
new Function('a,b', 'return a + b'); // comma-separated
new Function('a , b', 'return a + b'); // comma-separated with spaces
```

Các hàm được tạo bằng `new Function`, có `[[Environment]]` tham chiếu global Lexical Environment, không phải outer Lexical Environment. Do đó, chúng không thể sử dụng các outer variables. Nhưng điều đó thực sự tốt, bởi vì nó cứu chúng ta khỏi lỗi. Truyền các tham số rõ ràng là một phương pháp tốt hơn về mặt kiến trúc và không gây ra vấn đề gì với các minifiers.
