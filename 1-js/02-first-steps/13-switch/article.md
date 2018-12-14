# Câu lệnh "switch"

Một câu lệnh `switch` có thể thay thế nhiều kiểm tra `if`.

Nó đưa ra một cách mô tả hơn để so sánh một giá trị với nhiều biến thể.

## Cú pháp

`Switch` có một hoặc nhiều khối `case` và một default tùy chọn.

Nó trông như thế này:

```js
      switch(x) {
        case 'value1':  // if (x === 'value1')
          ...
          [break]

        case 'value2':  // if (x === 'value2')
          ...
          [break]

        default:
          ...
          [break]
      }
```

- Giá trị của `x` được kiểm tra với một đẳng thức nghiêm ngặt (strict equality) với giá trị từ `case` đầu tiên (nghĩa là `value1`) sau đó đến giá trị thứ hai (`value2`), v.v.
- Nếu tìm thấy đẳng thức, `switch` bắt đầu thực thi mã bắt đầu từ `case` tương ứng, cho đến khi `break` gần nhất (hoặc cho đến khi kết thúc `switch`).
- Nếu không có trường hợp nào khớp với mã thì `default` được thực thi (nếu nó tồn tại).

## Một ví dụ

Một ví dụ về `switch`:

```js
      let a = 2 + 2;

      switch (a) {
        case 3:
          alert( 'Too small' );
          break;
        case 4:
          alert( 'Exactly!' );
          break;
        case 5:
          alert( 'Too large' );
          break;
        default:
          alert( "I don't know such values" );
      }
```

Ở đây, `switch` bắt đầu so sánh `a` từ biến thể `case` đầu tiên là `3`. Trùng khớp thất bại.

Rồi `4`. Đó là một trùng khớp, vì vậy việc thực thi bắt đầu từ `case 4` cho đến khi `break` gần nhất.

**Nếu không có `break` thì việc thực thi tiếp tục với `case` tiếp theo mà không có bất kỳ kiểm tra nào.**

Một ví dụ không có `break`:

```js
      let a = 2 + 2;

      switch (a) {
        case 3:
          alert( 'Too small' );
        case 4:
          alert( 'Exactly!' );
        case 5:
          alert( 'Too big' );
        default:
          alert( "I don't know such values" );
      }
```

Trong ví dụ trên, chúng ta sẽ thấy thực thi tuần tự ba `alert`:

```js
      alert( 'Exactly!' );
      alert( 'Too big' );
      alert( "I don't know such values" );
```

<br>

> ---

**📌 Bất kỳ biểu thức nào cũng có thể là đối số `switch/case`**

Cả `switch` và `case` đều cho phép các biểu thức tùy ý.

Ví dụ:

```js
      let a = "1";
      let b = 0;

      switch (+a) {
        case b + 1:
          alert("this runs, because +a is 1, exactly equals b+1");
          break;
        default:
          alert("this doesn't run");
      }
```
Ở đây `+a` tạo ra `1`, được so sánh với `b + 1` trong `case` và mã tương ứng được thực thi.

> ---

<br>

## Phân nhóm "case"

Một số biến thể của `case` có chung mã có thể được nhóm lại.

Ví dụ, nếu chúng ta muốn cùng một mã chạy cho `case 3` và `case 5`:

```js
      let a = 2 + 2;

      switch (a) {
        case 4:
          alert('Right!');
          break;
        case 3:                    // (*) grouped two cases
        case 5:
          alert('Wrong!');
          alert("Why don't you take a math class?");
          break;
        default:
          alert('The result is strange. Really.');
      }
```

Bây giờ cả `3` và `5` đều hiển thị cùng một thông điệp.

Khả năng "nhóm" các trường hợp là một tác dụng phụ của cách `switch/case` hoạt động mà không có `break`. Ở đây, việc thực thi `case 3` bắt đầu từ dòng `(*)` và đi qua `case 5`, vì không có `break`.

## Các vấn đề về kiểu

Hãy nhấn mạnh rằng kiểm tra bằng nhau luôn luôn nghiêm ngặt. Các giá trị phải cùng loại để phù hợp.

Ví dụ, hãy xem xét mã:

```js
      let arg = prompt("Enter a value?")
      switch (arg) {
        case '0':
        case '1':
          alert( 'One or zero' );
          break;
        case '2':
          alert( 'Two' );
          break;
        case 3:
          alert( 'Never executes!' );
          break;
        default:
          alert( 'An unknown value' )
      }
```

1. Đối với `0`, `1`, `alert` đầu tiên chạy.
2. Đối với `2` thì `alert` thứ hai chạy.
3. Nhưng với `3`, kết quả của `prompt` là một chuỗi `"3"`, không hoàn toàn bằng `===` với số `3`. Vì vậy, chúng tôi đã có một mã chết (dead code) trong `trường hợp 3`! Biến thể `default` sẽ được thực thi.
