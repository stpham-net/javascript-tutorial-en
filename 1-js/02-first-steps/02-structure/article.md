# Cấu trúc mã

Điều đầu tiên để nghiên cứu là các khối xây dựng của mã.

## Các câu lệnh

Các câu lệnh là các cấu trúc cú pháp và các lệnh thực hiện các hành động.

Chúng ta đã thấy một câu lệnh `alert('Hello, world!')`, để hiển thị thông báo "Hello world!".

Chúng ta có thể có nhiều câu lệnh trong mã như chúng ta muốn. Một câu lệnh khác có thể được phân tách bằng dấu chấm phẩy.

Ví dụ: ở đây chúng ta chia tin nhắn thành hai:

```js
      alert('Hello'); alert('World');
```

Thông thường mỗi câu lệnh được viết trên một dòng riêng biệt - do đó mã trở nên dễ đọc hơn:

```js
      alert('Hello');
      alert('World');
```

## Dấu chấm phẩy

Dấu chấm phẩy có thể được bỏ qua trong hầu hết các trường hợp khi có ngắt dòng.

Điều này cũng sẽ làm việc:

```js
      alert('Hello')
      alert('World')
```

Ở đây, JavaScript diễn giải ngắt dòng là một dấu chấm phẩy "ngầm". Điều đó cũng được gọi là [chèn dấu chấm phẩy tự động](https://tc39.github.io/ecma262/#sec-automatic-semiaolon-inserts).

**Trong hầu hết các trường hợp, một dòng mới ngụ ý một dấu chấm phẩy. Nhưng "trong hầu hết các trường hợp" không có nghĩa là "luôn luôn"!**

Có những trường hợp khi một dòng mới không có nghĩa là dấu chấm phẩy, ví dụ:

```js
      alert(3 +
      1
      + 2);
```

Mã đầu ra sẽ là `6` vì JavaScript không chèn dấu chấm phẩy ở đây. Một cách trực quan rõ ràng là nếu dòng kết thúc bằng dấu cộng `"+"`, thì đó là một "biểu thức không đầy đủ", do đó không cần phải có dấu chấm phẩy. Và trong trường hợp này hoạt động như dự định.

**Nhưng có những tình huống JavaScript "thất bại" khi giả sử cần một dấu chấm phẩy ở nơi thực sự cần thiết.**

Lỗi xảy ra trong những trường hợp như vậy là khá khó để tìm và sửa.

<br/>

> ---

**📌  Một ví dụ về lỗi**

Nếu bạn tò mò muốn xem một ví dụ cụ thể về lỗi như vậy, hãy kiểm tra mã này:

```js
      [1, 2].forEach(alert)
```

Không cần phải suy nghĩ về ý nghĩa của dấu ngoặc `[]` và` forEach`. Chúng ta sẽ nghiên cứu chúng sau, bây giờ nó không thành vấn đề. Chúng ta hãy nhớ kết quả: nó hiển thị `1`, rồi `2`.

Bây giờ, hãy thêm một `alert` trước mã và *không* kết thúc nó bằng dấu chấm phẩy:

```js
      alert("There will be an error")

      [1, 2].forEach(alert)
```

Bây giờ nếu chúng ta chạy nó, chỉ có `alert` đầu tiên được hiển thị, và sau đó chúng ta gặp lỗi!

Nhưng mọi thứ sẽ ổn trở lại nếu chúng ta thêm dấu chấm phẩy sau `alert`:

```js
      alert("All fine now");

      [1, 2].forEach(alert)  
```

Bây giờ chúng ta có thông báo "All fine now" và sau đó là `1` và `2`.

Lỗi trong biến thể không có dấu chấm phẩy xảy ra do JavaScript không bao hàm dấu chấm phẩy trước dấu ngoặc vuông `[...]`.

Vậy, vì dấu chấm phẩy không được chèn tự động, mã trong ví dụ đầu tiên được coi là một câu lệnh. Đây là cách động cơ (engine) nhìn thấy nó:

```js
      alert("There will be an error")[1, 2].forEach(alert)
```

Nhưng nó phải là hai câu lệnh riêng biệt, không phải là một câu lệnh duy nhất. Việc hợp nhất như vậy trong trường hợp này là sai, do đó có lỗi. Có những tình huống tương tự khi điều như vậy xảy ra.

> ---

<br/>

Bạn nên đặt dấu chấm phẩy giữa các câu ngay cả khi chúng được phân tách bằng dòng mới. Quy tắc này được cộng đồng áp dụng rộng rãi. Chúng ta hãy lưu ý một lần nữa -- *có thể* bỏ qua dấu chấm phẩy hầu hết thời gian. Nhưng nó an toàn hơn - đặc biệt đối với người mới bắt đầu - sử dụng chúng.

## Comments

Thời gian trôi qua, chương trình ngày càng phức tạp hơn. Nó trở nên cần thiết để thêm *comments* mô tả những gì xảy ra và tại sao.

Comments có thể được đưa vào bất kỳ nơi nào của kịch bản. Chúng không ảnh hưởng đến việc thực thi vì động cơ (engine) đơn giản bỏ qua chúng.

**One-line comments bắt đầu bằng hai ký tự gạch chéo về phía trước `//`.**

Phần còn lại của dòng là một comment. Nó có thể chiếm một dòng đầy đủ của riêng mình hoặc theo sau một câu lệnh.

Giống như ở đây:

```js
      // This comment occupies a line of its own
      alert('Hello');

      alert('World'); // This comment follows the statement
```

** Nhận xét nhiều dòng bắt đầu bằng dấu gạch chéo và dấu hoa thị `/*` và kết thúc bằng dấu hoa thị và dấu gạch chéo `*/`.**

Như thế này:

```js
      /* An example with two messages.
      This is a multiline comment.
      */
      alert('Hello');
      alert('World');
```

Nội dung của các comment bị bỏ qua, vì vậy nếu chúng ta đặt mã bên trong `/* ... */` thì nó sẽ không được thực thi.

Đôi khi nó có ích để tạm thời vô hiệu hóa một phần mã:

```js
      /* Commenting out the code
      alert('Hello');
      */
      alert('World');
```

<br/>

> ---

**📌 Sử dụng phím nóng!**

Trong hầu hết các trình soạn thảo, một dòng mã có thể được nhận xét bằng `Ctrl+/`  cho một nhận xét một dòng và đại loại như `Ctrl+Shift+/` -- cho các nhận xét đa dòng (chọn một đoạn mã và nhấn phím nóng). Đối với Mac, hãy thử `Cmd` thay vì` Ctrl`.

> ---

<br>
<br>

> ---

**📌 Nhận xét lồng nhau không được hỗ trợ!**

Không thể có `/*...*/` bên trong một `/*...*/`.

Mã như vậy sẽ chết với một lỗi:

```js
      /*
        /* nested comment ?!? */
      */
      alert( 'World' );
```

> ---

<br>

Please, đừng ngần ngại comment code của bạn.

Nhận xét tăng dấu chân (footprint) mã tổng thể, nhưng đó không phải là một vấn đề. Có nhiều công cụ thu nhỏ (minify) mã trước khi xuất bản lên máy chủ sản xuất (production server). Công cụ sẽ xóa comments, vì vậy chúng không xuất hiện trong các kịch bản làm việc. Do đó, comments không có bất kỳ tác động tiêu cực nào đến sản xuất (production).

Hơn nữa trong hướng dẫn sẽ có một chương **Coding Style** cũng giải thích cách viết comments tốt hơn.
