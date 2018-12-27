# Error handling, "try..catch"

Cho dù chúng ta có giỏi lập trình đến đâu, đôi khi các tập lệnh của chúng ta cũng có lỗi. Chúng có thể xảy ra do lỗi của chúng ta, đầu vào của người dùng không mong muốn, phản hồi của máy chủ bị lỗi và vì hàng ngàn lý do khác.

Thông thường, một tập lệnh "chết" (dừng ngay lập tức) trong trường hợp có lỗi, in nó ra console.

Nhưng có một cú pháp construct `try..catch` cho phép "bắt" các lỗi và, thay vì chết, hãy làm điều gì đó hợp lý hơn.

## The "try..catch" syntax

Cấu trúc `try..catch` có hai khối chính: `try` và sau đó `catch`:

```js
try {

  // code...

} catch (err) {

  // error handling

}
```

Nó hoạt động như thế này:

1. Đầu tiên, mã trong `try {...}` được thực thi.
2. Nếu không có lỗi, thì `catch(err)` bị bỏ qua: việc thực thi đến hết `try` và sau đó nhảy qua `catch`.
3. Nếu xảy ra lỗi, thì việc thực thi `try` bị dừng lại và control sẽ chuyển đến đầu của `catch(err)`. Biến `err` (có thể sử dụng bất kỳ tên nào cho nó) chứa một error object với các chi tiết về những gì đã xảy ra.

![](try-catch-flow.png)

Vì vậy, một lỗi bên trong khối `try {…}` không giết được tập lệnh: chúng ta có cơ hội xử lý nó trong `catch`.

Hãy xem thêm ví dụ.

- Một ví dụ không có lỗi: hiển thị `alert` `(1)` và `(2)`:

    ```js
    try {

      alert('Start of try runs');  // (1) <--

      // ...no errors here

      alert('End of try runs');   // (2) <--

    } catch(err) {

      alert('Catch is ignored, because there are no errors'); // (3)

    }

    alert("...Then the execution continues");
    ```

- Một ví dụ có lỗi: hiển thị `(1)` và `(3)`:

    ```js
    try {

      alert('Start of try runs');  // (1) <--

      lalala; // error, variable is not defined!

      alert('End of try (never reached)');  // (2)

    } catch(err) {

      alert(`Error has occured!`); // (3) <--

    }

    alert("...Then the execution continues");
    ```

<br>

> ---

**📌 `try..catch` chỉ hoạt động đối với các runtime errors**

Để `try..catch` hoạt động, mã phải chạy được. Nói cách khác, nó phải là JavaScript hợp lệ.

Nó sẽ không hoạt động nếu mã bị sai về mặt cú pháp, ví dụ, nó có dấu ngoặc nhọn không khớp:

```js
try {
  {{{{{{{{{{{{
} catch(e) {
  alert("The engine can't understand this code, it's invalid");
}
```

The JavaScript engine trước tiên đọc mã và sau đó chạy nó. Các lỗi xảy ra trên cụm từ đọc được gọi là lỗi "parse-time" và không thể phục hồi (từ bên trong mã đó). Đó là bởi vì engine không thể hiểu được mã.

Vì vậy, `try..catch` chỉ có thể xử lý các lỗi xảy ra trong mã hợp lệ. Các lỗi như vậy được gọi là "runtime errors" hoặc, đôi khi, "exceptions".

> ---

<br>
<br>

> ---

**📌 `try..catch` works synchronously**

Nếu một ngoại lệ xảy ra trong mã "scheduled", như trong `setTimeout`, thì `try..catch` sẽ không bắt được nó:

```js
try {
  setTimeout(function() {
    noSuchVariable; // script will die here
  }, 1000);
} catch (e) {
  alert( "won't work" );
}
```

Đó là bởi vì `try..catch` thực sự bọc (wraps) cuộc gọi `setTimeout` lại để lên lịch cho hàm đó (schedules the function). Nhưng chính function này được thực thi sau đó, khi engine đã rời khỏi cấu trúc `try..catch`.

Để bắt một ngoại lệ bên trong một scheduled function, `try..catch` phải nằm trong function đó:

```js
setTimeout(function() {
  try {    
    noSuchVariable; // try..catch handles the error!
  } catch (e) {
    alert( "error is caught here!" );
  }
}, 1000);
```

> ---

<br>

## Error object

Khi xảy ra lỗi, JavaScript sẽ tạo một đối tượng chứa các chi tiết về nó. Đối tượng sau đó được chuyển làm đối số cho `catch`:

```js
try {
  // ...
} catch(err) { // <-- the "error object", could use another word instead of err
  // ...
}
```

Đối với tất cả các built-in errors, đối tượng lỗi trong `catch` block có hai thuộc tính chính:

**`name`**

Error name. Đối với một undefined variable đó là `"ReferenceError"`.

**`message`**

Tin nhắn văn bản về chi tiết lỗi.

Có các thuộc tính phi tiêu chuẩn (non-standard properties) khác có sẵn trong hầu hết các môi trường (environments). Một trong những sử dụng và hỗ trợ rộng rãi nhất là:

**`stack`**

Ngăn xếp cuộc gọi hiện tại (Current call stack): một string với thông tin về chuỗi các cuộc gọi lồng nhau dẫn đến lỗi. Được sử dụng cho mục đích gỡ lỗi.

Ví dụ:

```js
try {
  lalala; // error, variable is not defined!
} catch(err) {
  alert(err.name); // ReferenceError
  alert(err.message); // lalala is not defined
  alert(err.stack); // ReferenceError: lalala is not defined at ...

  // Can also show an error as a whole
  // The error is converted to string as "name: message"
  alert(err); // ReferenceError: lalala is not defined
}
```

## Using "try..catch"

Chúng ta hãy khám phá một trường hợp sử dụng thực tế của `try..catch`.

Như chúng ta đã biết, JavaScript hỗ trợ phương thức [JSON.parse(str)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/JSON/parse) để đọc JSON-encoded values.

Thông thường, nó được sử dụng để giải mã dữ liệu nhận được qua mạng, từ máy chủ hoặc nguồn khác.

Chúng ta nhận được nó và gọi `JSON.parse`, như thế này:

```js
let json = '{"name":"John", "age": 30}'; // data from the server

let user = JSON.parse(json); // convert the text representation to JS object

// now user is an object with properties from the string
alert( user.name ); // John
alert( user.age );  // 30
```

Bạn có thể tìm thấy thông tin chi tiết hơn về JSON trong chương **json**.

** Nếu `json` không đúng định dạng, `JSON.parse` sẽ phát sinh lỗi, vì vậy tập lệnh "chết".**

Chúng ta có nên hài lòng với điều đó? Tất nhiên là không rồi!

Bằng cách này, nếu có gì đó không đúng với dữ liệu, khách truy cập sẽ không bao giờ biết điều đó (trừ khi họ mở bảng điều khiển dành cho nhà phát triển). Và mọi người thực sự không thích khi một cái gì đó "chỉ chết" mà không có bất kỳ thông báo lỗi nào.

Hãy sử dụng `try..catch` để xử lý lỗi:

```js
let json = "{ bad json }";

try {

  let user = JSON.parse(json); // <-- when an error occurs...
  alert( user.name ); // doesn't work

} catch (e) {
  // ...the execution jumps here
  alert( "Our apologies, the data has errors, we'll try to request it one more time." );
  alert( e.name );
  alert( e.message );
}
```

Ở đây, chúng ta chỉ sử dụng `catch` block để hiển thị thông báo, nhưng chúng ta có thể làm nhiều hơn thế: gửi yêu cầu mạng mới, đề xuất một giải pháp thay thế cho khách truy cập, gửi thông tin về lỗi đến cơ sở ghi nhật ký, v.v. Tất cả tốt hơn nhiều so với chỉ chết.

## Throwing our own errors

Điều gì xảy ra nếu `json` đúng về mặt cú pháp, nhưng không có thuộc tính `name` bắt buộc?

Như thế này:

```js
let json = '{ "age": 30 }'; // incomplete data

try {

  let user = JSON.parse(json); // <-- no errors
  alert( user.name ); // no name!

} catch (e) {
  alert( "doesn't execute" );
}
```

Ở đây `JSON.parse` chạy bình thường, nhưng sự vắng mặt của `name` thực sự là một lỗi cho chúng ta.

Để thống nhất xử lý lỗi, chúng ta sẽ sử dụng toán tử `throw`.

### "Throw" operator

Toán tử `throw` tạo ra một lỗi.

Cú pháp là:

```js
throw <error object>
```

Về mặt kỹ thuật, chúng ta có thể sử dụng bất cứ thứ gì như một error object. Đó có thể là một nguyên thủy, như một số hoặc một chuỗi, nhưng tốt hơn là sử dụng các đối tượng, tốt nhất là với các thuộc tính `name` và `message` (để tương thích với các built-in errors).

JavaScript có nhiều built-in constructors cho các lỗi tiêu chuẩn: `Error`, `SyntaxError`, `ReferenceError`, `TypeError` và các loại khác. Chúng ta có thể sử dụng chúng để tạo các error objects là tốt.

Cú pháp của chúng là:

```js
let error = new Error(message);
// or
let error = new SyntaxError(message);
let error = new ReferenceError(message);
// ...
```

Đối với các built-in errors (không phải cho bất kỳ đối tượng nào, chỉ cho các lỗi), thuộc tính `name` chính xác là tên của constructor. Và `message` được lấy từ đối số.

Ví dụ:

```js
let error = new Error("Things happen o_O");

alert(error.name); // Error
alert(error.message); // Things happen o_O
```

Chúng ta hãy xem loại lỗi nào `JSON.parse` tạo ra:

```js
try {
  JSON.parse("{ bad json o_O }");
} catch(e) {
  alert(e.name); // SyntaxError
  alert(e.message); // Unexpected token o in JSON at position 0
}
```

Như chúng ta có thể thấy, đó là một `SyntaxError`.

Và trong trường hợp của chúng ta, sự vắng mặt của `name` cũng có thể được coi là một lỗi cú pháp, giả sử rằng người dùng phải có một `name`.

Vì vậy, hãy ném (throw) nó:

```js
let json = '{ "age": 30 }'; // incomplete data

try {

  let user = JSON.parse(json); // <-- no errors

  if (!user.name) {
    throw new SyntaxError("Incomplete data: no name"); // (*)
  }

  alert( user.name );

} catch(e) {
  alert( "JSON Error: " + e.message ); // JSON Error: Incomplete data: no name
}
```

Trong dòng `(*)`, toán tử `throw` tạo ra một `SyntaxError` với `message` đã cho, giống như cách JavaScript sẽ tự tạo ra nó. Việc thực thi `try` ngay lập tức dừng lại và luồng điều khiển nhảy vào `catch`.

Bây giờ `catch` đã trở thành một nơi duy nhất cho tất cả các error handling: cả cho `JSON.parse` và các trường hợp khác.

## Rethrowing

Trong ví dụ trên, chúng ta sử dụng `try..catch` để xử lý dữ liệu không chính xác. Nhưng có thể là *một lỗi không mong muốn khác* xảy ra trong khối `try {...}` không? Giống như một biến là undefined hoặc một cái gì đó khác, không chỉ là "dữ liệu không chính xác".

Như thế này:

```js
let json = '{ "age": 30 }'; // incomplete data

try {
  user = JSON.parse(json); // <-- forgot to put "let" before user

  // ...
} catch(err) {
  alert("JSON Error: " + err); // JSON Error: ReferenceError: user is not defined
  // (no JSON Error actually)
}
```

Tất nhiên, mọi thứ đều có thể! Lập trình viên làm sai. Ngay cả trong các tiện ích nguồn mở được sử dụng bởi hàng triệu người trong nhiều thập kỷ - đột nhiên một lỗi điên có thể được phát hiện dẫn đến các vụ hack khủng khiếp (giống như nó đã xảy ra với công cụ `ssh`).

Trong trường hợp của chúng ta, `try..catch` có nghĩa là bắt lỗi "dữ liệu không chính xác". Nhưng theo bản chất của nó, `catch` lấy *tất cả* lỗi từ `try`. Ở đây, nó gặp một lỗi không mong muốn, nhưng vẫn hiển thị cùng thông báo `"JSON Error"`. Điều đó sai và cũng làm cho mã khó gỡ lỗi hơn.

May mắn thay, chúng ta có thể tìm ra lỗi nào chúng ta nhận được, ví dụ từ `name`:

```js
try {
  user = { /*...*/ };
} catch(e) {
  alert(e.name); // "ReferenceError" for accessing an undefined variable
}
```

Quy tắc rất đơn giản:

**Catch chỉ nên xử lý các lỗi mà nó biết và "rethrow" tất cả các lỗi khác.**

Kỹ thuật "rethrowing" có thể được giải thích chi tiết hơn như sau:

1. Bắt được tất cả các lỗi.
2. Trong khối `catch(err) {...}`, chúng ta phân tích đối tượng lỗi `err`.
2. Nếu chúng ta không biết cách xử lý nó, thì chúng ta sẽ `throw err`.

Trong đoạn mã dưới đây, chúng ta sử dụng rethrowing để `catch` chỉ xử lý `SyntaxError`:

```js
let json = '{ "age": 30 }'; // incomplete data
try {

  let user = JSON.parse(json);

  if (!user.name) {
    throw new SyntaxError("Incomplete data: no name");
  }

  blabla(); // unexpected error

  alert( user.name );

} catch(e) {

  if (e.name == "SyntaxError") {
    alert( "JSON Error: " + e.message );
  } else {
    throw e; // rethrow (*)
  }

}
```

Lỗi throwing vào dòng `(*)` từ bên trong `catch` block "rơi ra" của `try..catch` và có thể bị bắt bởi một outer `try..catch` construct (nếu nó tồn tại), hoặc nó giết chết kịch bản.

Vì vậy, khối `catch` thực sự chỉ xử lý các lỗi mà nó biết cách xử lý và "bỏ qua" tất cả các lỗi khác.

Ví dụ dưới đây minh họa cách các lỗi như vậy có thể bị bắt bởi một cấp độ nữa của `try..catch`:

```js
function readData() {
  let json = '{ "age": 30 }';

  try {
    // ...
    blabla(); // error!
  } catch (e) {
    // ...
    if (e.name != 'SyntaxError') {
      throw e; // rethrow (don't know how to deal with it)
    }
  }
}

try {
  readData();
} catch (e) {
  alert( "External catch got: " + e ); // caught it!
}
```

Ở đây `readData` chỉ biết cách xử lý `SyntaxError`, trong khi `try..catch` bên ngoài biết cách xử lý mọi thứ.

## try..catch..finally

Đợi đã, đó không phải là tất cả.

Cấu trúc `try..catch` có thể có thêm một mệnh đề mã: `finally`.

Nếu nó tồn tại, nó chạy trong mọi trường hợp:

- sau `try`, nếu không có lỗi,
- sau `catch`, nếu có lỗi.

Cú pháp mở rộng trông như thế này:

```js
try {
   ... try to execute the code ...
} catch(e) {
   ... handle errors ...
} finally {
   ... execute always ...
}
```

Hãy thử chạy mã này:

```js
try {
  alert( 'try' );
  if (confirm('Make an error?')) BAD_CODE();
} catch (e) {
  alert( 'catch' );
} finally {
  alert( 'finally' );
}
```

Mã này có hai cách thực hiện:

1. Nếu bạn trả lời "Yes" cho "Make an error?", Sau đó `try -> catch -> finally`.
2. Nếu bạn nói "No", thì `try -> finally`.

Mệnh đề `finally` thường được sử dụng khi chúng ta bắt đầu làm một cái gì đó trước khi `try..catch` và muốn hoàn thiện nó trong mọi trường hợp của kết quả.

Chẳng hạn, chúng ta muốn đo thời gian mà một Fibonacci numbers function `fib(n)` cần. Đương nhiên, chúng ta có thể bắt đầu đo trước khi nó chạy và kết thúc sau đó. Nhưng nếu có lỗi trong khi gọi hàm thì sao? Cụ thể, việc triển khai `fib(n)` trong đoạn mã dưới đây trả về lỗi cho các số âm hoặc không nguyên.

Mệnh đề `finally` là một nơi tuyệt vời để hoàn thành các phép đo bất kể là gì.

Ở đây `finally` đảm bảo rằng thời gian sẽ được đo chính xác trong cả hai tình huống -- trong trường hợp thực hiện thành công `fib` và trong trường hợp có lỗi trong đó:

```js
let num = +prompt("Enter a positive integer number?", 35)

let diff, result;

function fib(n) {
  if (n < 0 || Math.trunc(n) != n) {
    throw new Error("Must not be negative, and also an integer.");
  }
  return n <= 1 ? n : fib(n - 1) + fib(n - 2);
}

let start = Date.now();

try {
  result = fib(num);
} catch (e) {
  result = 0;
} finally {
  diff = Date.now() - start;
}

alert(result || "error occured");

alert( `execution took ${diff}ms` );
```

Bạn có thể kiểm tra bằng cách chạy mã bằng cách nhập `35` vào `prompt` -- nó thực thi bình thường, `finally` sau `try`. Và sau đó nhập `-1` -- sẽ có một lỗi ngay lập tức, việc thực thi sẽ mất `0ms`. Cả hai phép đo đều được thực hiện chính xác.

Nói cách khác, có thể có hai cách để thoát khỏi hàm: hoặc là `return` hoặc `throw`. Mệnh đề `finally` xử lý cả hai.

<br>

> ---

**📌 Các biến là cục bộ bên trong `try..catch..finally`**

Xin lưu ý rằng các biến `result` và `diff` trong đoạn mã trên được khai báo *trước* `try..catch`.

Mặt khác, nếu `let` được tạo bên trong khối `{...}`, nó sẽ chỉ hiển thị bên trong khối.

> ---

<br>
<br>

> ---

**📌 `finally` and `return`**

Mệnh đề `finally` hoạt động cho *any* exit từ `try..catch`. Điều đó bao gồm một `return` rõ ràng.

Trong ví dụ dưới đây, có một `return` trong `try`. Trong trường hợp này, `finally` được thực thi ngay trước khi control quay trở lại outer code.

```js
function func() {

  try {
    return 1;

  } catch (e) {
    /* ... */
  } finally {
    alert( 'finally' );
  }
}

alert( func() ); // first works alert from finally, and then this one
```

> ---

<br>
<br>

> ---

**📌 `try..finally`**

Cấu trúc `try..finally`, không có mệnh đề `catch`, cũng hữu ích. Chúng ta áp dụng nó khi chúng ta không muốn xử lý lỗi ngay tại đây, nhưng muốn chắc chắn rằng các quy trình mà chúng ta đã bắt đầu được hoàn tất.

```js
function func() {
  // start doing something that needs completion (like measurements)
  try {
    // ...
  } finally {
    // complete that thing even if all dies
  }
}
```

Trong đoạn mã trên, một lỗi bên trong `try` luôn luôn rơi ra, bởi vì không có `catch`. Nhưng `finally` hoạt động trước khi dòng thực thi nhảy ra ngoài.

> ---

<br>

## Global catch

<br>

> ---

**📌 Environment-specific**

Thông tin từ phần này không phải là một phần của core JavaScript.

> ---

<br>

Chúng ta hãy tưởng tượng rằng chúng ta đã có một lỗi nghiêm trọng bên ngoài `try..catch`, và kịch bản đã chết. Giống như một lỗi lập trình hoặc một cái gì đó khủng khiếp.

Có cách nào để phản ứng về những sự cố như vậy không? Chúng ta có thể muốn log lại lỗi, hiển thị một cái gì đó cho người dùng (thông thường họ không thấy thông báo lỗi), v.v.

Không có gì trong đặc tả, nhưng environments thường cung cấp nó, bởi vì nó thực sự hữu ích. Chẳng hạn, [Node.JS có [process.on('uncaughtException')](https://nodejs.org/api/process.html#process_event_uncaughtexception) cho điều đó. Và trong trình duyệt, chúng ta có thể gán một hàm cho thuộc tính [window.onerror](https://developer.mozilla.org/en-US/docs/Web/API/GlobalEventHandlers/onerror) đặc biệt. Nó sẽ chạy trong trường hợp lỗi Không bị bắt.

Cú pháp:

```js
window.onerror = function(message, url, line, col, error) {
  // ...
};
```

**`message`**

Error message.

**`url`**

URL của tập lệnh nơi xảy ra lỗi.

**`line`, `col`**

Số dòng và cột nơi xảy ra lỗi.

**`error`**

Error object.

Ví dụ:

```html
<script>
  window.onerror = function(message, url, line, col, error) {
    alert(`${message}\n At ${line}:${col} of ${url}`);
  };

  function readData() {
    badFunc(); // Whoops, something went wrong!
  }

  readData();
</script>
```

Vai trò của global handler `window.onerror` thường không phục hồi việc thực thi tập lệnh -- điều đó có lẽ là không thể trong trường hợp có lỗi lập trình, nhưng để gửi thông báo lỗi cho nhà phát triển.

Ngoài ra còn có các dịch vụ web cung cấp ghi nhật ký lỗi cho các trường hợp như vậy, như <https://errorception.com> hoặc <http://www.muscula.com>.

Họ làm việc như thế này:

1. Chúng ta đăng ký tại dịch vụ và nhận một đoạn mã JS (hoặc một script URL) từ họ để chèn vào các trang.
2. JS script đó có hàm `window.onerror` tùy chỉnh.
3. Khi xảy ra lỗi, nó sẽ gửi một yêu cầu mạng về nó đến dịch vụ.
4. Chúng ta có thể đăng nhập vào giao diện web dịch vụ và thấy lỗi.

## Tóm lược

Cấu trúc `try..catch` cho phép xử lý các runtime errors. Nó thực sự cho phép try running the code và catch errors có thể xảy ra trong đó.

Cú pháp là:

```js
try {
  // run this code
} catch(err) {
  // if an error happened, then jump here
  // err is the error object
} finally {
  // do in any case after try/catch
}
```

Có thể không có phần `catch` hoặc không `finally`, vì vậy `try..catch` và `try..finally` cũng hợp lệ.

Các error objects có các thuộc tính sau:

- `message` -- thông báo lỗi có thể đọc được.
- `name` -- the string có tên lỗi (error constructor name).
- `stack` (non-standard) -- ngăn xếp tại thời điểm tạo lỗi.

Chúng ta cũng có thể tự tạo ra các lỗi của mình bằng cách sử dụng toán tử `throw`. Về mặt kỹ thuật, đối số của `throw` có thể là bất cứ điều gì, nhưng thông thường, đó là một đối tượng lỗi thừa hưởng từ built-in `Error` class. Thêm về extending errors trong chương tiếp theo.

Rethrowing là một mô hình xử lý lỗi cơ bản: một khối `catch` thường mong đợi và biết cách xử lý loại lỗi cụ thể, vì vậy nó nên điều chỉnh lại các lỗi mà nó không biết.

Ngay cả khi chúng ta không có `try..catch`, hầu hết các environments đều cho phép thiết lập một "global" error handler để bắt lỗi "rơi ra (fall out)". Trong trình duyệt đó là `window.onerror`.
