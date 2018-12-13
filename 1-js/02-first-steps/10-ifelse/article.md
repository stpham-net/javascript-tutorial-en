# Toán tử có điều kiện: if, '?';

Đôi khi chúng ta cần thực hiện các hành động khác nhau dựa trên một điều kiện.

Có câu lệnh `if` dành cho điều đó và cũng là toán tử có điều kiện (ternary) để đánh giá có điều kiện mà chúng ta sẽ gọi là toán tử "dấu chấm hỏi" `?` cho đơn giản.

## Câu lệnh "if"

Câu lệnh `if` nhận được một điều kiện, đánh giá nó và, nếu kết quả là `true`, thực thi mã.

Ví dụ:

```js
      let year = prompt('In which year was ECMAScript-2015 specification published?', '');

      if (year == 2015) alert( 'You are right!' );
```

Trong ví dụ trên, điều kiện là kiểm tra bằng nhau đơn giản: `năm == 2015`, nhưng nó có thể phức tạp hơn nhiều.

Nếu có nhiều hơn một câu lệnh được thực thi, chúng ta phải bọc khối mã của mình bên trong dấu ngoặc nhọn:

```js
      if (year == 2015) {
        alert( "That's correct!" );
        alert( "You're so smart!" );
      }
```

Bạn nên bọc khối mã của mình bằng dấu ngoặc nhọn `{}` mỗi lần với `if`, ngay cả khi chỉ có một câu lệnh. Điều đó cải thiện khả năng đọc.

## Chuyển đổi Boolean

Câu lệnh `if (…)` đánh giá biểu thức trong ngoặc đơn và chuyển đổi nó thành kiểu boolean.

Chúng ta hãy nhớ lại các quy tắc chuyển đổi từ chương **chuyển đổi kiểu (type-conversions)**:

- Một số `0`, một chuỗi rỗng `""`, `null`, `không xác định` và `NaN` trở thành `false`. Do đó, chúng được gọi là giá trị "giả".
- Các giá trị khác trở thành 'true`, vì vậy chúng được gọi là "thật".

Vì vậy, mã theo điều kiện này sẽ không bao giờ thực thi:

```js
      if (0) { // 0 is falsy
        ...
      }
```

...Và bên trong điều kiện này -- luôn hoạt động:

```js
      if (1) { // 1 is truthy
        ...
      }
```

Chúng ta cũng có thể chuyển một giá trị boolean được đánh giá trước cho `if`, như ở đây:

```js
      let cond = (year == 2015); // equality evaluates to true or false

      if (cond) {
        ...
      }
```

## Mệnh đề "else"

Câu lệnh `if` có thể chứa một khối "else" tùy chọn. Nó thực thi khi điều kiện sai.

Ví dụ:

```js
      let year = prompt('In which year was ECMAScript-2015 specification published?', '');

      if (year == 2015) {
        alert( 'You guessed it right!' );
      } else {
        alert( 'How can you be so wrong?' ); // any value except 2015
      }
```

## Một số điều kiện: "else if"

Đôi khi chúng tôi muốn thử nghiệm một số biến thể của một điều kiện. Có một mệnh đề `else if` cho điều đó.

Ví dụ:

```js
      let year = prompt('In which year was ECMAScript-2015 specification published?', '');

      if (year < 2015) {
        alert( 'Too early...' );
      } else if (year > 2015) {
        alert( 'Too late' );
      } else {
        alert( 'Exactly!' );
      }
```

Trong đoạn mã JavaScript trên, đầu tiên kiểm tra `year < 2015`. Nếu nó là giả thì nó sẽ chuyển sang điều kiện tiếp theo `year > 2015`, và nếu không thì hiển thị `alert` cuối cùng.

Có thể có nhiều khối `else if`. Kết thúc `else` là tùy chọn.

## Toán tử ternary '?'

Đôi khi chúng ta cần gán một biến phụ thuộc vào một điều kiện.

Ví dụ:

```js
      let accessAllowed;
      let age = prompt('How old are you?', '');

      if (age > 18) {
        accessAllowed = true;
      } else {
        accessAllowed = false;
      }

      alert(accessAllowed);
```

Toán tử được gọi là "ternary" hoặc "dấu hỏi chấm" cho phép chúng ta làm điều đó ngắn hơn và đơn giản hơn.

Toán tử được biểu diễn bằng dấu chấm hỏi `?`.  Thuật ngữ chính thức "ternary" có nghĩa là toán tử có ba toán hạng. Nó thực sự là toán tử một và duy nhất trong JavaScript xuất hiện rất nhiều.

Cú pháp là:

```js
      let result = condition ? value1 : value2
```

`Điều kiện` được ước tính, nếu nó là sự thật thì `value1` được trả về, nếu không -- `value2`.

Ví dụ:

```js
      let accessAllowed = (age > 18) ? true : false;
```

Về mặt kỹ thuật, chúng ta có thể bỏ qua dấu ngoặc đơn trong khoảng `age > 18`. Toán tử dấu hỏi có độ ưu tiên thấp. Nó thực thi sau khi so sánh `>`, do đó sẽ làm tương tự:

```js
      // the comparison operator "age > 18" executes first anyway
      // (no need to wrap it into parentheses)
      let accessAllowed = age > 18 ? true : false;
```

Nhưng dấu ngoặc đơn làm cho mã dễ đọc hơn, vì vậy nên sử dụng chúng.

<br>

> ---

**📌 Xin lưu ý:**

Trong ví dụ trên, có thể tránh toán tử dấu hỏi, bởi vì chính phép so sánh trả về `true/false`:

```js
      // the same
      let accessAllowed = age > 18;
```

> ---

<br>

## Nhiều '?'

Một chuỗi các toán tử dấu hỏi `?` cho phép trả về một giá trị phụ thuộc vào nhiều điều kiện.

Ví dụ:

```js
      let age = prompt('age?', 18);

      let message = (age < 3) ? 'Hi, baby!' :
        (age < 18) ? 'Hello!' :
        (age < 100) ? 'Greetings!' :
        'What an unusual age!';

      alert( message );
```

Ban đầu có thể khó nắm bắt những gì đang diễn ra. Nhưng sau khi xem xét kỹ hơn chúng ta có thể thấy rằng đó chỉ là một chuỗi thử nghiệm thông thường.

1. Dấu hỏi đầu tiên kiểm tra xem `age < 3`.
2. Nếu đúng - trả về `'Hi, baby!'`, nếu không -- đi tiếp sau dấu hai chấm `":"` và kiểm tra `age < 18`.
3. Nếu đó là true -- trả về `'Hello!'`, nếu không -- đi tiếp sau dấu hai chấm tiếp theo `":"` và kiểm tra `age < 100`.
4. Nếu đó là true -- trả về `'Greetings!'`, nếu không -- đi tiếp sau dấu hai chấm cuối `":"` và trả về `'What an unusual age!'`.

Logic tương tự sử dụng `if..else`:

```js
      if (age < 3) {
        message = 'Hi, baby!';
      } else if (age < 18) {
        message = 'Hello!';
      } else if (age < 100) {
        message = 'Greetings!';
      } else {
        message = 'What an unusual age!';
      }
```

## Sử dụng phi truyền thống '?'

Đôi khi dấu hỏi `?` được sử dụng để thay thế cho `if`:

```js
      let company = prompt('Which company created JavaScript?', '');

      (company == 'Netscape') ?
         alert('Right!') : alert('Wrong.');
```

Tùy thuộc vào điều kiện `company == 'Netscape'`, phần đầu tiên hoặc phần thứ hai sau `?` được thực thi và hiển thị cảnh báo.

Chúng ta không chỉ định một kết quả cho một biến ở đây. Ý tưởng là để thực thi mã khác nhau tùy thuộc vào điều kiện.

**Không nên sử dụng toán tử dấu hỏi theo cách này.**

Ký hiệu dường như ngắn hơn `if`, điều này hấp dẫn một số lập trình viên. Nhưng nó khó đọc hơn.

Đây là cùng một mã với `if` để so sánh:

```js
      let company = prompt('Which company created JavaScript?', '');

      if (company == 'Netscape') {
        alert('Right!');
      } else {
        alert('Wrong.');
      }
```

Mắt của chúng ta quét mã theo chiều dọc. Các cấu trúc trải dài trên một số dòng dễ hiểu hơn một tập lệnh ngang dài.

Ý tưởng về một dấu hỏi `?` là trả về một hoặc một giá trị khác tùy theo điều kiện. Hãy sử dụng nó chính xác cho điều đó. Còn `if` để thực thi các nhánh khác nhau của mã.
No search results.
