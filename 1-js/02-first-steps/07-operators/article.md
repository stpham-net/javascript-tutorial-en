# Toán tử

Nhiều toán tử được biết đến với chúng ta từ trường học. Chúng là phép cộng `+`, phép nhân `*`, phép trừ `-`, v.v.

Trong chương này, chúng ta tập trung vào các khía cạnh không được bao gồm trong số học của trường học.

## Các điều kiện (Terms): "đơn phân (unary)", "nhị phân (binary)", "toán hạng (operand)"

Trước khi chúng ta tiếp tục, hãy nắm bắt các thuật ngữ phổ biến.

- *Một Toán hạng (An operand)* -- là cái mà toán tử được áp dụng cho. Ví dụ, trong phép nhân `5 * 2` có hai toán hạng: toán hạng bên trái là `5` và toán hạng bên phải là `2`. Đôi khi người ta nói "đối số" thay vì "toán hạng".
- Một toán tử là *đơn phân (unary)* nếu nó có một toán hạng đơn. Ví dụ: đơn phân phủ định (unary negation) `-` đảo ngược dấu của số:

    ```js
    let x = 1;

    x = -x;
    alert( x ); // -1, unary negation was applied
    ```
    
- Một toán tử là *nhị phân (binary)* nếu nó có hai toán hạng. Dấu trừ tương tự cũng tồn tại ở dạng nhị phân:

    ```js
    let x = 1, y = 3;
    alert( y - x ); // 2, binary minus subtracts values
    ```

    Chính thức, chúng ta đang nói về hai toán tử khác nhau ở đây: đơn phân phủ định (unary negation) (toán hạng đơn, đảo ngược dấu) và phép trừ nhị phân (hai toán hạng, phép trừ).

## Nối chuỗi, nhị phân + (Strings concatenation, binary +)

Bây giờ chúng ta hãy xem các tính năng đặc biệt của các toán tử JavaScript vượt ra ngoài các trường đại học.

Thông thường toán tử cộng `+` tổng các số.

Nhưng nếu nhị phân `+` được áp dụng cho các chuỗi, nó sẽ hợp nhất (merges) (nối) chúng:

```js
      let s = "my" + "string";
      alert(s); // mystring
```

Lưu ý rằng nếu bất kỳ toán hạng nào là một chuỗi, thì một toán hạng khác cũng được chuyển đổi thành một chuỗi.

Ví dụ:

```js
      alert( '1' + 2 ); // "12"
      alert( 2 + '1' ); // "21"
```

Xem, không quan trọng là toán hạng thứ nhất là chuỗi hay chuỗi thứ hai. Quy tắc rất đơn giản: nếu một trong hai toán hạng là một chuỗi, thì cũng chuyển đổi một chuỗi khác thành một chuỗi.

Tuy nhiên, lưu ý rằng các hoạt động chạy từ trái sang phải. Nếu có hai số được theo sau bởi một chuỗi, các số đó sẽ được thêm vào trước khi được chuyển đổi thành một chuỗi:


```js
      alert(2 + 2 + '1' ); // "41" and not "221"
```

Nối chuỗi và chuyển đổi chuỗi là một tính năng đặc biệt của nhị phân cộng `+`. Các toán tử số học khác chỉ làm việc với các số. Chúng luôn chuyển đổi toán hạng của chúng thành số.

Ví dụ, phép trừ và phép chia:

```js
      alert( 2 - '1' ); // 1
      alert( '6' / '2' ); // 3
```

## Chuyển đổi số, đơn phân + (Numeric conversion, unary +)

Dấu cộng `+` tồn tại ở hai dạng. Hình thức nhị phân mà chúng ta đã sử dụng ở trên và hình thức đơn phân.

Đơn phân cộng hay nói cách khác, toán tử cộng `+` được áp dụng cho một giá trị duy nhất, không làm gì với các số, nhưng nếu toán hạng không phải là một số, thì nó được chuyển đổi thành số.

Ví dụ:

```js
      // No effect on numbers
      let x = 1;
      alert( +x ); // 1

      let y = -2;
      alert( +y ); // -2

      // Converts non-numbers
      alert( +true ); // 1
      alert( +"" );   // 0
```

Nó thực sự hoạt động giống như `Number(...)`, nhưng ngắn hơn.

Một nhu cầu chuyển đổi chuỗi thành số phát sinh rất thường xuyên. Ví dụ: nếu chúng ta đang nhận các giá trị từ các trường biểu mẫu HTML, thì chúng thường là các chuỗi.

Nếu chúng ta muốn tổng hợp chúng thì sao?

Phép cộng nhị phân sẽ thêm chúng dưới dạng chuỗi:

```js
      let apples = "2";
      let oranges = "3";

      alert( apples + oranges ); // "23", the binary plus concatenates strings
```

Nếu chúng ta muốn coi chúng là số, thì chúng ta có thể chuyển đổi và sau đó tổng hợp:

```js
      let apples = "2";
      let oranges = "3";

      // both values converted to numbers before the binary plus
      alert( +apples + +oranges ); // 5

      // the longer variant
      // alert( Number(apples) + Number(oranges) ); // 5
```

Từ quan điểm của một nhà toán học, sự phong phú của các phép cộng có vẻ lạ. Nhưng theo quan điểm của một lập trình viên, không có gì đặc biệt: đơn phân cộng được áp dụng trước, chúng chuyển đổi chuỗi (strings) thành số (numbers) và sau đó nhị phân cộng cộng chúng lại.

Tại sao các đơn phân cộng được áp dụng cho các giá trị trước nhị phân? Như chúng ta thấy, đó là vì *ưu tiên cao hơn* của chúng.

## Các toán tử ưu tiên

Nếu một biểu thức có nhiều toán tử, thứ tự thực thi được xác định bởi *ưu tiên* của chúng, hay nói cách khác, có một thứ tự ưu tiên ngầm giữa các toán tử.

Từ trường học, tất cả chúng ta đều biết rằng phép nhân trong biểu thức `1 + 2 * 2` nên được tính trước khi cộng. Đó chính xác là điều ưu tiên. Phép nhân được cho là có *độ ưu tiên cao hơn* phép cộng.

Dấu ngoặc đơn ghi đè bất kỳ quyền ưu tiên nào, vì vậy nếu chúng ta không hài lòng với vị trí, chúng ta có thể sử dụng chúng, như: `(1 + 2) * 2`.

Có nhiều toán tử trong JavaScript. Mỗi toán tử có một số ưu tiên tương ứng. Một trong những số lớn hơn thực hiện đầu tiên. Nếu quyền ưu tiên là như nhau, thứ tự thực hiện là từ trái sang phải.

Một trích xuất từ [bảng ưu tiên](https://developer.mozilla.org/en/JavaScript/Reference/operators/operator_precedence) (bạn không cần phải nhớ điều này, nhưng lưu ý rằng các toán tử đơn phân ưu tiên cao hơn các nhị phân tương ứng):

| Precedence | Name           | Sign |
|------------|----------------|------|
| ...        | ...            | ...  |
| 16         | unary plus     | `+`  |
| 16         | unary negation | `-`  |
| 14         | multiplication | `*`  |
| 14         | division       | `/`  |
| 13         | addition       | `+`  |
| 13         | subtraction    | `-`  |
| ...        | ...            | ...  |
| 3          | assignment     | `=`  |
| ...        | ...            | ...  |

Như chúng ta có thể thấy, "unary plus" có mức độ ưu tiên là `16`, cao hơn `13` cho "phép cộng" (cộng nhị phân). Đó là lý do tại sao trong biểu thức `"+apples + +oranges"` unary pluses hoạt động trước, và sau đó là phép cộng.

## Phép gán

Chúng ta hãy lưu ý rằng một phép gán `=` cũng là một toán tử. Nó được liệt kê trong bảng ưu tiên với mức độ ưu tiên rất thấp là `3`.

Đó là lý do tại sao khi chúng ta gán một biến, như `x = 2 * 2 + 1`, thì các phép tính được thực hiện trước và sau đó `=` được ước tính, lưu trữ kết quả trong `x`.

```js
      let x = 2 * 2 + 1;

      alert( x ); // 5
```

Có thể xâu chuỗi các phép gán:

```js
      let a, b, c;

      a = b = c = 2 + 2;

      alert( a ); // 4
      alert( b ); // 4
      alert( c ); // 4
```

Xâu chuỗi phép gán được đánh giá từ phải sang trái. Đầu tiên biểu thức ngoài cùng bên phải `2 + 2` được ước tính sau đó được gán cho các biến ở bên trái: `c`, `b` và `a`. Cuối cùng, tất cả các biến chia sẻ một giá trị duy nhất.

<br>

> ---

**📌 Toán tử gán `"="` trả về một giá trị**

Một toán tử luôn trả về một giá trị. Điều đó rõ ràng đối với hầu hết trong số chúng như một phép cộng `+` hoặc phép nhân `*`. Nhưng toán tử gán cũng tuân theo quy tắc đó.

Cuộc gọi `x = value` ghi `value` vào `x` *và sau đó trả về nó*.

Đây là demo sử dụng một phép gán như một phần của biểu thức phức tạp hơn:

```js
  let a = 1;
  let b = 2;

  let c = 3 - (a = b + 1);

  alert( a ); // 3
  alert( c ); // 0
```

Trong ví dụ trên, kết quả của `(a = b + 1)` là giá trị được gán cho `a` (đó là `3`). Sau đó, nó được sử dụng để trừ đi `3`.

Mã thật hài hước, phải không? Chúng ta nên hiểu cách thức hoạt động của nó, bởi vì đôi khi chúng ta có thể thấy nó trong các thư viện của bên thứ 3, nhưng chúng ta không nên tự viết bất cứ thứ gì như vậy. Những thủ thuật như vậy chắc chắn không làm cho mã rõ ràng hơn và dễ đọc hơn.

> ---

<br>

## Phần dư % (Remainder %)

Toán tử lấy phần dư `%` mặc dù giao diện của nó trông rất giống nhưng nó không có liên quan đến phần trăm.

Kết quả của `a % b` là phần dư còn lại của phép chia số nguyên của `a` cho `b`.

Ví dụ:

```js
      alert( 5 % 2 ); // 1 is a remainder of 5 divided by 2
      alert( 8 % 3 ); // 2 is a remainder of 8 divided by 3
      alert( 6 % 3 ); // 0 is a remainder of 6 divided by 3
```

## lũy thừa **

Toán tử lũy thừa `**` là một bổ sung gần đây cho ngôn ngữ JavaScript.

Đối với một số tự nhiên `b`, kết quả của `a ** b` là `a` nhân với chính nó `b` lần.

Ví dụ:

```js
      alert( 2 ** 2 ); // 4  (2 * 2)
      alert( 2 ** 3 ); // 8  (2 * 2 * 2)
      alert( 2 ** 4 ); // 16 (2 * 2 * 2 * 2)
```

Toán tử cũng làm việc với các số không nguyên của `a` và `b`, ví dụ:

```js
      alert( 4 ** (1/2) ); // 2 (power of 1/2 is the same as a square root, that's maths)
      alert( 8 ** (1/3) ); // 2 (power of 1/3 is the same as a cubic root)
```

## Tăng/giảm (Increment/decrement)

Tăng hoặc giảm một số là một trong những thao tác số phổ biến nhất.

Vì vậy, có các toán tử đặc biệt cho điều đó:

- **Tăng (Increment)** `++` tăng một biến bằng 1:

    ```js
    let counter = 2;
    counter++;      // works the same as counter = counter + 1, but is shorter
    alert( counter ); // 3
    ```
- **Giảm (Decrement)** `--` giảm một biến bằng 1:

    ```js
    let counter = 2;
    counter--;      // works the same as counter = counter - 1, but is shorter
    alert( counter ); // 1
    ```

<br>

> ---

**📌 Quan trọng:**

Increment/decrement chỉ có thể được áp dụng cho một biến. Nỗ lực sử dụng nó trên một giá trị như `5++` sẽ báo lỗi.

> ---

<br>

Các toán tử `++` và `--` có thể được đặt cả sau và trước biến.

- Khi toán tử đi sau biến, nó được gọi là "dạng hậu tố": `counter++`.
- "Dạng tiền tố" là khi toán tử đứng trước biến: `++counter`.

Cả hai bản ghi này đều làm như nhau: tăng `counter` lên `1`.

Có sự khác biệt nào không? Có, nhưng chúng ta chỉ có thể thấy nó nếu chúng ta sử dụng giá trị trả về của `++/--`.

Hãy làm rõ. Như chúng ta biết, tất cả các toán tử trả về một giá trị. Increment/decrement không phải là một ngoại lệ ở đây. Biểu mẫu tiền tố trả về giá trị mới, trong khi biểu mẫu hậu tố trả về giá trị cũ (trước khi tăng/giảm).

Để thấy sự khác biệt, đây là ví dụ:

```js
      let counter = 1;
      let a = ++counter; // (*)

      alert(a); // 2
```

Ở đây trong dòng `(*)` tiền tố gọi `++counter` tăng `counter` và trả về giá trị mới đó là `2`. Vì vậy, `alert` hiển thị `2`.

Bây giờ hãy sử dụng hậu tố:

```js
      let counter = 1;
      let a = counter++; // (*) changed ++counter to counter++

      alert(a); // 1
```

Trong dòng `(*)` mẫu *hậu tố (postfix)* `counter++` cũng tăng `counter`, nhưng trả về giá trị *cũ* (trước khi tăng). Vì vậy, `alert` hiển thị `1`.

Để tóm tắt:

- Nếu kết quả increment/decrement không được sử dụng, thì không có sự khác biệt trong hình thức sử dụng:

    ```js
    let counter = 0;
    counter++;
    ++counter;
    alert( counter ); // 2, the lines above did the same
    ```
- Nếu chúng ta muốn tăng giá trị *và* sử dụng kết quả của toán tử ngay bây giờ, thì chúng ta cần biểu mẫu tiền tố:

    ```js
    let counter = 0;
    alert( ++counter ); // 1
    ```
- Nếu chúng ta muốn tăng, nhưng sử dụng giá trị trước đó, thì chúng ta cần biểu mẫu hậu tố:

    ```js
    let counter = 0;
    alert( counter++ ); // 0
    ```

<br>

> ---

**📌 Tăng/giảm (Increment/decrement) giữa các toán tử khác**

Toán tử `++/--` cũng có thể được sử dụng bên trong một biểu thức. Ưu tiên của chúng cao hơn hầu hết các hoạt động số học khác.

Ví dụ:

```js
      let counter = 1;
      alert( 2 * ++counter ); // 4
```

So sánh với:

```js
      let counter = 1;
      alert( 2 * counter++ ); // 2, because counter++ returns the "old" value
```

Mặc dù về mặt kỹ thuật cho phép, ký hiệu như vậy thường làm cho mã khó đọc hơn. Một dòng làm nhiều việc -- không tốt.

Trong khi đọc mã, quét mắt "dọc" nhanh có thể dễ dàng bỏ lỡ `counter++`, và không biết rõ ràng là biến sẽ tăng.

Nên sử dụng kiểu "một dòng -- một hành động (one line -- one action)":

```js
      let counter = 1;
      alert( 2 * counter );
      counter++;
```

> ---

<br>

## Các toán tử bitwise (Bitwise operators)

Các toán tử bitwise coi các đối số là số nguyên 32-bit và hoạt động ở mức độ biểu diễn nhị phân của chúng.

Các toán tử này không dành riêng cho JavaScript. Chúng được hỗ trợ trong hầu hết các ngôn ngữ lập trình.

Danh sách các toán tử:

- AND ( `&` )
- OR ( `|` )
- XOR ( `^` )
- NOT ( `~` )
- LEFT SHIFT ( `<<` )
- RIGHT SHIFT ( `>>` )
- ZERO-FILL RIGHT SHIFT ( `>>>` )

Các toán tử này rất hiếm khi được sử dụng. Để hiểu chúng, chúng ta nên đi sâu vào biểu diễn số cấp độ thấp và sẽ không tối ưu để làm điều đó ngay bây giờ. Đặc biệt là vì chúng ta sẽ không cần chúng sớm. Nếu bạn tò mò, bạn có thể đọc bài viết [Toán tử bit](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Operators/Bitwise_Operators) trong MDN. Sẽ thực tế hơn để làm điều đó khi có nhu cầu thực sự.

## Sửa đổi tại chỗ (Modify-in-place)

Chúng ta thường cần áp dụng một toán tử cho một biến và lưu trữ kết quả mới trong đó.

Ví dụ:

```js
      let n = 2;
      n = n + 5;
      n = n * 2;
```

Ký hiệu này có thể được rút ngắn bằng cách sử dụng các toán tử `+ =` và `* =`:

```js
      let n = 2;
      n += 5; // now n = 7 (same as n = n + 5)
      n *= 2; // now n = 14 (same as n = n * 2)

      alert( n ); // 14
```

Các toán tử ngắn "sửa đổi và gán (modify-and-assign)" có sẵn cho tất cả các toán tử đối xứng và bitwise: `/=`, `-=`, v.v.

Các toán tử như vậy có cùng mức ưu tiên như một phép gán thông thường, vì vậy chúng chạy sau hầu hết các phép tính khác:

```js
      let n = 2;

      n *= 3 + 5;

      alert( n ); // 16  (right part evaluated first, same as n *= 8)
```

## Dấu phẩy (Comma)

Toán tử dấu phẩy `,` là một trong những toán tử hiếm và ít sử dụng nhất. Đôi khi, nó được sử dụng để viết mã ngắn hơn, vì vậy chúng ta cần biết nó để hiểu những gì đang diễn ra.

Toán tử dấu phẩy cho phép chúng ta đánh giá một số biểu thức, chia chúng bằng dấu phẩy `,`. Mỗi cái trong số chúng được đánh giá, nhưng kết quả của chỉ cái cuối cùng được trả về.

Ví dụ:

```js
      let a = (1 + 2, 3 + 4);

      alert( a ); // 7 (the result of 3 + 4)
```

Ở đây, biểu thức đầu tiên `1 + 2` được ước tính và kết quả của nó bị loại bỏ, sau đó `3 + 4` được ước tính và trả về kết quả.

<br>

> ---

**📌 Dấu phẩy có độ ưu tiên rất thấp**

Xin lưu ý rằng toán tử dấu phẩy có độ ưu tiên rất thấp, thấp hơn `=`, vì vậy dấu ngoặc đơn rất quan trọng trong ví dụ trên.

Nếu không có chúng: `a = 1 + 2, 3 + 4` sẽ đánh giá `+` trước tiên, tính tổng các số thành `a = 3, 7`, sau đó toán tử gán `=` gán `a = 3`, rồi số sau dấu phẩy `7` không được xử lý, vì vậy nó bị bỏ qua.

> ---

<br>

Tại sao chúng ta cần một toán tử như vậy mà vứt bỏ mọi thứ trừ phần cuối cùng?

Đôi khi mọi người sử dụng nó trong các cấu trúc phức tạp hơn để đặt một số hành động trong một dòng.

Ví dụ:

```js
      // ba thao tác trong một dòng
      for (a = 1, b = 3, c = a * b; a < 10; a++) {
       ...
      }
```

Các thủ thuật như vậy được sử dụng trong nhiều khung JavaScript, đó là lý do tại sao chúng ta đề cập đến chúng. Nhưng thông thường họ không cải thiện khả năng đọc mã, vì vậy chúng ta nên suy nghĩ kỹ trước khi viết như vậy.
No search results.
