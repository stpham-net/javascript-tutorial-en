# Data types

Một biến trong JavaScript có thể chứa bất kỳ dữ liệu nào. Một biến có thể tại một thời điểm là một chuỗi và sau đó nhận được một giá trị số:

```js
      // no error
      let message = "hello";
      message = 123456;
```

Các ngôn ngữ lập trình cho phép những thứ như vậy được gọi là "kiểu động (dynamically typed)", nghĩa là có các kiểu dữ liệu, nhưng các biến không bị ràng buộc với bất kỳ trong số chúng.

Có bảy kiểu dữ liệu cơ bản trong JavaScript. Ở đây chúng ta sẽ nghiên cứu những điều cơ bản, và trong các chương tiếp theo, chúng ta sẽ nói chi tiết về từng điều đó.

## A number

```js
      let n = 123;
      n = 12.345;
```

Kiểu *number* phục vụ cả cho số nguyên và số thập phân.

Có nhiều phép toán cho các số, ví dụ như phép nhân `*`, phép chia `/`, phép cộng `+`, phép trừ `-`, v.v.

Bên cạnh các số thông thường, còn có cái gọi là "giá trị số đặc biệt" cũng thuộc loại đó: `Infinity`,` -Infinity` và `NaN`.

- `Infinity` đại diện cho toán học [Vô cực](https://en.wikipedia.org/wiki/Infinity). Đó là một giá trị đặc biệt lớn hơn bất kỳ số nào.

    Chúng ta có thể lấy nó là kết quả của phép chia cho số 0:

    ```js
    alert( 1 / 0 ); // Infinity
    ```

    Hoặc chỉ cần đề cập đến nó trong mã trực tiếp:

    ```js
    alert( Infinity ); // Infinity
    ```

- `NaN` đại diện cho một lỗi tính toán. Ví dụ, đó là kết quả của một phép toán không chính xác hoặc không xác định:

    ```js
    alert( "not a number" / 2 ); // NaN, such division is erroneous
    ```

    `NaN` là dính (sticky). Bất kỳ hoạt động nào nữa trên `NaN` sẽ cung cấp `NaN`:

    ```js
    alert( "not a number" / 2 + 5 ); // NaN
    ```

    Vì vậy, nếu có `NaN` ở đâu đó trong một biểu thức toán học, nó sẽ lan truyền đến toàn bộ kết quả.

<br>

> ---

**📌 Các phép toán đều an toàn**

Làm toán là an toàn trong JavaScript. Chúng ta có thể làm bất cứ điều gì: chia cho số 0, coi các chuỗi không phải là số, v.v.

Kịch bản sẽ không bao giờ dừng lại với một lỗi nghiêm trọng ("chết"). Tệ nhất là chúng ta sẽ nhận được `NaN`.

> ---

<br>

Các giá trị số đặc biệt đều thuộc về kiểu "number". Tất nhiên chúng không phải là số theo nghĩa thông thường của từ này.

Chúng ta sẽ thấy nhiều hơn khi làm việc với các số trong chương `number`.

## Một chuỗi

Một chuỗi trong JavaScript phải được trích dẫn (quoted).

```js
      let str = "Hello";
      let str2 = 'Single quotes are ok too';
      let phrase = `can embed ${str}`;
```

Trong JavaScript, có 3 kiểu trích dẫn (quotes).

1. Dấu ngoặc kép: `"Hello"`.
2. Dấu ngoặc đơn: `'Hello'`.
3. Backticks: <code>&#96;Hello&#96;</code>.

Dấu ngoặc đôi và đơn là dấu ngoặc "đơn giản". Không có sự khác biệt giữa chúng trong JavaScript.

Backticks là trích dẫn "chức năng mở rộng (extended functionality)". Chúng cho phép chúng ta nhúng các biến và biểu thức vào một chuỗi bằng cách gói chúng trong `${…}`, chẳng hạn:

```js
      let name = "John";

      // embed a variable
      alert( `Hello, ${name}` ); // Hello, John!

      // embed an expression
      alert( `the result is ${1 + 2}` ); // the result is 3
```

Biểu thức bên trong `${…}` được đánh giá (evaluated) và kết quả trở thành một phần của chuỗi. Chúng ta có thể đặt bất cứ thứ gì ở đó: một biến như `name` hoặc một biểu thức số học như `1 + 2` hoặc một cái gì đó phức tạp hơn.

Xin lưu ý rằng điều này chỉ có thể được thực hiện trong backticks. Quotes khác không cho phép nhúng như vậy!

```js
      alert( "the result is ${1 + 2}" ); // the result is ${1 + 2} (double quotes do nothing)
```

Chúng tôi sẽ đề cập đến các chuỗi kỹ lưỡng hơn trong chương `string`.

<br>

> ---

**📌  Không có kiểu *character*.**

Trong một số ngôn ngữ, có một loại "character" đặc biệt cho một ký tự (character). Ví dụ, trong ngôn ngữ C và trong Java, đó là `char`.

Trong JavaScript, không có loại đó. Chỉ có một loại: `string`. Một chuỗi có thể chỉ bao gồm một ký tự hoặc nhiều ký tự.

> ---

<br>

## A boolean (logical type)

Kiểu boolean chỉ có hai giá trị: `true` và `false`.

Loại này thường được sử dụng để lưu trữ các giá trị yes/no: `true` có nghĩa là "có, đúng" và `false` có nghĩa là "không, không chính xác".

Ví dụ:

```js
      let nameFieldChecked = true; // yes, name field is checked
      let ageFieldChecked = false; // no, age field is not checked
```

Các giá trị Boolean cũng là kết quả của sự so sánh:

```js
      let isGreater = 4 > 1;

      alert( isGreater ); // true (the comparison result is "yes")
```

Chúng ta sẽ đề cập đến các booleans sâu hơn về sau trong chương `logical-operators`.

## Giá trị "null"

Giá trị `null` đặc biệt không thuộc về bất kỳ loại nào được mô tả ở trên.

Nó tạo thành một kiểu riêng của nó, chỉ chứa giá trị `null`:

```js
      let age = null;
```

Trong JavaScript `null` không phải là "tham chiếu đến một đối tượng không tồn tại" hoặc "con trỏ null (null pointer)" như trong một số ngôn ngữ khác.

Nó chỉ là một giá trị đặc biệt có ý nghĩa "không có gì", "trống rỗng" hoặc "không biết giá trị".

Đoạn mã trên nói rằng `age` không rõ hoặc trống vì một số lý do.

## Giá trị "undefined"

Giá trị đặc biệt `undefined` đứng ngoài. Nó tạo ra một kiểu của riêng nó, giống như `null`.

Ý nghĩa của 'undefined` là "giá trị không được gán (assigned)".

Nếu một biến được khai báo, nhưng không được gán, thì giá trị của nó chính xác là `undefined`:

```js
      let x;

      alert(x); // shows "undefined"
```

Về mặt kỹ thuật, có thể gán `undefined` cho bất kỳ biến nào:

```js
      let x = 123;

      x = undefined;

      alert(x); // "undefined"
```

...Nhưng nó không được khuyến khích để làm điều đó. Thông thường, chúng tôi sử dụng `null` để viết giá trị "empty" hoặc "unknown" vào biến và `undefined` chỉ được sử dụng để kiểm tra, để xem biến đó được gán hay chưa?

## Objects và Symbols

Kiểu `object` là đặc biệt.

Tất cả các loại khác được gọi là "nguyên thủy", bởi vì các giá trị của chúng chỉ có thể chứa một thứ duy nhất (có thể là một chuỗi hoặc một số hoặc bất cứ thứ gì). Ngược lại, các đối tượng được sử dụng để lưu trữ các bộ sưu tập dữ liệu và các thực thể phức tạp hơn. Chúng ta sẽ giải quyết chúng sau này trong chương `object` sau khi chúng ta biết đủ về nguyên thủy.

Kiểu `Symbol` được sử dụng để tạo các định danh độc nhất (unique identifiers) cho các objects. Chúng ta đề cập một ít đến nó ở đây, nhưng tốt hơn là nghiên cứu chúng sau khi các nghiên cứu đối tượng.

## Toán tử typeof

Toán tử `typeof` trả về kiểu của đối số. Nó hữu ích khi chúng ta muốn xử lý các giá trị của các loại khác nhau hoặc chỉ muốn kiểm tra nhanh.

Nó hỗ trợ hai dạng cú pháp:

1. Là một toán tử: `typeof x`.
2. Kiểu hàm: `typeof (x)`.

Nói cách khác, nó hoạt động cả với dấu ngoặc đơn hoặc không có chúng. Kết quả là như nhau.

Cuộc gọi tới `typeof x` trả về một chuỗi có tên kiểu:

```js
      typeof undefined // "undefined"

      typeof 0 // "number"

      typeof true // "boolean"

      typeof "foo" // "string"

      typeof Symbol("id") // "symbol"

      typeof Math // "object"  (1)

      typeof null // "object"  (2)

      typeof alert // "function"  (3)
```

Ba dòng cuối có thể cần giải thích thêm:

1. `Math` là một đối tượng tích hợp cung cấp các phép toán. Chúng ta sẽ học nó trong chương `number`. Ở đây nó phục vụ như là một ví dụ về một đối tượng.
2. Kết quả của `typeof null` là `"object"`. Sai rồi. Đó là một lỗi được công nhận chính thức trong `typeof`, được giữ cho tương thích. Tất nhiên, `null` không phải là một đối tượng. Đó là một giá trị đặc biệt với một loại riêng của nó. Vì vậy, một lần nữa, đó là một lỗi trong ngôn ngữ JavaScript.
3. Kết quả của `typeof alert` là `"function"`, bởi vì `alert` là một function của ngôn ngữ JS. Chúng ta sẽ nghiên cứu các functions trong các chương tiếp theo và chúng ta sẽ thấy rằng không có loại "function" đặc biệt nào trong ngôn ngữ. Các function thuộc về kiểu object. Nhưng `typeof` phân biệt chúng khác nhau. Chính thức, nó không chính xác, nhưng rất thuận tiện trong thực tế.

## Tóm lược

Có 7 loại cơ bản trong JavaScript.

- `number` cho các số thuộc bất kỳ loại nào: số nguyên hoặc số thập phân.
- `string` cho chuỗi. Một chuỗi có thể có một hoặc nhiều ký tự, không có loại ký tự đơn riêng biệt.
- `boolean` cho `true`/`false`.
- `null` cho các giá trị không xác định - một loại độc lập có một giá trị duy nhất `null`.
- `undefined` cho các giá trị chưa được gán - một loại độc lập có một giá trị duy nhất `undefined`.
- `object` cho các cấu trúc dữ liệu phức tạp hơn.
- `Symbol` cho các định danh độc nhất.

Toán tử `typeof` cho phép chúng ta xem loại nào được lưu trữ trong biến.

- Hai dạng: `typeof x` hoặc `typeof(x)`.
- Trả về một chuỗi có tên của kiểu, như `"string"`.
- Với `null` trả về `"object"` - đó là một lỗi trong ngôn ngữ JS, thực tế nó không phải là một đối tượng.

Trong các chương tiếp theo, chúng ta sẽ tập trung vào các giá trị nguyên thủy và một khi chúng ta đã quen thuộc với chúng, thì chúng ta sẽ chuyển sang các đối tượng.
No search results.
