# Functions

Thông thường chúng ta hay cần thực hiện một hành động tương tự ở nhiều nơi của kịch bản.

Ví dụ: chúng ta cần hiển thị một thông báo đẹp mắt khi khách truy cập đăng nhập, đăng xuất và có thể ở một nơi khác.

Functions là "khối xây dựng (building blocks)" chính của chương trình. Chúng cho phép mã được gọi nhiều lần mà không lặp lại.

Chúng ta đã thấy các ví dụ về các hàm dựng sẵn (built-in functions), như `alert(message)`, `prompt(message, default)` và `confirm(question)`. Nhưng chúng ta cũng có thể tạo ra các function của riêng mình.

## Khai báo hàm (Function Declaration)

Để tạo một hàm chúng ta có thể sử dụng một *khai báo hàm (function declaration)*.

Nó trông như thế này:

```js
      function showMessage() {
        alert( 'Hello everyone!' );
      }
```

Từ khóa `function` đi trước, sau đó đến *tên của hàm*, sau đó là danh sách *tham số* giữa dấu ngoặc đơn (trống trong ví dụ trên) và cuối cùng là mã của hàm, cũng được đặt tên là "thân hàm (the function body)" , đặt giữa dấu ngoặc nhọn.

![](function_basics.png)

Hàm mới của chúng ta có thể được gọi bằng tên của nó: `showMessage()`.

Ví dụ:

```js
      function showMessage() {
        alert( 'Hello everyone!' );
      }

      showMessage();
      showMessage();
```

Cuộc gọi `showMessage()` thực thi mã của hàm. Ở đây chúng ta sẽ thấy message hai lần.

Ví dụ này thể hiện rõ một trong những mục đích chính của các hàm: để tránh trùng lặp mã.

Nếu chúng ta cần thay đổi message hoặc cách nó được hiển thị, thì chỉ cần sửa đổi mã ở một nơi: function xuất ra nó.

## Local variables

Một biến được khai báo bên trong một hàm chỉ hiển thị bên trong hàm đó.

Ví dụ:

```js
      function showMessage() {
        let message = "Hello, I'm JavaScript!"; // local variable

        alert( message );
      }

      showMessage(); // Hello, I'm JavaScript!

      alert( message ); // <-- Error! The variable is local to the function
```

## Outer variables

Một hàm cũng có thể truy cập một biến bên ngoài, ví dụ:

```js
      let userName = 'John';

      function showMessage() {
        let message = 'Hello, ' + userName;
        alert(message);
      }

      showMessage(); // Hello, John
```

Hàm có toàn quyền truy cập vào biến bên ngoài (outer variable). Nó cũng có thể sửa đổi nó.

Ví dụ:

```js
      let userName = 'John';

      function showMessage() {
        userName = "Bob"; // (1) changed the outer variable

        let message = 'Hello, ' + userName;
        alert(message);
      }

      alert( userName ); // John before the function call

      showMessage();

      alert( userName ); // Bob, the value was modified by the function
```

Biến bên ngoài (outer variable) chỉ được sử dụng nếu không có biến cục bộ. Vì vậy, một sự sửa đổi không thường xuyên có thể xảy ra nếu chúng ta quên `let`.

Nếu một biến cùng tên được khai báo bên trong hàm thì nó *đổ bóng (đè)* biến bên ngoài. Chẳng hạn, trong đoạn mã bên dưới, hàm sử dụng `userName` cục bộ. Cái bên ngoài bị bỏ qua:

```js
      let userName = 'John';

      function showMessage() {
        let userName = "Bob"; // declare a local variable

        let message = 'Hello, ' + userName; // Bob
        alert(message);
      }

      // the function will create and use its own userName
      showMessage();

      alert( userName ); // John, unchanged, the function did not access the outer variable
```

<br>

> ---

**📌 Biến toàn cầu (Global variables)**

Các biến được khai báo bên ngoài bất kỳ hàm nào, chẳng hạn như `userName` bên ngoài trong đoạn mã trên, được gọi là *global*.

Các biến toàn cầu (Global variable) có thể nhìn thấy từ bất kỳ function nào (trừ khi bị che khuất bởi locals).

Thông thường, một hàm khai báo tất cả các biến cụ thể cho nhiệm vụ của nó. Các global variable chỉ lưu trữ dữ liệu cấp dự án, do đó, điều quan trọng là các biến này có thể truy cập được từ mọi nơi. Mã hiện đại có ít hoặc không có globals. Hầu hết các biến nằm trong chức năng của họ.

> ---

<br>

## Các tham số (Parameters)

Chúng ta có thể truyền dữ liệu tùy ý cho các hàm bằng các tham số (còn được gọi là *đối số hàm (function arguments)*).

Trong ví dụ dưới đây, hàm có hai tham số: `from` và `text`.

```js
      function showMessage(from, text) { // arguments: from, text
        alert(from + ': ' + text);
      }

      showMessage('Ann', 'Hello!'); // Ann: Hello! (*)
      showMessage('Ann', "What's up?"); // Ann: What's up? (**)
```

Khi hàm được gọi trong các dòng `(*)` và `(**)`, các giá trị đã cho sẽ được sao chép vào các biến cục bộ `from` và` text`. Sau đó, function sử dụng chúng.

Dưới đây là một ví dụ nữa: chúng ta có một biến `from` và truyền nó cho hàm. Xin lưu ý: hàm thay đổi `from`, nhưng sự thay đổi không được nhìn thấy từ bên ngoài, bởi vì một hàm luôn nhận được một copy của value:


```js
      function showMessage(from, text) {

        from = '*' + from + '*'; // make "from" look nicer

        alert( from + ': ' + text );
      }

      let from = "Ann";

      showMessage(from, "Hello"); // *Ann*: Hello

      // the value of "from" is the same, the function modified a local copy
      alert( from ); // Ann
```

## Giá trị mặc định (Default values)

Nếu một tham số không được cung cấp, thì giá trị của nó sẽ trở thành `undefined`.

Chẳng hạn, hàm đã nói ở trên `showMessage(from, text)` có thể được gọi với một đối số duy nhất:

```js
      showMessage("Ann");
```

Đó không phải là một lỗi. Một cuộc gọi như vậy sẽ tạo ra `"Ann: undefined"`. Không có `text`, vì vậy nó giả định rằng `text === undefined`.

Nếu chúng ta muốn sử dụng một "default" `text` trong trường hợp này, thì chúng ta có thể chỉ định nó sau `=`:

```js
      function showMessage(from, text = "no text given") {
        alert( from + ": " + text );
      }

      showMessage("Ann"); // Ann: no text given
```

Bây giờ nếu tham số `text` không được thông qua, nó sẽ nhận được giá trị `"no text given"`

Ở đây `"no text given"` là một chuỗi, nhưng nó có thể là một biểu thức phức tạp hơn, nó chỉ được ước tính và gán nếu tham số bị thiếu. Vì vậy, điều này cũng có thể:

```js
      function showMessage(from, text = anotherFunction()) {
        // anotherFunction() only executed if no text given
        // its result becomes the value of text
      }
```

<br>

> ---

**📌 Đánh giá các tham số mặc định (Evaluation of default parameters)**

Trong JavaScript, một tham số mặc định được đánh giá mỗi khi hàm được gọi mà không có tham số tương ứng. Trong ví dụ trên, `anotherFunctions()` được gọi mọi lúc `someMessage()` được gọi mà không có tham số `text`. Điều này trái ngược với một số ngôn ngữ khác như Python, trong đó mọi tham số mặc định chỉ được đánh giá một lần trong quá trình diễn giải ban đầu.

> ---

<br>
<br>

> ---

**📌 Thông số mặc định kiểu cũ (Default parameters old-style)**

Các phiên bản cũ của JavaScript không hỗ trợ các tham số mặc định. Vì vậy, có những cách khác để hỗ trợ chúng, mà bạn có thể tìm thấy chủ yếu trong các tập lệnh cũ.

Chẳng hạn, một kiểm tra rõ ràng về việc `undefined`:

```js
      function showMessage(from, text) {
        if (text === undefined) {
          text = 'no text given';
        }

        alert( from + ": " + text );
      }
```

...Hoặc toán tử `||`:

```js
      function showMessage(from, text) {
        // if text is falsy then text gets the "default" value
        text = text || 'no text given';
        ...
      }
```

> ---

<br>

## Trả lại một giá trị (Returning a value)

Một hàm có thể trả một giá trị về cho mã gọi (calling code) như là kết quả.

Ví dụ đơn giản nhất sẽ là một hàm tính tổng hai giá trị:

```js
      function sum(a, b) {
        return a + b;
      }

      let result = sum(1, 2);
      alert( result ); // 3
```

Lệnh `return` có thể ở bất kỳ vị trí nào của hàm. Khi thực thi bắt gặp nó, hàm dừng lại và giá trị được trả về mã gọi (calling code) (được gán cho `result` ở trên).

Có thể có nhiều lần xuất hiện của `return` trong một hàm duy nhất. Ví dụ:

```js
      function checkAge(age) {
        if (age > 18) {
          return true;
        } else {
          return confirm('Do you have permission from your parents?');
        }
      }

      let age = prompt('How old are you?', 18);

      if ( checkAge(age) ) {
        alert( 'Access granted' );
      } else {
        alert( 'Access denied' );
      }
```

Có thể sử dụng `return` mà không có giá trị. Điều đó khiến function thoát ra ngay lập tức.

Ví dụ:

```js
      function showMovie(age) {
        if ( !checkAge(age) ) {
          return;
        }

        alert( "Showing you the movie" ); // (*)
        // ...
      }
```

Trong đoạn mã trên, nếu `checkAge(age)` trả về `false`, thì `showMovie` sẽ không tiếp tục với `alert`.

<br>

> ---

**📌 Một hàm có `return` trống hoặc không có nó trả về `undefined`***

Nếu một hàm không trả về một giá trị, thì nó cũng giống như khi nó trả về `undefined`:

```js
      function doNothing() { /* empty */ }

      alert( doNothing() === undefined ); // true
```

Một `return` trống cũng giống như `return undefined`:

```js
      function doNothing() {
        return;
      }

      alert( doNothing() === undefined ); // true
```

> ---

<br>
<br>

> ---

**📌 Không bao giờ thêm một dòng mới giữa `return` và giá trị**

Đối với một biểu thức dài trong `return`, có thể sẽ rất hấp dẫn khi đặt nó trên một dòng riêng biệt, như thế này:

```js
      return
       (some + long + expression + or + whatever * f(a) + f(b))
```
Điều đó không hiệu quả, bởi vì JavaScript giả định sẽ có một dấu chấm phẩy sau khi `return`. Điều đó sẽ làm việc giống như:

```js
      return;
       (some + long + expression + or + whatever * f(a) + f(b))
```

Vì vậy, nó thực sự trở thành một sự empty return. Thay vào đó, chúng ta nên đặt giá trị trên cùng một dòng.

> ---

<br>

## Đặt tên hàm

Cac function là hành động. Vì vậy, tên của chúng thường là một động từ. Nó phải ngắn gọn, chính xác nhất có thể và mô tả function làm gì, để ai đó đọc mã nhận được chỉ dẫn về function đó làm gì.

Đó là một thực tế phổ biến để bắt đầu một function với tiền tố bằng lời nói mô tả mơ hồ hành động. Phải có một thỏa thuận trong nhóm về ý nghĩa của các tiền tố.

Chẳng hạn, các hàm bắt đầu bằng `"show"` thường hiển thị một cái gì đó.

Chức năng bắt đầu bằng ...

- `"get…"` -- return a value,
- `"calc…"` -- calculate something,
- `"create…"` -- create something,
- `"check…"` -- check something and return a boolean, etc.

Ví dụ về các tên như vậy:

```js
      showMessage(..)     // shows a message
      getAge(..)          // returns the age (gets it somehow)
      calcSum(..)         // calculates a sum and returns the result
      createForm(..)      // creates a form (and usually returns it)
      checkPermission(..) // checks a permission, returns true/false
```

Với các tiền tố được đặt đúng chỗ, việc lướt qua một tên hàm cho biết về loại công việc và loại giá trị mà nó trả về.

<br>

> ---

**📌 Một chức năng - một hành động**

Một hàm nên làm chính xác những gì được đề xuất bởi tên của nó, không hơn.

Hai hành động độc lập thường xứng đáng với hai function, ngay cả khi chúng thường được gọi cùng nhau (trong trường hợp đó chúng ta có thể thực hiện chức năng thứ 3 gọi hai function đó).

Một vài ví dụ về việc phá vỡ quy tắc này:

- `getAge` -- sẽ rất tệ nếu nó hiển thị một `alert` với age (nó chỉ nên get).
- `createForm` -- sẽ rất tệ nếu nó sửa đổi tài liệu, thêm một biểu mẫu vào nó (nó chỉ nên tạo và trả về).
- `checkPermission` -- sẽ rất tệ nếu hiển thị `access granted/denied` message (chỉ nên thực hiện kiểm tra và trả về kết quả).

Những ví dụ này giả định ý nghĩa phổ biến của tiền tố. Ý nghĩa của chúng đối với bạn được quyết định bởi bạn và nhóm của bạn. Có lẽ nó khá bình thường khi mã của bạn hoạt động khác đi. Nhưng bạn nên có một sự hiểu biết vững chắc về ý nghĩa của tiền tố, chức năng tiền tố có thể và không thể làm gì. Tất cả các hàm có cùng tiền tố phải tuân theo các quy tắc. Và nhóm nên chia sẻ kiến thức.

> ---

<br>
<br>

> ---

**📌 Ultrashort function names***

Các hàm được sử dụng *rất thường xuyên* đôi khi có tên ultrashort.

Ví dụ: khung [jQuery](http://jquery.com) định nghĩa một hàm với `$`. Thư viện [LoDash](http://lodash.com/) có core function có tên là `_`.

Đây là những ngoại lệ. Tên hàm thường phải ngắn gọn và mô tả.

> ---

<br>

## Functions == Comments

Các function nên ngắn và làm chính xác một điều. Nếu điều đó là lớn, có lẽ nó đáng để chia function thành một vài function nhỏ hơn. Đôi khi tuân theo quy tắc này có thể không dễ dàng, nhưng đó chắc chắn là một điều tốt.

Một function riêng biệt không chỉ dễ dàng hơn để kiểm tra và gỡ lỗi -- chính sự tồn tại của nó là một comment tuyệt vời!

Chẳng hạn, so sánh hai hàm `showPrimes(n)` bên dưới. Mỗi cái xuất ra [số nguyên tố](https://en.wikipedia.org/wiki/Prime_number) cho đến `n`.

Biến thể đầu tiên sử dụng nhãn:

```js
function showPrimes(n) {
  nextPrime: for (let i = 2; i < n; i++) {

    for (let j = 2; j < i; j++) {
      if (i % j == 0) continue nextPrime;
    }

    alert( i ); // a prime
  }
}
```

Biến thể thứ hai sử dụng một hàm bổ sung `isPrime(n)` để kiểm tra tính nguyên thủy:

```js
function showPrimes(n) {

  for (let i = 2; i < n; i++) {
    if (!isPrime(i)) continue;

    alert(i);  // a prime
  }
}

function isPrime(n) {
  for (let i = 2; i < n; i++) {
    if ( n % i == 0) return false;
  }
  return true;
}
```

Biến thể thứ hai dễ hiểu hơn phải không? Thay vì đoạn mã, chúng ta thấy một tên của hành động (`isPrime`). Đôi khi mọi người đề cập đến mã như *tự mô tả (self-describing)*.

Vì vậy, các function có thể được tạo ngay cả khi chúng ta không có ý định sử dụng lại chúng. Chúng cấu trúc mã và làm cho nó dễ đọc.

## Tóm lược

Một khai báo hàm trông như thế này:

```js
function name(parameters, delimited, by, comma) {
  /* code */
}
```

- Các giá trị được truyền cho một hàm như các tham số được sao chép vào các biến cục bộ của nó.
- Một function có thể truy cập các biến bên ngoài. Nhưng nó chỉ hoạt động từ trong ra ngoài. Mã bên ngoài hàm không thấy các biến cục bộ của nó.
- Một hàm có thể trả về một giá trị. Nếu không, thì kết quả của nó là `undefined`.

Để làm cho mã sạch và dễ hiểu, nên sử dụng chủ yếu các biến và tham số cục bộ trong hàm, không nên sử dụng các biến bên ngoài (outer variables).

Luôn luôn dễ dàng hơn để hiểu một hàm lấy tham số, làm việc với chúng và trả về kết quả so với hàm không có tham số, nhưng sửa đổi các biến bên ngoài như là hiệu ứng phụ.

Function naming:

- Một tên nên mô tả rõ ràng những gì function làm. Khi chúng ta thấy một lệnh gọi hàm trong mã, một cái tên hay ngay lập tức cho chúng ta hiểu nó làm gì và trả về gì.
- Hàm là một hành động, vì vậy tên hàm thường bằng lời nói.
- Tồn tại nhiều tiền tố chức năng nổi tiếng như `create…`, `show…`, `get…`, `check…`, v.v. Sử dụng chúng để gợi ý những gì một function làm.

Các function là các khối xây dựng chính (main building blocks) của các kịch bản. Bây giờ chúng ta đã bao quát xong những điều cơ bản, vì vậy chúng ta thực sự có thể bắt đầu tạo và sử dụng chúng. Nhưng đó chỉ là sự khởi đầu của lộ trình. Chúng ta sẽ trở lại với chúng nhiều lần, đi sâu hơn vào các tính năng nâng cao của chúng.
