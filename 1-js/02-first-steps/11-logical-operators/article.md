# Toán tử logic

Có ba toán tử logic trong JavaScript: `||` (OR), `&&` (AND), `!` (NOT).

Mặc dù chúng được gọi là "logic", chúng có thể được áp dụng cho các giá trị thuộc bất kỳ loại nào, không chỉ boolean. Kết quả cũng có thể là bất kỳ loại nào.

Chúng ta hãy xem chi tiết.

## || (OR)

Toán tử "OR" được biểu thị bằng hai ký hiệu đường thẳng đứng:

```js
      result = a || b;
```

Trong lập trình cổ điển, logic OR có nghĩa là chỉ thao tác các giá trị boolean. Nếu bất kỳ đối số nào của nó là `true`, thì nó trả về` true`, nếu không, nó sẽ trả về `false`.

Trong JavaScript, toán tử phức tạp hơn và mạnh hơn một chút. Nhưng trước tiên hãy xem điều gì xảy ra với các giá trị boolean.

Có bốn kết hợp logic có thể:

```js
      alert( true || true );   // true
      alert( false || true );  // true
      alert( true || false );  // true
      alert( false || false ); // false
```

Như chúng ta có thể thấy, kết quả luôn là `true` ngoại trừ trường hợp khi cả hai toán hạng đều là `false`.

Nếu một toán hạng không phải là boolean, thì nó được chuyển đổi thành boolean để đánh giá.

Chẳng hạn, một số `1` được coi là` true`, một số `0` - là `false`:

```js
      if (1 || 0) { // works just like if( true || false )
        alert( 'truthy!' );
      }
```

Hầu hết thời gian, OR `||` được sử dụng trong câu lệnh `if` để kiểm tra xem *bất kì* các điều kiện đã cho có đúng không.

Ví dụ:

```js
      let hour = 9;

      if (hour < 10 || hour > 18) {
        alert( 'The office is closed.' );
      }
```

Chúng ta có thể vượt qua nhiều điều kiện hơn:

```js
      let hour = 12;
      let isWeekend = true;

      if (hour < 10 || hour > 18 || isWeekend) {
        alert( 'The office is closed.' ); // it is the weekend
      }
```

## OR tìm kiếm giá trị trung thực đầu tiên

Logic được mô tả ở trên là hơi cổ điển. Bây giờ, hãy mang các tính năng "bổ sung" của JavaScript.

Thuật toán mở rộng hoạt động như sau.

Cho nhiều giá trị OR'ed:

```js
      result = value1 || value2 || value3;
```

Toán tử OR `||` thực hiện như sau:

- Đánh giá toán hạng từ trái sang phải.
- Đối với mỗi toán hạng, chuyển đổi nó thành boolean. Nếu kết quả là `true`, thì dừng và trả về giá trị ban đầu của toán hạng đó.
- Nếu tất cả các toán hạng khác đã được đánh giá (tức là tất cả đều là `false`), hãy trả về toán hạng cuối cùng.

Một giá trị được trả về ở dạng ban đầu, không có chuyển đổi.

Nói cách khác, một chuỗi OR `"||"` trả về giá trị trung thực đầu tiên hoặc giá trị cuối cùng nếu không tìm thấy giá trị như vậy.

Ví dụ:

```js
      alert( 1 || 0 ); // 1 (1 is truthy)
      alert( true || 'no matter what' ); // (true is truthy)

      alert( null || 1 ); // 1 (1 is the first truthy value)
      alert( null || 0 || 1 ); // 1 (the first truthy value)
      alert( undefined || null || 0 ); // 0 (all falsy, returns the last value)
```

Điều đó dẫn đến một số cách sử dụng thú vị so với "pure, classical, boolean-only OR".

1. **Lấy giá trị trung thực đầu tiên từ danh sách các biến hoặc biểu thức.**

    Hãy tưởng tượng chúng ta có một số biến, có thể chứa dữ liệu hoặc là `null/undefined`. Và chúng ta cần chọn cái đầu tiên có dữ liệu.

    Chúng ta có thể sử dụng OR `||` cho điều đó:

    ```js
    let currentUser = null;
    let defaultUser = "John";

    let name = currentUser || defaultUser || "unnamed";

    alert( name ); // selects "John" – the first truthy value
    ```

    Nếu cả `currentUser` và `defaultUser` đều sai lệch thì `"unnamed"` sẽ là kết quả.
    
2. **Đánh giá ngắn mạch (Short-circuit evaluation).**

    Các toán tử có thể không chỉ là các giá trị, mà là các biểu thức tùy ý. OR đánh giá và kiểm tra chúng từ trái sang phải. Việc đánh giá dừng lại khi đạt được giá trị trung thực và giá trị được trả về. Quá trình này được gọi là "đánh giá ngắn mạch", bởi vì nó diễn ra càng ngắn càng tốt từ trái sang phải.

    Điều này được thấy rõ khi biểu thức được đưa ra làm đối số thứ hai có tác dụng phụ. Giống như một bài tập biến.

    Nếu chúng ta chạy ví dụ dưới đây, `x` sẽ không được chỉ định:

    ```js
    let x;

    true || (x = 1);

    alert(x); // undefined, because (x = 1) not evaluated
    ```

    ...Và nếu đối số đầu tiên là `false`, thì `OR` tiếp tục và đánh giá đối số thứ hai:

    ```js
    let x;

    false || (x = 1);

    alert(x); // 1
    ```

    Một phép gán là một trường hợp đơn giản, các tác dụng phụ khác có thể được tham gia.

    Như chúng ta có thể thấy, trường hợp sử dụng như vậy là "cách ngắn hơn để làm `if`". Toán hạng thứ nhất được chuyển đổi thành boolean và nếu nó sai thì toán hạng thứ hai được ước tính.

    Hầu hết thời gian tốt hơn là sử dụng một "thông thường" `if` để giữ cho mã dễ hiểu, nhưng đôi khi điều đó có thể hữu ích.

## && (AND)

Toán tử AND được biểu diễn bằng hai ký hiệu `&&`:

```js
      result = a && b;
```

Trong lập trình cổ điển AND trả về `true` nếu cả hai toán hạng là true và `false` nếu không:

```js
      alert( true && true );   // true
      alert( false && true );  // false
      alert( true && false );  // false
      alert( false && false ); // false
```

Một ví dụ với `if`:

```js
      let hour = 12;
      let minute = 30;

      if (hour == 12 && minute == 30) {
        alert( 'Time is 12:30' );
      }
```

Cũng như đối với OR, mọi giá trị đều được phép dưới dạng toán hạng AND:

```js
      if (1 && 0) { // evaluated as true && false
        alert( "won't work, because the result is falsy" );
      }
```


## AND tìm kiếm giá trị giả mạo đầu tiên

Cho nhiều giá trị AND'ed:

```js
      result = value1 && value2 && value3;
```

Toán tử AND `&&` thực hiện như sau:

- Đánh giá toán hạng từ trái sang phải.
- Đối với mỗi toán hạng, chuyển đổi nó thành boolean. Nếu kết quả là `false`, dừng và trả về giá trị ban đầu của toán hạng đó.
- Nếu tất cả các toán hạng khác đã được đánh giá (tức là tất cả đều là sự thật), hãy trả về toán hạng cuối cùng.

Nói cách khác, AND trả về giá trị giả đầu tiên hoặc giá trị cuối cùng nếu không tìm thấy.

Các quy tắc trên tương tự như OR. Sự khác biệt là AND trả về giá trị *giả (falsy)* đầu tiên trong khi OR trả về giá trị *thật (truthy)* đầu tiên.

Ví dụ:

```js
      // if the first operand is truthy,
      // AND returns the second operand:
      alert( 1 && 0 ); // 0
      alert( 1 && 5 ); // 5

      // if the first operand is falsy,
      // AND returns it. The second operand is ignored
      alert( null && 5 ); // null
      alert( 0 && "no matter what" ); // 0
```

Chúng ta cũng có thể vượt qua một số giá trị liên tiếp. Xem làm thế nào một sai lầm đầu tiên được trả lại:

```js
      alert( 1 && 2 && null && 3 ); // null
```

Khi tất cả các giá trị là trung thực, giá trị cuối cùng được trả về:

```js
      alert( 1 && 2 && 3 ); // 3, the last one
```

<br>

> ---

**📌 Ưu tiên của AND `&&` cao hơn OR `||`**

Ưu tiên của toán tử AND `&&` cao hơn OR `||`.

Vì vậy, mã `a && b || c && d` về cơ bản giống như khi `&&` nằm trong ngoặc đơn: `(a && b) || (c && d)`.

> ---

<br>

Cũng giống như OR, toán tử AND `&&` đôi khi có thể thay thế `if`.

Ví dụ:

```js
      let x = 1;

      (x > 0) && alert( 'Greater than zero!' );
```

Hành động trong phần bên phải của `&&` sẽ chỉ thực hiện nếu đánh giá đạt được. Đó là: chỉ khi `(x > 0)` là đúng.

Vì vậy, về cơ bản chúng ta có một tương tự cho:

```js
      let x = 1;

      if (x > 0) {
        alert( 'Greater than zero!' );
      }
```

Biến thể với `&&` dường như ngắn hơn. Nhưng 'if` rõ ràng hơn và có xu hướng dễ đọc hơn một chút.

Vì vậy, nên sử dụng mọi cấu trúc cho mục đích của nó. Sử dụng `if` nếu chúng ta muốn if. Và sử dụng `&&` nếu chúng ta muốn AND.

##! (NOT)

Toán tử boolean NOT được biểu thị bằng dấu chấm than `!`.

Cú pháp khá đơn giản:

```js
      result = !value;
```

Toán tử chấp nhận một đối số duy nhất và thực hiện như sau:

1. Chuyển đổi toán hạng thành kiểu boolean: `true/false`.
2. Trả về một giá trị nghịch đảo.

Ví dụ:

```js
      alert( !true ); // false
      alert( !0 ); // true
```

Một double NOT `!!` đôi khi được sử dụng để chuyển đổi một giá trị thành kiểu boolean:

```js
      alert( !!"non-empty string" ); // true
      alert( !!null ); // false
```

Nghĩa là, NOT đầu tiên chuyển đổi giá trị thành boolean và trả về nghịch đảo, và NOT thứ hai đảo ngược nó một lần nữa. Cuối cùng, chúng ta có một chuyển đổi giá trị thành boolean đơn giản.

Có một cách dài dòng hơn để làm điều tương tự -- một hàm `Boolean` tích hợp:

```js
      alert( Boolean("non-empty string") ); // true
      alert( Boolean(null) ); // false
```

Ưu tiên của NOT `!` là mức cao nhất trong tất cả các toán tử logic, vì vậy nó luôn luôn thực thi trước, trước bất kỳ `&&`, `||`.
