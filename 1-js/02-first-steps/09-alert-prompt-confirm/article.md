# Tương tác: alert, prompt, confirm

Phần này của hướng dẫn nhằm mục đích tổng quan JavaScript "nguyên bản", mà không có các điều chỉnh cụ thể về môi trường (environment-specific tweaks).

Nhưng chúng ta vẫn sử dụng một trình duyệt làm môi trường demo. Vì vậy, chúng ta nên biết ít nhất một vài chức năng giao diện người dùng. Trong chương này, chúng ta sẽ làm quen với các chức năng của trình duyệt `alert`, `prompt` và `confirm`.

## alert

Cú pháp:

```js
      alert(message);
```

Điều này hiển thị một message và tạm dừng thực thi tập lệnh cho đến khi người dùng nhấn "OK".

Ví dụ:

```js
      alert("Hello");
```

Cửa sổ nhỏ với thông báo được gọi là *modal window*. Từ "modal" có nghĩa là khách truy cập không thể tương tác với phần còn lại của trang, nhấn các nút khác, v.v., cho đến khi họ xử lý với cửa sổ này. Trong trường hợp này -- cho đến khi họ nhấn "OK".

## prompt

Hàm `prompt` chấp nhận hai đối số:

```js
      result = prompt(title[, default]);
```

Nó hiển thị một modal window với một message, một trường input cho khách truy cập và các nút OK/CANCEL.

**`title`** Văn bản để hiển thị cho khách truy cập.

**`default`** Một tham số thứ hai tùy chọn, giá trị ban đầu cho trường input.

Khách truy cập có thể nhập nội dung nào đó vào trường nhập nhanh chóng (prompt input) và nhấn OK. Hoặc họ có thể hủy bỏ input bằng cách nhấn nút CANCEL hoặc nhấn phím `Esc`.

Cuộc gọi đến `prompt` trả về văn bản từ trường input hoặc `null` nếu input bị hủy.

Ví dụ:

```js
      let age = prompt('How old are you?', 100);

      alert(`You are ${age} years old!`); // You are 100 years old!
```

<br>

> ---

***📌 IE: luôn cung cấp một `default`***

Tham số thứ hai là tùy chọn. Nhưng nếu chúng ta không cung cấp nó, Internet Explorer sẽ chèn văn bản `"undefined"` vào prompt.

Chạy mã này trong Internet Explorer để thấy rằng:

```js
      let test = prompt("Test");
```

Vì vậy, để trông tốt hơn trong IE, bạn nên luôn cung cấp đối số thứ hai:

```js
      let test = prompt("Test", ''); // <-- for IE
```

> ---

<br>

## confirm

Cú pháp:

```js
      result = confirm(question);
```

Hàm `confirm` hiển thị một modal window với một `question` và hai button: OK và CANCEL.

Kết quả là `true` nếu nhấn OK và `false` nếu không.

Ví dụ:

```js
      let isBoss = confirm("Are you the boss?");

      alert( isBoss ); // true if OK is pressed
```

## Tóm lược

Chúng ta bao gồm 3 chức năng dành riêng cho trình duyệt để tương tác với khách truy cập:

**`alert`** hiển thị một tin nhắn.

**`prompt`** hiển thị một message yêu cầu người dùng nhập văn bản. Nó trả về văn bản hoặc, nếu CANCEL hoặc `Esc` được nhấp, tất cả các trình duyệt trả về `null`.

**`confirm`** hiển thị một message và chờ người dùng nhấn "OK" hoặc "CANCEL". Nó trả về `true` cho OK và `false` cho CANCEL/`Esc`.

Tất cả các phương thức này là modal: chúng tạm dừng thực thi script và không cho phép khách truy cập tương tác với phần còn lại của trang cho đến khi message bị loại bỏ.

Có hai hạn chế được dùng chung bởi tất cả các phương pháp trên:

1. Vị trí chính xác của modal window được xác định bởi trình duyệt. Thông thường nó ở center.
2. Giao diện chính xác của cửa sổ cũng phụ thuộc vào trình duyệt. Chúng ta không thể sửa đổi nó.

Đó là cái giá cho sự đơn giản. Có nhiều cách khác để hiển thị các cửa sổ đẹp hơn và tương tác phong phú hơn với khách truy cập, nhưng nếu "chuông và còi" không quan trọng lắm, các phương pháp này hoạt động tốt.
