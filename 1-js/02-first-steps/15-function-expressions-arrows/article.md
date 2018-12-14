# Biểu thức hàm và mũi tên (Function expressions and arrows)

Trong JavaScript, một hàm không phải là "cấu trúc ngôn ngữ ma thuật (magical language structure)", mà là một loại giá trị đặc biệt.

Cú pháp mà chúng ta đã sử dụng trước đây được gọi là *Khai báo hàm (Function Declaration)*:

```js
      function sayHi() {
        alert( "Hello" );
      }
```

Có một cú pháp khác để tạo một hàm được gọi là *Biểu thức hàm (Function Expression)*.

Nó trông như thế này:

```js
      let sayHi = function() {
        alert( "Hello" );
      };
```

Ở đây, hàm được tạo và gán cho biến rõ ràng, giống như bất kỳ giá trị nào khác. Bất kể hàm được định nghĩa như thế nào, nó chỉ là một giá trị được lưu trữ trong biến `sayHi`.

Ý nghĩa của các mẫu mã này là như nhau: "tạo một hàm và đặt nó vào biến `sayHi`".

Chúng ta thậm chí có thể in ra giá trị đó bằng cách sử dụng `alert`:

```js
      function sayHi() {
        alert( "Hello" );
      }

      alert( sayHi ); // shows the function code
```

Xin lưu ý rằng dòng cuối cùng không chạy hàm, vì không có dấu ngoặc đơn sau `sayHi`. Có những ngôn ngữ lập trình trong đó bất kỳ khi nào đề cập đến tên hàm gây ra sự thực thi của nó, nhưng JavaScript không giống như vậy.

Trong JavaScript, một hàm là một giá trị, vì vậy chúng ta có thể coi nó là một giá trị. Đoạn mã trên cho thấy biểu diễn chuỗi của nó, đó là mã nguồn.

Tất nhiên, đó là một giá trị đặc biệt, theo nghĩa mà chúng ta có thể gọi nó như `sayHi()`.

Nhưng nó vẫn là một giá trị. Vì vậy, chúng ta có thể làm việc với nó như với các loại giá trị khác.

Chúng ta có thể sao chép một hàm sang một biến khác:

```js
      function sayHi() {   // (1) create
        alert( "Hello" );
      }

      let func = sayHi;    // (2) copy

      func(); // Hello     // (3) run the copy (it works)!
      sayHi(); // Hello    //     this still works too (why wouldn't it)
```

Đây là những gì xảy ra ở trên một cách chi tiết:

1. Khai báo hàm `(1)` tạo hàm và đặt nó vào biến có tên `sayHi`.
2. Dòng `(2)` sao chép nó vào biến `func`.

    Xin lưu ý lại: không có dấu ngoặc đơn sau `sayHi`. Nếu có, thì `func = sayHi()` sẽ viết *kết quả của cuộc gọi* `sayHi()` vào `func`, chứ không phải *hàm* `sayHi`.
    
3. Bây giờ hàm có thể được gọi là cả `sayHi()` và `func()`.

Lưu ý rằng chúng ta cũng có thể sử dụng một Biểu thức hàm (Function Expression) để khai báo `sayHi`, trong dòng đầu tiên:

```js
      let sayHi = function() { ... };

      let func = sayHi;
      // ...
```

Mọi thứ sẽ làm việc như nhau. Thậm chí rõ ràng hơn những gì đang xảy ra, phải không?

<br>

> ---

**📌 Tại sao cuối cùng lại có dấu chấm phẩy?**

Bạn có thể tự hỏi, tại sao cuối Biểu thức hàm (Function Expression) có dấu chấm phẩy `;` ở cuối, nhưng Tuyên bố hàm (Function Declaration) không:

```js
      function sayHi() {
        // ...
      }

      let sayHi = function() {
        // ...
      };
```

Đáp án đơn giản:
- Không cần `;` ở cuối khối mã và cấu trúc cú pháp sử dụng chúng như `if {...}`, `for { ... }`, `function f { ... }`, v.v.
- Một biểu thức hàm (Function Expression) được sử dụng bên trong câu lệnh: `let sayHi = ...;`, làm giá trị. Đây không phải là một khối mã. Dấu chấm phẩy `;` được khuyến nghị ở cuối các câu lệnh, bất kể giá trị là gì. Vì vậy, dấu chấm phẩy ở đây không liên quan đến chính Biểu thức hàm (Function Expression) theo bất kỳ cách nào, nó chỉ chấm dứt câu lệnh.

> ---

<br>

## Hàm gọi lại (Callback functions)

Chúng ta hãy xem xét thêm nhiều ví dụ về việc truyền hàm dưới dạng giá trị và sử dụng biểu thức hàm (function expressions).

Chúng ta sẽ viết một hàm `ask(question, yes, no)` với ba tham số:

`question` Văn bản của câu hỏi

`yes` Function để chạy nếu câu trả lời là "Yes"

`no` Function để chạy nếu câu trả lời là "No"

Function nên hỏi `question` và, tùy thuộc vào câu trả lời của người dùng, gọi `yes()` hoặc `no()`:

```js
      function ask(question, yes, no) {
        if (confirm(question)) yes()
        else no();
      }

      function showOk() {
        alert( "You agreed." );
      }

      function showCancel() {
        alert( "You canceled the execution." );
      }

      // usage: functions showOk, showCancel are passed as arguments to ask
      ask("Do you agree?", showOk, showCancel);
```

Trước khi chúng ta khám phá cách chúng ta có thể viết nó theo cách ngắn hơn nhiều, hãy lưu ý rằng trong trình duyệt (và ở phía máy chủ trong một số trường hợp) các chức năng như vậy khá phổ biến. Sự khác biệt chính giữa triển khai thực tế và ví dụ ở trên là các hàm trong đời thực sử dụng các cách phức tạp hơn để tương tác với người dùng hơn là một `confirm` đơn giản. Trong trình duyệt, một chức năng như vậy thường vẽ ra một cửa sổ câu hỏi trông đẹp mắt. Nhưng nó là một câu chuyện khác.

**Các đối số của `ask` được gọi là *hàm gọi lại (callback functions)* hoặc chỉ *gọi lại (callbacks)*.**

Ý tưởng là chúng ta sẽ truyền một hàm và hy vọng nó sẽ được "gọi lại (called back)" sau nếu cần. Trong trường hợp của chúng ta, `showOk` trở thành callback cho câu trả lời "yes" và `showCancel` cho câu trả lời "no".

Chúng ta có thể sử dụng Biểu thức hàm (Function Expressions) để viết cùng một hàm ngắn hơn nhiều:

```js
      function ask(question, yes, no) {
        if (confirm(question)) yes()
        else no();
      }

      ask(
        "Do you agree?",
        function() { alert("You agreed."); },
        function() { alert("You canceled the execution."); }
      );
```

Ở đây, các hàm được khai báo ngay bên trong lệnh gọi `ask(...)`. Chúng không có tên, và vì vậy được gọi là *anonymous*. Các hàm như vậy không thể truy cập được bên ngoài `ask` (vì chúng không được gán cho các biến), nhưng đó là những gì chúng ta muốn ở đây.

Mã như vậy xuất hiện trong các tập lệnh của chúng ta rất tự nhiên, đó là tinh thần của JavaScript.

<br>

> ---

**📌 Hàm là một giá trị đại diện cho một "hành động"**

Các giá trị thông thường như chuỗi hoặc số đại diện cho *dữ liệu*.

Một function có thể được coi là một *hành động*.

Chúng ta có thể chuyển nó giữa các biến và chạy khi chúng ta muốn.

> ---

<br>

## Biểu thức hàm (Function Expression) và khai báo hàm (Function Declaration)

Chúng ta hãy hình thành sự khác biệt chính giữa Function Declarations và Expressions.

Đầu tiên, cú pháp: làm thế nào để xem cái gì là cái gì (what is what) trong mã.

- *Khai báo hàm (Function Declaration):* một hàm, được khai báo là một câu lệnh riêng, trong luồng mã chính.

    ```js
    // Function Declaration
    function sum(a, b) {
      return a + b;
    }
    ```
- *Biểu thức hàm (Function Expression):* một hàm, được tạo bên trong một biểu thức hoặc bên trong một cấu trúc cú pháp khác. Ở đây, hàm được tạo ở phía bên phải của "biểu thức gán" `=`:
    
    ```js
    // Function Expression
    let sum = function(a, b) {
      return a + b;
    };
    ```

Sự khác biệt tinh tế hơn là *khi* một function được tạo bởi JavaScript engine.

**Một Function Expression được tạo khi thực thi gặp nó và có thể sử dụng được từ đó trở đi.**

Khi luồng thực thi chuyển sang phía bên phải của phép gán `let sum = function…` -- bắt đầu từ đây, hàm được tạo và có thể được sử dụng (được gán, được gọi, v.v.) từ bây giờ.

Function Declarations là khác biệt.

**Một Function Declaration có thể sử dụng được trong toàn bộ script/code block.**

Nói cách khác, khi JavaScript *chuẩn bị* để chạy tập lệnh hoặc khối mã, trước tiên, nó sẽ tìm các Function Declaration trong nó và tạo các hàm. Chúng ta có thể nghĩ về nó như là một "giai đoạn khởi tạo".

Và sau khi tất cả các Function Declaration được xử lý, việc thực thi sẽ tiếp tục.

Kết quả là, một hàm được khai báo là một Function Declaration có thể được gọi sớm hơn nó được định nghĩa.

Ví dụ, điều này hoạt động:

```js
      sayHi("John"); // Hello, John

      function sayHi(name) {
        alert( `Hello, ${name}` );
      }
```

Function Declaration `sayHi` được tạo khi JavaScript đang chuẩn bị khởi động tập lệnh và hiển thị ở mọi nơi trong nó.

...Nếu đó là Function Expression, thì nó sẽ không hoạt động:

```js
      sayHi("John"); // error!

      let sayHi = function(name) {  // (*) no magic any more
        alert( `Hello, ${name}` );
      };
```

Các Function Expression được tạo khi thực thi bắt gặp chúng. Điều đó chỉ xảy ra trong dòng `(*)`. Quá muộn.

**Khi một Function Declaration được thực hiện trong một khối mã (code block), nó có thể nhìn thấy ở mọi nơi trong khối đó. Nhưng không phải bên ngoài nó.**

Đôi khi, việc khai báo một hàm cục bộ chỉ cần trong khối đó là điều hữu ích. Nhưng tính năng đó cũng có thể gây ra vấn đề.

Chẳng hạn, hãy tưởng tượng rằng chúng ta cần khai báo một hàm `welcome()` tùy thuộc vào biến `age` mà chúng ta nhận được trong thời gian chạy. Và sau đó chúng ta dự định sử dụng nó một thời gian sau.

Mã dưới đây không hoạt động:

```js
      let age = prompt("What is your age?", 18);

      // conditionally declare a function
      if (age < 18) {

        function welcome() {
          alert("Hello!");
        }

      } else {

        function welcome() {
          alert("Greetings!");
        }

      }

      // ...use it later
      welcome(); // Error: welcome is not defined
```

Đó là bởi vì một Function Declaration chỉ hiển thị bên trong khối mã (code block) mà nó nằm trong đó.

Đây là một ví dụ khác:

```js
      let age = 16; // take 16 as an example

      if (age < 18) {
        welcome();               // \   (runs)
                                 //  |
        function welcome() {     //  |  
          alert("Hello!");       //  |  Function Declaration is available
        }                        //  |  everywhere in the block where it's declared
                                 //  |
        welcome();               // /   (runs)

      } else {

        function welcome() {     //  for age = 16, this "welcome" is never created
          alert("Greetings!");
        }
      }

      // Here we're out of curly braces,
      // so we can not see Function Declarations made inside of them.

      welcome(); // Error: welcome is not defined
```

Chúng ta có thể làm gì để làm cho `welcome` hiển thị bên ngoài `if`?

Cách tiếp cận đúng sẽ là sử dụng Function Expression và gán `welcome` cho biến được khai báo bên ngoài `if` và có mức độ hiển thị phù hợp.

Bây giờ nó hoạt động như dự định:

```js
      let age = prompt("What is your age?", 18);

      let welcome;

      if (age < 18) {

        welcome = function() {
          alert("Hello!");
        };

      } else {

        welcome = function() {
          alert("Greetings!");
        };

      }

      welcome(); // ok now
```

Hoặc chúng ta có thể đơn giản hóa nó hơn nữa bằng cách sử dụng toán tử dấu hỏi `?`:

```js
      let age = prompt("What is your age?", 18);

      let welcome = (age < 18) ?
        function() { alert("Hello!"); } :
        function() { alert("Greetings!"); };

      welcome(); // ok now
```

<br>

> ---

**📌 Khi nào bạn nên chọn Khai báo hàm (Function Declaration) so với biểu thức hàm (Function Expression)?**

Theo nguyên tắc thông thường, khi chúng ta cần khai báo một hàm, đầu tiên cần xem xét là cú pháp Khai báo hàm, hàm chúng ta đã sử dụng trước đó. Nó cho phép tự do hơn trong cách tổ chức mã của chúng ta, bởi vì chúng ta có thể gọi các hàm như vậy trước khi chúng được khai báo.

Cũng dễ dàng hơn một chút để tìm kiếm `function f(…) {…}` trong mã so với `let f = function(…) {…}`. Function Declarations trông "bắt mắt" hơn.

...Nhưng nếu Function Declaration không phù hợp với chúng ta vì một số lý do (chúng ta đã thấy một ví dụ ở trên), thì nên sử dụng Function Expression.

> ---

<br>

## Hàm mũi tên (Arrow functions)

Có một cú pháp rất đơn giản và ngắn gọn để tạo hàm, thường tốt hơn Biểu thức hàm (Function Expressions). Nó được gọi là "hàm mũi tên", bởi vì nó trông như thế này:


```js
      let func = (arg1, arg2, ...argN) => expression
```

...Điều này tạo ra một hàm `func` có các đối số `arg1..argN`, đánh giá `biểu thức` ở phía bên phải với việc sử dụng chúng và trả về kết quả của nó.

Nói cách khác, nó giống như:

```js
      let func = function(arg1, arg2, ...argN) {
        return expression;
      };
```

...Nhưng ngắn gọn hơn nhiều.

Hãy xem một ví dụ:

```js
      let sum = (a, b) => a + b;

      /* 
      
      The arrow function is a shorter form of:

      let sum = function(a, b) {
        return a + b;
      };
      
      */

      alert( sum(1, 2) ); // 3
```

Nếu chúng ta chỉ có một đối số, thì dấu ngoặc đơn có thể được bỏ qua, làm cho nó thậm chí ngắn hơn:

```js
      // same as
      // let double = function(n) { return n * 2 }
      let double = n => n * 2;

      alert( double(3) ); // 6
```

Nếu không có đối số, dấu ngoặc đơn sẽ trống (nhưng chúng phải có mặt):

```js
      let sayHi = () => alert("Hello!");

      sayHi();
```

Các hàm mũi tên có thể được sử dụng giống như Biểu thức hàm.

Chẳng hạn, đây là ví dụ viết lại với `welcome()`:

```js
      let age = prompt("What is your age?", 18);

      let welcome = (age < 18) ?
        () => alert('Hello') :
        () => alert("Greetings!");

      welcome(); // ok now
```

Các arrow function có thể không quen thuộc và không dễ đọc lúc đầu, nhưng điều đó nhanh chóng thay đổi khi mắt quen với cấu trúc.

Chúng rất thuận tiện cho các hành động một dòng đơn giản, khi chúng ta quá lười để viết nhiều từ.

<br>

> ---

**📌 Multiline arrow functions**

Các ví dụ ở trên đã lấy các đối số từ bên trái của `=>` và đánh giá biểu thức phía bên phải với chúng.

Đôi khi chúng ta cần một cái gì đó phức tạp hơn một chút, như nhiều biểu thức hoặc câu lệnh. Cũng có thể, nhưng chúng ta nên đặt chúng trong các dấu ngoặc nhọn. Sau đó sử dụng một `return` bình thường trong chúng.

Như thế này:

```js
      let sum = (a, b) => {  // the curly brace opens a multiline function
        let result = a + b;
        return result; // if we use curly braces, use return to get results
      };

      alert( sum(1, 2) ); // 3
```

> ---

<br>
<br>

> ---

**📌 More to come**

Ở đây chúng ta ca ngợi các arrow function cho ngắn gọn. Nhưng đó không phải là tất cả! Arrow functions có các tính năng thú vị khác. Chúng ta sẽ trở lại với chúng sau trong chương **arrow-functions**.

Hiện tại, chúng ta đã có thể sử dụng chúng cho các hành động và cuộc gọi lại một dòng.

> ---

<br>

## Tóm lược

- Các hàm là các giá trị. Chúng có thể được gán, sao chép hoặc khai báo ở bất kỳ vị trí nào của mã.
- Nếu hàm được khai báo là một câu lệnh riêng trong luồng mã chính, thì đó gọi là "Khai báo hàm (Function Declaration)".
- Nếu hàm được tạo như một phần của biểu thức, nó được gọi là "Biểu thức hàm (Function Expression)".
- Function Declarations được xử lý trước khi khối mã được thực thi. Chúng có thể nhìn thấy ở mọi nơi trong khối.
- Function Expressions được tạo khi luồng thực thi đến chúng.

Trong hầu hết các trường hợp khi chúng ta cần khai báo một hàm, Function Declaration là thích hợp hơn, bởi vì nó có tác dụng trước chính các Function Declaration đó. Điều đó cho chúng ta linh hoạt hơn trong tổ chức mã và thường dễ đọc hơn.

Vì vậy, chúng ta chỉ nên sử dụng Function Expression khi Function Declaration không phù hợp với nhiệm vụ. Chúng ta đã thấy một vài ví dụ về điều đó trong chương này và sẽ thấy nhiều hơn trong tương lai.

Các arrow function là tiện dụng cho một dòng. Chúng có hai hương vị:

1. Không có dấu ngoặc nhọn: `(...args) => expression` -- phía bên phải là một biểu thức: hàm đánh giá nó và trả về kết quả.
2. Với dấu ngoặc nhọn: `(...args) => { body }` -- các dấu ngoặc cho phép chúng ta viết nhiều câu lệnh bên trong hàm, nhưng chúng ta cần một `return` rõ ràng để trả về một cái gì đó.
