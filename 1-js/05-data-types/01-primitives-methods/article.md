# Methods of primitives

JavaScript cho phép chúng ta làm việc với các nguyên thủy (chuỗi, số, v.v.) như thể chúng là các đối tượng.

Họ cũng cung cấp các phương thức để gọi như vậy. Chúng ta sẽ nghiên cứu chúng sớm, nhưng trước tiên chúng ta sẽ xem nó hoạt động như thế nào, tất nhiên, nguyên thủy không phải là đối tượng (và ở đây chúng ta sẽ làm cho nó rõ ràng hơn nữa).

Chúng ta hãy xem xét sự khác biệt chính giữa nguyên thủy và đối tượng.

Một nguyên thủy

- Là một giá trị của một kiểu nguyên thủy.
- Có 6 kiểu nguyên thủy: `string`, `number`, `boolean`, `symbol`, `null` và `undefined`.

Một đối tượng

- Có khả năng lưu trữ nhiều giá trị như các thuộc tính.
- Có thể được tạo bằng `{}`, ví dụ: `{name: "John", age: 30}`. Có các loại đối tượng khác trong JavaScript; functions, để ví dụ, là các đối tượng.

Một trong những điều tốt nhất về các đối tượng là chúng ta có thể lưu trữ một hàm như một trong các thuộc tính của nó.

```js
      let john = {
        name: "John",
        sayHi: function() {
          alert("Hi buddy!");
        }
      };

      john.sayHi(); // Hi buddy!
```

Vì vậy, ở đây chúng ta đã tạo ra một đối tượng `john` với phương thức `sayHi`.

Nhiều built-in objects đã tồn tại, chẳng hạn như các đối tượng hoạt động với dates, errors, HTML elements, etc, v.v. Chúng có các thuộc tính và phương thức khác nhau.

Nhưng, các tính năng này đi kèm với một chi phí!

Đối tượng "nặng" hơn so với nguyên thủy. Chúng yêu cầu các nguồn lực bổ sung để hỗ trợ các máy móc nội bộ (internal machinery). Nhưng vì các thuộc tính và phương thức rất hữu ích trong lập trình, các JavaScript engines cố gắng tối ưu hóa chúng để giảm bớt gánh nặng bổ sung.

## Một nguyên thủy như một đối tượng

Đây là nghịch lý mà người tạo ra JavaScript phải đối mặt:

- Có nhiều điều người ta muốn làm với một nguyên thủy như một chuỗi hoặc một số. Nó sẽ là tuyệt vời để truy cập chúng như là phương thức.
- Nguyên thủy phải nhanh và nhẹ nhất có thể.

Giải pháp có vẻ hơi khó xử, nhưng đây là:

1. Các nguyên thủy vẫn còn nguyên thủy. Một giá trị duy nhất, như mong muốn.
2. Ngôn ngữ cho phép truy cập vào các phương thức và thuộc tính của chuỗi, số, booleans và symbols.
3. Khi điều này xảy ra, một "trình bao bọc đối tượng (object wrapper)" đặc biệt được tạo ra cung cấp chức năng bổ sung và sau đó bị hủy.

Các "trình bao bọc đối tượng (object wrapper)" khác nhau đối với từng loại nguyên thủy và được gọi là: `String`,` Number`, `Boolean` và `Symbol`. Vì vậy, chúng cung cấp các bộ phương thức khác nhau.

Chẳng hạn, tồn tại một phương thức [str.toUpperCase()](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/String/toUpperCase) trả về một chuỗi viết hoa.

Đây là cách nó hoạt động:

```js
      let str = "Hello";

      alert( str.toUpperCase() ); // HELLO
```

Đơn giản phải không? Đây là những gì thực sự xảy ra trong `str.toUpperCase()`:

1. Chuỗi `str` là một nguyên thủy. Vì vậy, trong thời điểm truy cập thuộc tính của nó, một đối tượng đặc biệt được tạo ra biết giá trị của chuỗi và có các phương thức hữu ích, như `toUpperCase()`.
2. Phương thức đó chạy và trả về một chuỗi mới (được hiển thị bởi `alert`).
3. Đối tượng đặc biệt bị phá hủy, để lại `str` nguyên thủy.

Vì vậy, nguyên thủy có thể cung cấp các phương thức, nhưng chúng vẫn nhẹ.

JavaScript engine tối ưu hóa quá trình này. Nó thậm chí có thể bỏ qua việc tạo ra các đối tượng bổ sung. Nhưng nó vẫn phải tuân thủ các đặc điểm kỹ thuật và hành xử như thể nó tạo ra một đối tượng.

Ví dụ, một số có các phương thức của riêng nó, [toFixed(n)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number/toFixed) làm tròn số tới độ chính xác:

```js
      let n = 1.23456;

      alert( n.toFixed(2) ); // 1.23
```

Chúng ta sẽ thấy các phương thức cụ thể hơn trong các chương **number** và **string**.

<br>

> ---

**📌 Constructors `String/Number/Boolean` chỉ dành cho sử dụng nội bộ**

Một số ngôn ngữ như Java cho phép chúng ta tạo "wrapper objects" cho các nguyên thủy rõ ràng bằng cách sử dụng cú pháp như `new Number(1)` hoặc `new Boolean(false)`.

Trong JavaScript, điều đó cũng có thể vì lý do lịch sử, nhưng rất **không được khuyến khích**. Mọi thứ sẽ phát điên ở một vài nơi.

Ví dụ:

```js
      alert( typeof 1 ); // "number"

      alert( typeof new Number(1) ); // "object"!
```

Và bởi vì những gì tiếp theo, `zero`, là một đối tượng, cảnh báo sẽ hiển thị:

```js
      let zero = new Number(0);

      if (zero) { // zero is true, because it's an object
        alert( "zero is truthy?!?" );
      }
```

Mặt khác, sử dụng các hàm tương tự `String/Number/Boolean` mà không có `new` là một điều hoàn toàn lành mạnh và hữu ích. Họ chuyển đổi một giá trị thành loại tương ứng: thành một chuỗi, một số hoặc một boolean (nguyên thủy).

Ví dụ: điều này hoàn toàn hợp lệ:

```js
      let num = Number("123"); // convert a string to number
```

> ---

<br>
<br>

> ---

**📌 null/undefined không có các phương thức**

Các nguyên thủy đặc biệt `null` và `undefined` là các ngoại lệ. Chúng không có "wrapper objects" tương ứng và không cung cấp bất kì phương thức nào. Theo một nghĩa nào đó, chúng là "nguyên thủy nhất".

Cố gắng truy cập vào một thuộc tính có giá trị như vậy sẽ gây ra lỗi:

```js
      alert(null.test); // error
```

> ---

<br>

## Tóm lược

- Nguyên thủy ngoại trừ `null` và `undefined` cung cấp nhiều phương thức hữu ích. Chúng ta sẽ nghiên cứu chúng trong các chương sắp tới.
- Chính thức, các phương thức này hoạt động thông qua các đối tượng tạm thời, nhưng các JavaScript engines được điều chỉnh tốt để tối ưu hóa nội bộ đó, vì vậy chúng không tốn kém để gọi.

