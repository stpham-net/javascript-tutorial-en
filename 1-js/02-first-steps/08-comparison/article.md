# So sánh (Comparisons)

Nhiều toán tử so sánh chúng ta biết từ toán học:

- Lớn hơn/nhỏ hơn (Greater/less): <code>a > b</code>, <code>a < b</code>.
- Lớn hơn/nhỏ hơn hoặc bằng: <code>a >= b</code>, <code>a <= b</code>.
- Kiểm tra bằng nhau được viết là `a == b` (xin lưu ý dấu bằng kép `=`. Một ký hiệu đơn `a = b` có nghĩa là một phép gán).
- Không bằng. Trong toán học, ký hiệu là <code>≠</code>, trong JavaScript nó được viết dưới dạng một phép gán với dấu chấm than trước nó: <code>a != b</code>.

## Boolean là kết quả

Cũng như tất cả các toán tử khác, một phép so sánh trả về một giá trị. Giá trị là loại boolean.

- `true` -- có nghĩa là "có", "chính xác" hoặc "sự thật".
- `false` -- có nghĩa là "không", "sai" hoặc "dối trá".

Ví dụ:

```js
      alert( 2 > 1 );  // true (correct)
      alert( 2 == 1 ); // false (wrong)
      alert( 2 != 1 ); // true (correct)
```

Một kết quả so sánh có thể được gán cho một biến, giống như bất kỳ giá trị nào:

```js
      let result = 5 > 4; // assign the result of the comparison
      alert( result ); // true
```

## So sánh chuỗi

Để xem chuỗi nào lớn hơn chuỗi nào, cái gọi là là "dictionary" hoặc "lexicographical" (so sánh theo từ điển) được sử dụng.

Nói cách khác, các chuỗi được so sánh từng chữ cái.

Ví dụ:

```js
      alert( 'Z' > 'A' ); // true
      alert( 'Glow' > 'Glee' ); // true
      alert( 'Bee' > 'Be' ); // true
```

Thuật toán để so sánh hai chuỗi rất đơn giản:

1. So sánh các ký tự đầu tiên của cả hai chuỗi.
2. Nếu chuỗi đầu tiên lớn hơn (hoặc nhỏ hơn), thì chuỗi đầu tiên lớn hơn (hoặc nhỏ hơn) so với chuỗi thứ hai. Xong.
3. Mặt khác, nếu các ký tự đầu tiên bằng nhau, tiếp tục so sánh các ký tự thứ hai theo cùng một cách.
4. Lặp lại cho đến khi kết thúc chuỗi.
5. Nếu cả hai chuỗi kết thúc giống nhau, thì chúng bằng nhau. Nếu không thì chuỗi dài hơn là lớn hơn.

Trong ví dụ trên, phép so sánh `'Z' > 'A'` nhận được kết quả ở bước đầu tiên.

Các chuỗi `"Glow"` và `"Glee"` được so sánh theo từng ký tự:

1. `G` giống `G`.
2. `l` giống như `l`.
3. `o` lớn hơn `e`. Dừng ở đây. Chuỗi đầu tiên là lớn hơn.

<br>

> ---

**📌 Không phải là một từ điển thực sự, mà là thứ tự Unicode**

Thuật toán so sánh được đưa ra ở trên gần tương đương với thuật toán được sử dụng trong từ điển sách hoặc danh bạ điện thoại. Nhưng nó không hoàn toàn giống nhau.

Ví dụ, trường hợp quan trọng. Một chữ cái viết hoa `"A"` không bằng chữ thường `"a"`. Cái nào lớn hơn? Thật ra, chữ thường `"a"` là lớn hơn. Tại sao? Bởi vì ký tự chữ thường có chỉ mục lớn hơn trong bảng mã hóa nội bộ (Unicode). Chúng tôi sẽ quay lại chi tiết cụ thể và hậu quả trong chương **string**.

> ---

<br>

## So sánh các loại khác nhau

Khi các giá trị so sánh thuộc về các loại khác nhau, chúng được chuyển đổi thành số.

Ví dụ:

```js
      alert( '2' > 1 ); // true, string '2' becomes a number 2
      alert( '01' == 1 ); // true, string '01' becomes a number 1
```

Đối với các giá trị boolean, `true` trở thành `1` và` false` trở thành `0`, đó là lý do:

```js
      alert( true == 1 ); // true
      alert( false == 0 ); // true
```

<br>

> ---

**📌 Một hậu quả buồn cười**

Có thể là cùng một lúc:

- Hai giá trị bằng nhau.
- Một trong số đó là `true` là boolean và cái còn lại là `false` là boolean.

Ví dụ:

```js
      let a = 0;
      alert( Boolean(a) ); // false

      let b = "0";
      alert( Boolean(b) ); // true

      alert(a == b); // true!
```

Từ quan điểm của JavaScript đó là khá bình thường. Kiểm tra bằng nhau chuyển đổi bằng cách sử dụng chuyển đổi số (do đó `"0"` trở thành `0`), trong khi chuyển đổi `Boolean` sử dụng một bộ quy tắc khác.

> ---

<br>

## Bằng nhau nghiêm ngặt (Strict equality)

Kiểm tra bằng nhau thông thường `==` có vấn đề. Nó không thể có sự khác nhau giữa `0` với `false`:

```js
      alert( 0 == false ); // true
```

Điều tương tự với một chuỗi rỗng:

```js
      alert( '' == false ); // true
```

Đó là bởi vì các toán hạng của các loại khác nhau được chuyển đổi thành một số bởi toán tử bằng nhau `==`. Một chuỗi rỗng, giống như `false`, trở thành số không.

Phải làm gì nếu chúng ta muốn phân biệt `0` với `false`?

**Một toán tử bằng nhau nghiêm ngặt `===` kiểm tra bằng nhau mà không cần chuyển đổi kiểu.**

Nói cách khác, nếu `a` và `b` thuộc các loại khác nhau, thì `a === b` ngay lập tức trả về `false` mà không cần cố gắng chuyển đổi chúng.

Hãy thử nó:

```js
      alert( 0 === false ); // false, because the types are different
```

Ngoài ra còn tồn tại một toán tử "không bằng nhau nghiêm ngặt" `!==`, như một sự tương tự cho `!=`.

Toán tử kiểm tra bằng nhau nghiêm ngặt dài hơn một chút để viết, nhưng làm cho nó rõ ràng những gì đang xảy ra và để lại ít không gian hơn cho các lỗi.

## So sánh với null và undefined

Chúng ta hãy xem các trường hợp góc cạnh hơn.

Có một hành vi không trực quan khi `null` hoặc `undefined` được so sánh với các giá trị khác.


Đối với kiểm tra bằng nhau nghiêm ngặt `===`

Các giá trị này là khác nhau, bởi vì mỗi trong số chúng thuộc về một loại riêng của nó.

```js
      alert( null === undefined ); // false
```

Đối với kiểm tra không nghiêm ngặt `==`

Có một quy tắc đặc biệt. Hai người này là một "cặp đôi ngọt ngào": chúng bằng nhau (theo nghĩa của `==`), nhưng không có giá trị nào khác.

```js
      alert( null == undefined ); // true
```

Đối với toán học và các so sánh khác `< > <= >=`

Các giá trị `null/undefined` được chuyển đổi thành một số: `null` trở thành `0`, trong khi `undefined` trở thành `NaN`.

Bây giờ hãy xem những điều buồn cười xảy ra khi chúng ta áp dụng các quy tắc đó. Và, điều quan trọng hơn, làm thế nào để không rơi vào bẫy với các tính năng này.

### Kết quả kỳ lạ: null vs 0

Hãy so sánh `null` với số không:

```js
      alert( null > 0 );  // (1) false
      alert( null == 0 ); // (2) false
      alert( null >= 0 ); // (3) true
```

Vâng, về mặt toán học thật kỳ lạ. Kết quả cuối cùng nói rằng "`null` lớn hơn hoặc bằng 0". thì như vậy, một trong những so sánh ở trên phải chính xác, nhưng cả hai đều sai.

Lý do là một kiểm tra bằng nhau `==` và so sánh `> < >= <=` hoạt động khác nhau. So sánh chuyển đổi `null` thành một số, do đó coi nó là `0`. Đó là lý do tại sao (3) `null >= 0` là đúng và (1) `null > 0` là sai.

Mặt khác, kiểm tra bằng nhau `==` cho `undefined` và `null` được định nghĩa sao cho không có bất kỳ chuyển đổi nào, chúng bằng nhau và không bằng bất cứ thứ gì khác. Đó là lý do tại sao (2) `null == 0` là sai.

### Một undefined không thể so sánh

Giá trị 'undefined` không nên tham gia so sánh ở tất cả:

```js
      alert( undefined > 0 ); // false (1)
      alert( undefined < 0 ); // false (2)
      alert( undefined == 0 ); // false (3)
```

Tại sao nó không thích số 0 nhiều như vậy? Luôn luôn sai!

Chúng tôi đã có những kết quả này bởi vì:

- So sánh `(1)` và `(2)` trả về `false` vì `undefined` được chuyển đổi thành `NaN`. Và `NaN` là một giá trị số đặc biệt trả về `false` cho tất cả các phép so sánh.
- Kiểm tra đẳng thức `(3)` trả về `false`, bởi vì `undefined` chỉ bằng `null` và không có giá trị nào khác.

### Tránh những vấn đề (Evade problems)

Tại sao chúng ta quan sát những ví dụ này? Chúng ta có nên nhớ những đặc thù này mọi lúc không? Vâng, không thực sự. Trên thực tế, những điều khó khăn này sẽ dần trở nên quen thuộc theo thời gian, nhưng có một cách vững chắc để tránh mọi vấn đề với chúng.

Chỉ cần đối xử với bất kỳ so sánh nào với `undefined/null` ngoại trừ sự bằng nhau nghiêm ngặt `===` với sự quan tâm đặc biệt.

Không sử dụng so sánh `>= > < <=` với một biến có thể là `null/undefined`, trừ khi bạn thực sự chắc chắn những gì bạn đang làm. Nếu một biến có thể có các giá trị như vậy, thì hãy kiểm tra chúng một cách riêng biệt.

## Tóm lược

- Toán tử so sánh trả về một giá trị logic.
- Chuỗi được so sánh từng chữ cái theo thứ tự "từ điển".
- Khi các giá trị của các loại khác nhau được so sánh, chúng được chuyển đổi thành số (ngoại trừ kiểm tra bằng nhau nghiêm ngặt).
- Các giá trị `null` và `undefined` bằng `==` lẫn nhau và không bằng bất kỳ giá trị nào khác.
- Hãy cẩn thận khi sử dụng các phép so sánh như `>` hoặc `<` với các biến đôi khi có thể là `null/undefined`. Tạo một kiểm tra riêng cho `null/undefined` là một ý tưởng tốt.
No search results.
