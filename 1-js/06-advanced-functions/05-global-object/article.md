

# Global object

Khi JavaScript được tạo ra, có một ý tưởng về một "global object" cung cấp tất cả các global variables và functions. Nó đã được lên kế hoạch rằng nhiều in-browser scripts sẽ sử dụng single global object đó và chia sẻ các biến thông qua nó.

Kể từ đó, JavaScript đã phát triển rất nhiều và ý tưởng liên kết mã thông qua các global variables trở nên ít hấp dẫn hơn nhiều. Trong JavaScript hiện đại, khái niệm mô-đun đã diễn ra.

Nhưng global object vẫn còn trong đặc điểm kỹ thuật (specification).

Trong một trình duyệt, nó được đặt tên là "window", cho Node.JS nó là "global", đối với các môi trường khác, nó có thể có một tên khác.

Nó làm hai việc:

1. Cung cấp quyền truy cập vào các built-in functions và values, được xác định bởi đặc tả (specification) và environment.
    Chẳng hạn, chúng ta có thể gọi `alert` trực tiếp hoặc như một phương thức của `window`:

    ```js
    alert("Hello");

    // the same as
    window.alert("Hello");
    ```

    Điều tương tự áp dụng cho các built-ins khác. Ví dụ, chúng ta có thể sử dụng `window.Array` thay vì `Array`.

2. Cung cấp quyền truy cập vào global Function Declarations và `var` variables. Chúng ta có thể đọc và viết chúng bằng các thuộc tính của chúng, ví dụ:

    ```js
    var phrase = "Hello";

    function sayHi() {
      alert(phrase);
    }

    // can read from window
    alert( window.phrase ); // Hello (global var)
    alert( window.sayHi ); // function (global function declaration)

    // can write to window (creates a new global variable)
    window.test = 5;

    alert(test); // 5
    ```

...Nhưng global object không có các biến được khai báo bằng `let/const`!

```js
letuser = "John";
alert(user); // John

alert(window.user); // undefined, don't have let
alert("user" in window); // false
```

<br>

> ---

**📌 The global object không phải là global Environment Record**

Trong các phiên bản của ECMAScript trước ES-2015, không có biến `let/const`, chỉ có `var`. Và global object đã được sử dụng như một global Environment Record (các từ có một chút khác biệt, nhưng đó là ý chính).

Nhưng bắt đầu từ ES-2015, các thực thể này bị tách ra. Có một global Lexical Environment với Environment Record của nó. Và có một global object cung cấp *một số* các global variables.

Như một sự khác biệt thực tế, các biến `let/const` global là các thuộc tính rõ ràng của global Environment Record, nhưng chúng không tồn tại trong global object.

Đương nhiên, đó là vì ý tưởng về một global object như một cách để truy cập "tất cả mọi thứ toàn cầu" xuất phát từ thời cổ đại. Ngày nay không được coi là một điều tốt. Các tính năng ngôn ngữ hiện đại như `let/const` không kết bạn với nó, nhưng các tính năng cũ vẫn tương thích.

> ---

<br>

## Công dụng của "window"

Trong môi trường server-side như Node.JS, đối tượng `global` hiếm khi được sử dụng. Có lẽ sẽ công bằng khi nói "không bao giờ".

Mặc dù, trong trình duyệt `window` đôi khi được sử dụng.

Thông thường, nó không phải là một ý tưởng tốt để sử dụng nó, nhưng đây là một số ví dụ bạn có thể gặp.

1. Để truy cập chính xác global variable nếu hàm có local variable có cùng tên.

    ```js
    var user = "Global";

    function sayHi() {
      var user = "Local";

      alert(window.user); // Global
    }

    sayHi();
    ```

    Sử dụng như vậy là một cách giải quyết. Sẽ tốt hơn nếu đặt tên biến khác nhau, điều đó sẽ không yêu cầu viết mã theo cách này. Và xin lưu ý `"var"` trước `user`. Thủ thuật không hoạt động với các biến `let`.

2. Để kiểm tra xem một biến global nhất định hoặc một builtin có tồn tại không.

    Chẳng hạn, chúng ta muốn kiểm tra xem một global function `XMLHttpRequest` có tồn tại không.

    Chúng ta không thể viết `if (XMLHttpRequest)`, bởi vì nếu không có `XMLHttpRequest`, sẽ có một lỗi (biến không được xác định).

    Nhưng chúng ta có thể đọc nó từ `window.XMLHttpRequest`:

    ```js
    if (window.XMLHttpRequest) {
      alert('XMLHttpRequest exists!')
    }
    ```

    Nếu không có global function như vậy thì `window.XMLHttpRequest` chỉ là một non-existing object property. Đó là `undefined`, không có lỗi, vì vậy nó hoạt động.

    Chúng ta cũng có thể viết bài kiểm tra mà không có `window`:

    ```js
    if (typeof XMLHttpRequest == 'function') {
      /* is there a function XMLHttpRequest? */
    }
    ```

    Điều này không sử dụng `window`, nhưng (về mặt lý thuyết) ít đáng tin cậy hơn, bởi vì `typeof` có thể sử dụng XMLHttpRequest cục bộ và chúng ta muốn cái toàn cầu.

3. Để lấy biến từ cửa sổ bên phải. Đó có lẽ là trường hợp sử dụng hợp lệ nhất.

    Một trình duyệt có thể mở nhiều cửa sổ và tab. Một cửa sổ cũng có thể nhúng một cửa sổ khác vào `<iframe>`. Mỗi cửa sổ trình duyệt có `window` object và các global variables riêng. JavaScript cho phép các cửa sổ đến từ cùng một trang web (cùng giao thức, máy chủ, cổng) để truy cập các biến từ nhau.

    Việc sử dụng đó hơi vượt quá phạm vi của chúng ta bây giờ, nhưng nó sẽ giống như này:
    
    ```html
    <iframe src="/" id="iframe"></iframe>

    <script>
      alert( innerWidth ); // get innerWidth property of the current window (browser only)
      alert( Array ); // get Array of the current window (javascript core builtin)

      // when the iframe loads...
      iframe.onload = function() {
        // get width of the iframe window
        alert( iframe.contentWindow.innerWidth );
        // get the builtin Array from the iframe window
        alert( iframe.contentWindow.Array );
      };
    </script>
    ```

    Ở đây, hai alerts đầu tiên sử dụng cửa sổ hiện tại và hai alerts sau lấy các biến từ cửa sổ `iframe`. Có thể là bất kỳ biến nào nếu `iframe` xuất phát từ cùng một protocol/host/port.

## "this" và global object

Đôi khi, giá trị của `this` chính xác là global object. Điều đó hiếm khi được sử dụng, nhưng một số kịch bản dựa trên điều đó.

1. Trong trình duyệt, giá trị của `this` trong khu vực toàn cầu là `window`:

    ```js
    // outside of functions
    alert( this === window ); // true
    ```

    Các môi trường khác, không phải trình duyệt, có thể sử dụng giá trị khác cho `this` trong những trường hợp như vậy.

2. Khi một hàm với `this` được gọi trong non-strict mode, nó sẽ nhận được global object là `this`:

    ```js
    // not in strict mode (!)
    function f() {
      alert(this); // [object Window]
    }

    f(); // called without an object
    ```

    Theo đặc tả, `this` trong trường hợp này phải là global object, ngay cả trong các môi trường không có trình duyệt như Node.JS. Đó là khả năng tương thích với các tập lệnh cũ, trong chế độ nghiêm ngặt `this` sẽ là `undefined`.
