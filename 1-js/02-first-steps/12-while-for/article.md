# Vòng lặp: while và for

Chúng ta thường có nhu cầu thực hiện các hành động tương tự nhiều lần liên tiếp.

Ví dụ, khi chúng ta cần xuất hàng hóa từ danh sách này đến danh sách khác. Hoặc chỉ chạy cùng một mã cho mỗi số từ 1 đến 10.

*Loops* là một cách để lặp lại cùng một phần của mã nhiều lần.

## Vòng lặp "while"

Vòng lặp `while` có cú pháp sau:

```js
      while (condition) {
        // code
        // so-called "loop body"
      }
```

Trong khi `condition` là `true`, `code` từ thân vòng lặp được thực thi.

Chẳng hạn, vòng lặp bên dưới xuất ra `i` khi `i < 3`:

```js
      let i = 0;
      while (i < 3) { // shows 0, then 1, then 2
        alert( i );
        i++;
      }
```

Một thực thi đơn lẻ của thân vòng lặp được gọi là *một lặp (an iteration)*. Vòng lặp trong ví dụ trên tạo ra ba lần lặp.

Nếu không có `i++` trong ví dụ trên, vòng lặp sẽ lặp lại (theo lý thuyết) mãi mãi. Trong thực tế, trình duyệt cung cấp các cách để ngăn chặn các vòng lặp như vậy và đối với JavaScript phía máy chủ, chúng ta có thể giết tiến trình.

Bất kỳ biểu thức hoặc một biến có thể là một điều kiện vòng lặp, không chỉ là một so sánh. Chúng được đánh giá và chuyển đổi thành boolean bởi `while`.

Chẳng hạn, cách viết ngắn hơn `while (i != 0)` có thể là `while (i)`:

```js
      let i = 3;
      while (i) { // when i becomes 0, the condition becomes falsy, and the loop stops
        alert( i );
        i--;
      }
```
<br>

> ---

**📌 Không cần có dấu ngoặc cho thân vòng lặp một dòng**

Nếu thân vòng lặp có một câu lệnh đơn, chúng ta có thể bỏ qua dấu ngoặc `{…}`:

```js
      let i = 3;
      while (i) alert(i--);
```

> ---

<br>

## Vòng lặp "do.. while"

Kiểm tra điều kiện có thể được di chuyển *bên dưới* thân vòng lặp bằng cú pháp `do..while`:

```js
      do {
        // loop body
      } while (condition);
```

Vòng lặp trước tiên sẽ thực thi phần thân, sau đó kiểm tra điều kiện và, trong khi đó là đúng, thực thi nó nhiều lần.

Ví dụ:

```js
      let i = 0;
      do {
        alert( i );
        i++;
      } while (i < 3);
```

Dạng cú pháp này hiếm khi được sử dụng trừ khi bạn muốn phần thân của vòng lặp thực thi **ít nhất một lần** bất kể điều kiện là đúng. Thông thường, hình thức khác được ưu tiên: `while(…) {…}`.

## Vòng lặp "for"

Vòng lặp `for` là vòng lặp thường được sử dụng nhất.

Nó trông như thế này:

```js
      for (begin; condition; step) {
        // ... loop body ...
      }
```

Hãy tìm hiểu ý nghĩa của những phần này bằng ví dụ. Vòng lặp bên dưới chạy `alert (i)` cho `i` từ `0` cho đến (nhưng không bao gồm) `3`:

```js
      for (let i = 0; i < 3; i++) { // shows 0, then 1, then 2
        alert(i);
      }
```

Chúng ta hãy kiểm tra phần câu lệnh `for` theo từng phần:

| phần | | |
| ----------- | ------------ | ------------------------ -------------------------------------------------- - |
| bắt đầu | `i = 0` | Thực hiện một lần khi vào vòng lặp.                                      |
| điều kiện | `tôi <3` | Đã kiểm tra trước mỗi lần lặp lại, nếu thất bại thì vòng lặp dừng lại.              |
| bước | `i ++` | Thực hiện sau khi cơ thể trên mỗi lần lặp, nhưng trước khi kiểm tra điều kiện. |
| cơ thể | `cảnh báo (i)` | Chạy đi chạy lại trong khi điều kiện là sự thật |


Thuật toán vòng lặp chung hoạt động như thế này:

```
      Run begin
      → (if condition → run body and run step)
      → (if condition → run body and run step)
      → (if condition → run body and run step)
      → ...
```

Nếu bạn chưa quen với các vòng lặp, thì có lẽ nó sẽ hữu ích nếu bạn quay lại ví dụ và tái tạo cách nó chạy từng bước trên một tờ giấy.

Đây là những gì chính xác xảy ra trong trường hợp của chúng ta:

```js
      // for (let i = 0; i < 3; i++) alert(i)

      // run begin
      let i = 0
      // if condition → run body and run step
      if (i < 3) { alert(i); i++ }
      // if condition → run body and run step
      if (i < 3) { alert(i); i++ }
      // if condition → run body and run step
      if (i < 3) { alert(i); i++ }
      // ...finish, because now i == 3
```

<br>

> ---

**📌 Khai báo biến nội tuyến**

Ở đây biến "counter" `i` được khai báo ngay trong vòng lặp. Đó gọi là khai báo biến "nội tuyến". Các biến như vậy chỉ được nhìn thấy bên trong vòng lặp.

```js
      for (let i = 0; i < 3; i++) {
        alert(i); // 0, 1, 2
      }
      alert(i); // error, no such variable
```

Thay vì định nghĩa một biến, chúng ta có thể sử dụng một biến hiện có:

```js
      let i = 0;

      for (i = 0; i < 3; i++) { // use an existing variable
        alert(i); // 0, 1, 2
      }

      alert(i); // 3, visible, because declared outside of the loop
```

> ---

<br>

### Bỏ qua các phần

Bất kỳ phần nào của `for` đều có thể được bỏ qua.

Ví dụ, chúng ta có thể bỏ qua `begin` nếu chúng ta không cần làm gì ở vòng lặp bắt đầu.

Giống như ở đây:

```js
      let i = 0; // we have i already declared and assigned

      for (; i < 3; i++) { // no need for "begin"
        alert( i ); // 0, 1, 2
      }
```

Chúng ta cũng có thể loại bỏ phần `step`:

```js
      let i = 0;

      for (; i < 3;) {
        alert( i++ );
      }
```

Vòng lặp trở nên giống hệt với `while (i < 3)`.

Chúng ta thực sự có thể loại bỏ mọi thứ, do đó tạo ra một vòng lặp vô hạn:

```js
      for (;;) {
        // repeats without limits
      }
```

Xin lưu ý rằng hai dấu `;` của `for` phải có mặt, nếu không đó sẽ là một lỗi cú pháp.

## Phá vỡ vòng lặp (Breaking the loop)

Thông thường vòng lặp thoát khi điều kiện trở nên sai.

Nhưng chúng ta có thể bắt buộc thoát ra bất cứ lúc nào. Có một chỉ thị `break` đặc biệt cho điều đó.

Ví dụ: vòng lặp bên dưới yêu cầu người dùng cho một loạt các số, nhưng "breaks" khi không có số nào được nhập:

```js
      let sum = 0;

      while (true) {

        let value = +prompt("Enter a number", '');

        if (!value) break; // (*)

        sum += value;

      }
      alert( 'Sum: ' + sum );
```

Lệnh `break` được kích hoạt tại dòng `(*)` nếu người dùng nhập vào một dòng trống hoặc hủy bỏ đầu vào. Nó dừng vòng lặp ngay lập tức, chuyển điều khiển (control) đến dòng đầu tiên sau vòng lặp. Cụ thể, `alert`.

Sự kết hợp "vòng lặp vô hạn (infinite loop) + `break` khi cần thiết "rất phù hợp cho các tình huống khi điều kiện phải được kiểm tra không phải ở đầu/cuối vòng lặp, mà ở giữa, hoặc thậm chí ở một vài nơi trên body.

## Tiếp tục lặp lại lần tiếp theo

Lệnh `continue` là "phiên bản nhẹ hơn" của `break`. Nó không dừng toàn bộ vòng lặp. Thay vào đó, nó dừng vòng lặp hiện tại và buộc vòng lặp bắt đầu một vòng lặp mới (nếu điều kiện cho phép).

Chúng tôi có thể sử dụng nó nếu chúng tôi hoàn thành bước lặp hiện tại và muốn chuyển sang phần tiếp theo.

Vòng lặp bên dưới sử dụng `continue` để chỉ xuất các giá trị lẻ:

```js
      for (let i = 0; i < 10; i++) {

        // if true, skip the remaining part of the body
        if (i % 2 == 0) continue;

        alert(i); // 1, then 3, 5, 7, 9
      }
```

Đối với các giá trị chẵn của `i`, lệnh `continue` dừng thực thi body, chuyển điều khiển (control) sang lần lặp tiếp theo của `for` (với số tiếp theo). Vì vậy, `alert` chỉ được gọi cho các giá trị lẻ.

<br>

> ---

**📌 Lệnh 'continue' giúp giảm mức lồng nhau**

Một vòng lặp hiển thị các giá trị lẻ có thể trông như thế này:

```js
      for (let i = 0; i < 10; i++) {

        if (i % 2) {
          alert( i );
        }

      }
```

Từ quan điểm kỹ thuật, nó giống hệt với ví dụ trên. Chắc chắn, chúng ta chỉ cần bọc mã trong khối `if` thay vì `continue`.

Nhưng như một hiệu ứng phụ, chúng ta có thêm một mức lồng nhau (`alert` được gọi bên trong dấu ngoặc nhọn). Nếu mã bên trong `if` dài hơn một vài dòng, điều đó có thể làm giảm khả năng đọc tổng thể.

> ---

<br>
<br>

> ---

**📌 Không `break/continue` sang bên phải của '?'

Xin lưu ý rằng các cấu trúc cú pháp không phải là biểu thức có thể được sử dụng với toán tử ternary `?`. Cụ thể, các chỉ thị như 'break/continue` không được phép ở đó.

Ví dụ: nếu chúng tôi lấy mã này:

```js
      if (i > 5) {
        alert(i);
      } else {
        continue;
      }
```

...Và viết lại bằng dấu chấm hỏi:


```js
      (i > 5) ? alert(i) : continue; // continue not allowed here
```

...Rồi nó ngừng hoạt động. Mã như thế này sẽ đưa ra một lỗi cú pháp.

Đó chỉ là một lý do khác để không sử dụng toán tử dấu hỏi `?` thay vì `if`.

> ---

<br>

## Nhãn (Labels) cho break/continue

Đôi khi chúng ta cần thoát ra khỏi nhiều vòng lặp lồng nhau cùng một lúc.

Ví dụ, trong đoạn mã bên dưới, chúng tôi lặp lại `i` và `j` nhắc các tọa độ `(i, j)` từ `(0,0)` đến `(3,3)`:

```js
      for (let i = 0; i < 3; i++) {

        for (let j = 0; j < 3; j++) {

          let input = prompt(`Value at coords (${i},${j})`, '');

          // what if I want to exit from here to Done (below)?

        }
      }

      alert('Done!');
```

Chúng tôi cần một cách để dừng quá trình nếu người dùng hủy bỏ đầu vào.

`Break` thông thường sau `input` sẽ chỉ phá vỡ vòng lặp bên trong. Điều đó là không đủ. Labels đến để cứu.

Một *label* là mã định danh có dấu hai chấm trước vòng lặp:

```js
      labelName: for (...) {
        ...
      }
```

Câu lệnh `break <labelName>` trong vòng lặp thoát ra khỏi label.

Giống như ở đây:

```js
      outer: for (let i = 0; i < 3; i++) {

        for (let j = 0; j < 3; j++) {

          let input = prompt(`Value at coords (${i},${j})`, '');

          // if an empty string or canceled, then break out of both loops
          if (!input) break outer; // (*)

          // do something with the value...
        }
      }
      alert('Done!');
```

Trong đoạn mã trên, `break outer` nhìn lên trên cho label có tên `outer` và thoát ra khỏi vòng lặp đó.

Vì vậy, điều khiển đi thẳng từ `(*)` đến `alert('Done!')`.

Chúng ta cũng có thể di chuyển label lên một dòng riêng biệt:

```js
      outer:
      for (let i = 0; i < 3; i++) { ... }
```

Lệnh `continue` cũng có thể được sử dụng với label. Trong trường hợp này, việc thực thi nhảy tới lần lặp tiếp theo của vòng lặp được gắn label.

<br>

> ---

**📌 Labels không phải là "goto"**

Nhãn không cho phép chúng ta nhảy vào một nơi của mã tùy ý.

Ví dụ, không thể làm điều này:

```js
      break label;  // jumps to label? No.

      label: for (...)
```

Cuộc gọi đến `break/continue` chỉ có thể thực hiện được từ bên trong vòng lặp và nhãn phải ở đâu đó từ chỉ dẫn.

> ---

<br>

## Tóm lược

Chúng ta bao gồm 3 loại loop:

- `while` -- Điều kiện được kiểm tra trước mỗi lần lặp.
- `do..while` -- Điều kiện được kiểm tra sau mỗi lần lặp.
- `for (;;)` -- Điều kiện được kiểm tra trước mỗi lần lặp, cài đặt bổ sung có sẵn (additional settings available).

Để tạo một vòng lặp "vô hạn", thường sử dụng cấu trúc `while(true)`. Một vòng lặp như vậy, giống như bất kỳ vòng lặp nào khác, có thể được dừng lại bằng lệnh 'break`.

Nếu chúng ta không muốn làm tiếp bất cứ điều gì trên lần lặp hiện tại và muốn chuyển tiếp đến lần tiếp theo, thì lệnh 'continue` sẽ thực hiện điều đó.

`break/continue` hỗ trợ labels trước vòng lặp. Một label là cách duy nhất để `break/continue` thoát khỏi lồng và đi ra ngoài vòng lặp.
No search results.
