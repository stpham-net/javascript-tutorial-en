# Custom errors, extending Error

Khi chúng ta phát triển một cái gì đó, chúng ta thường cần các error classes của riêng mình để phản ánh những điều cụ thể có thể sai trong các nhiệm vụ của chúng ta. Đối với các lỗi trong hoạt động mạng (network operations), chúng ta có thể cần `HttpError`, đối với các hoạt động cơ sở dữ liệu (database operations) `DbError`, cho các hoạt động tìm kiếm `NotFoundError`, và tương tự v.v.

Các lỗi của chúng ta sẽ hỗ trợ các thuộc tính lỗi cơ bản như `message`, `name` và, tốt nhất là, `stack`. Nhưng chúng cũng có thể có các thuộc tính khác của riêng chúng, ví dụ: các `HttpError` objects có thể có thuộc tính `statusCode` với giá trị như `404` hoặc `403` hoặc `500`.

JavaScript cho phép sử dụng `throw` với bất kỳ đối số nào, vì vậy về mặt kỹ thuật, các custom error classes của chúng ta không cần phải kế thừa từ `Error`. Nhưng nếu chúng ta kế thừa, thì có thể sử dụng `obj instanceof Error` để xác định các error objects. Vì vậy, tốt hơn là thừa kế từ nó.

Khi chúng ta xây dựng ứng dụng của mình, các lỗi của chúng ta tự nhiên tạo thành một hệ thống phân cấp, ví dụ `HttpTimeoutError` có thể kế thừa từ `HttpError`, và tương tự v.v.

## Extending Error

Ví dụ, hãy xem xét một hàm `readUser(json)` nên đọc JSON với user data.

Đây là một ví dụ về cách một `json` hợp lệ có thể trông như thế nào:

```js
let json = `{ "name": "John", "age": 30 }`;
```

Trong nội bộ, chúng ta sẽ sử dụng `JSON.parse`. Nếu nó nhận được định dạng sai `json`, thì nó sẽ throws `SyntaxError`.

Nhưng ngay cả khi `json` đúng về mặt cú pháp, điều đó không có nghĩa đó là người dùng hợp lệ, phải không? Nó có thể bỏ lỡ các dữ liệu cần thiết. Chẳng hạn, nếu có thể không có thuộc tính `name` và `age` cần thiết cho người dùng của chúng ta.

Hàm `readUser(json)` của chúng ta sẽ không chỉ đọc JSON mà còn kiểm tra ("validate") dữ liệu. Nếu không có required fields, hoặc định dạng sai, thì đó là lỗi. Và đó không phải là một `SyntaxError`, vì dữ liệu đúng về mặt cú pháp, nhưng là một loại lỗi khác. Chúng ta sẽ gọi nó là `ValidationError` và tạo một class cho nó. Một lỗi thuộc loại đó cũng sẽ mang thông tin về trường vi phạm.

Lớp `ValidationError` của chúng ta sẽ kế thừa từ built-in `Error` class.

Lớp đó được tích hợp sẵn, nhưng chúng ta nên có mã gần đúng của nó trước mắt, để hiểu những gì chúng ta đang mở rộng.

So here you are:

```js
// The "pseudocode" for the built-in Error class defined by JavaScript itself
class Error {
  constructor(message) {
    this.message = message;
    this.name = "Error"; // (different names for different built-in error classes)
    this.stack = <nested calls>; // non-standard, but most environments support it
  }
}
```

Bây giờ chúng ta hãy tiếp tục và kế thừa `ValidationError` từ nó:

```js
class ValidationError extends Error {
  constructor(message) {
    super(message); // (1)
    this.name = "ValidationError"; // (2)
  }
}

function test() {
  throw new ValidationError("Whoops!");
}

try {
  test();
} catch(err) {
  alert(err.message); // Whoops!
  alert(err.name); // ValidationError
  alert(err.stack); // a list of nested calls with line numbers for each
}
```

Xin hãy xem the constructor:

1. Trong dòng `(1)`, chúng ta gọi parent constructor. JavaScript yêu cầu chúng ta gọi `super` trong child constructor, vì vậy điều đó là bắt buộc. The parent constructor đặt thuộc tính `message`.
2. The parent constructor cũng đặt thuộc tính `name` thành `"Error"`, vì vậy trong dòng `(2)`, chúng ta đặt lại nó về đúng giá trị.

Hãy thử sử dụng nó trong `readUser(json)`:

```js
class ValidationError extends Error {
  constructor(message) {
    super(message);
    this.name = "ValidationError";
  }
}

// Usage
function readUser(json) {
  let user = JSON.parse(json);

  if (!user.age) {
    throw new ValidationError("No field: age");
  }
  if (!user.name) {
    throw new ValidationError("No field: name");
  }

  return user;
}

// Working example with try..catch

try {
  let user = readUser('{ "age": 25 }');
} catch (err) {
  if (err instanceof ValidationError) {
    alert("Invalid data: " + err.message); // Invalid data: No field: name
  } else if (err instanceof SyntaxError) { // (*)
    alert("JSON Syntax Error: " + err.message);
  } else {
    throw err; // unknown error, rethrow it (**)
  }
}
```

Khối `try..catch` trong đoạn mã trên xử lý cả `ValidationError` và built-in `SyntaxError` từ `JSON.parse`.

Vui lòng xem cách chúng ta sử dụng `instanceof` để kiểm tra loại lỗi cụ thể trong dòng `(*)`.

Chúng ta cũng có thể xem ở `err.name`, như thế này:

```js
// ...
// instead of (err instanceof SyntaxError)
} else if (err.name == "SyntaxError") { // (*)
// ...
```  

Phiên bản `instanceof` tốt hơn nhiều, vì trong tương lai chúng ta sẽ mở rộng `ValidationError`, tạo các kiểu con của nó, như `PropertyRequiredError`. Và kiểm tra `instanceof` sẽ tiếp tục hoạt động cho các lớp kế thừa mới. Vì vậy, đó là bằng chứng trong tương lai (future-proof).

Ngoài ra, một điều quan trọng là nếu `catch` gặp một unknown error, thì nó sẽ rethrows nó trong dòng `(**)`. The `catch` chỉ biết cách xử lý lỗi validation và lỗi syntax, các loại khác (do lỗi chính tả trong mã hoặc như vậy) sẽ rơi qua (fall through).

## Further inheritance

Lớp `ValidationError` rất chung chung. Nhiều thứ có thể đi sai. Thuộc tính có thể vắng mặt hoặc nó có thể ở định dạng sai (như giá trị chuỗi cho `age`). Chúng ta hãy tạo một lớp cụ thể hơn `PropertyRequiredError`, chính xác cho các thuộc tính vắng mặt. Nó sẽ mang thêm thông tin về thuộc tính bị thiếu.

```js
class ValidationError extends Error {
  constructor(message) {
    super(message);
    this.name = "ValidationError";
  }
}

class PropertyRequiredError extends ValidationError {
  constructor(property) {
    super("No property: " + property);
    this.name = "PropertyRequiredError";
    this.property = property;
  }
}

// Usage
function readUser(json) {
  let user = JSON.parse(json);

  if (!user.age) {
    throw new PropertyRequiredError("age");
  }
  if (!user.name) {
    throw new PropertyRequiredError("name");
  }

  return user;
}

// Working example with try..catch

try {
  let user = readUser('{ "age": 25 }');
} catch (err) {
  if (err instanceof ValidationError) {
    alert("Invalid data: " + err.message); // Invalid data: No property: name
    alert(err.name); // PropertyRequiredError
    alert(err.property); // name
  } else if (err instanceof SyntaxError) {
    alert("JSON Syntax Error: " + err.message);
  } else {
    throw err; // unknown error, rethrow it
  }
}
```

Lớp mới `PropertyRequiredError` rất dễ sử dụng: chúng ta chỉ cần truyền tên thuộc tính: `new PropertyRequiredError(property)`. The human-readable `message` được tạo bởi the constructor.

Xin lưu ý rằng `this.name` trong `PropertyRequiredError` constructor một lần nữa được gán thủ công. Điều đó có thể trở thành một chút tẻ nhạt -- để gán `this.name = <class name>` khi tạo từng custom error. Nhưng có một lối thoát. Chúng ta có thể tạo lớp "basic error" của riêng mình để loại bỏ gánh nặng này khỏi vai bằng cách sử dụng `this.constructor.name` cho `this.name` trong the constructor. Và sau đó kế thừa từ nó.

Hãy gọi nó là `MyError`.

Đây là mã với `MyError` và các custom error classes khác, được đơn giản hóa:

```js
class MyError extends Error {
  constructor(message) {
    super(message);
    this.name = this.constructor.name;
  }
}

class ValidationError extends MyError { }

class PropertyRequiredError extends ValidationError {
  constructor(property) {
    super("No property: " + property);
    this.property = property;
  }
}

// name is correct
alert( new PropertyRequiredError("field").name ); // PropertyRequiredError
```

Bây giờ các custom errors ngắn hơn nhiều, đặc biệt là `ValidationError`, vì chúng ta đã loại bỏ dòng `"this.name = ..."` trong constructor.

## Wrapping exceptions

Mục đích của hàm `readUser` trong đoạn mã trên là "để đọc dữ liệu người dùng", phải không? Có thể xảy ra các loại lỗi khác nhau trong process. Ngay bây giờ chúng ta có `SyntaxError` và `ValidationError`, nhưng trong tương lai hàm `readUser` có thể phát triển: mã mới có thể sẽ tạo ra các loại lỗi khác.

Mã gọi `readUser` sẽ xử lý các lỗi này. Ngay bây giờ, nó sử dụng nhiều `if` trong khối `catch` để kiểm tra các loại lỗi khác nhau và rethrow các lỗi chưa biết. Nhưng nếu hàm `readUser` tạo ra một số loại lỗi nữa -- thì chúng ta nên tự hỏi: chúng ta có thực sự muốn kiểm tra từng loại lỗi một trong mỗi mã gọi `readUser` không?

Thường thì câu trả lời là "Không": mã bên ngoài (the outer code) muốn là "một cấp trên tất cả (one level above all that)". Nó muốn có một số loại "lỗi đọc dữ liệu (data reading error)". Tại sao chính xác nó đã xảy ra -- thường không liên quan (thông báo lỗi mô tả nó). Hoặc, thậm chí tốt hơn nếu có một cách để có được chi tiết lỗi, nhưng chỉ khi chúng ta cần.

Vì vậy, hãy tạo một lớp mới `ReadError` để thể hiện các lỗi đó. Nếu xảy ra lỗi bên trong `readUser`, chúng ta sẽ bắt lỗi ở đó và tạo `ReadError`. Chúng ta cũng sẽ giữ tham chiếu đến original error trong thuộc tính `cause` của nó. Sau đó, outer code sẽ chỉ phải kiểm tra `ReadError`.

Đây là mã định nghĩa `ReadError` và thể hiện việc sử dụng nó trong `readUser` và `try..catch`:

```js
class ReadError extends Error {
  constructor(message, cause) {
    super(message);
    this.cause = cause;
    this.name = 'ReadError';
  }
}

class ValidationError extends Error { /*...*/ }
class PropertyRequiredError extends ValidationError { /* ... */ }

function validateUser(user) {
  if (!user.age) {
    throw new PropertyRequiredError("age");
  }

  if (!user.name) {
    throw new PropertyRequiredError("name");
  }
}

function readUser(json) {
  let user;

  try {
    user = JSON.parse(json);
  } catch (err) {
    if (err instanceof SyntaxError) {
      throw new ReadError("Syntax Error", err);
    } else {
      throw err;
    }
  }

  try {
    validateUser(user);
  } catch (err) {
    if (err instanceof ValidationError) {
      throw new ReadError("Validation Error", err);
    } else {
      throw err;
    }
  }

}

try {
  readUser('{bad json}');
} catch (e) {
  if (e instanceof ReadError) {
    alert(e);
    // Original error: SyntaxError: Unexpected token b in JSON at position 1
    alert("Original error: " + e.cause);
  } else {
    throw e;
  }
}
```

Trong đoạn mã trên, `readUser` hoạt động chính xác như được mô tả -- catches các syntax and validation errors và throws `ReadError` errors (unknown errors được rethrown như thường lệ).

Vì vậy, mã bên ngoài kiểm tra `instanceof ReadError` và đó là nó. Không cần phải liệt kê tất cả các loại lỗi.

Cách tiếp cận này được gọi là "wrapping exceptions", bởi vì chúng ta lấy "ngoại lệ cấp thấp (low level exceptions)" và "bọc (wrap)" chúng thành `ReadError`, nó trừu tượng hơn và thuận tiện hơn để sử dụng cho mã gọi. Nó được sử dụng rộng rãi trong lập trình hướng đối tượng.

## Tóm lược

- Chúng ta có thể kế thừa từ `Error` và các built-in error classes khác một cách bình thường, chỉ cần chăm sóc thuộc tính `name` và đừng quên gọi `super`.
- Hầu hết thời gian, chúng ta nên sử dụng `instanceof` để kiểm tra các lỗi cụ thể. Nó cũng hoạt động với sự kế thừa. Nhưng đôi khi chúng ta có một error object đến từ thư viện của bên thứ 3 và không có cách nào dễ dàng để có được class. Sau đó, thuộc tính `name` có thể được sử dụng cho các kiểm tra như vậy.
- Bao bọc các ngoại lệ (Wrapping exceptions) là một kỹ thuật phổ biến khi một hàm xử lý các ngoại lệ cấp thấp (low-level exceptions) và tạo một đối tượng cấp cao hơn ( higher-level object) để báo cáo về các lỗi. Các ngoại lệ cấp thấp (Low-level exceptions) đôi khi trở thành các thuộc tính của đối tượng đó như `err.cause` trong các ví dụ ở trên, nhưng điều đó không bắt buộc.
