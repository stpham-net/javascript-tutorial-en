# Chuyển đổi kiểu

Hầu hết thời gian, các toán tử và hàm tự động chuyển đổi một giá trị thành đúng kiểu. Đó gọi là "chuyển đổi kiểu".

Ví dụ, `alert` tự động chuyển đổi bất kỳ giá trị nào thành chuỗi để hiển thị nó. Các phép toán chuyển đổi giá trị thành số.

Cũng có trường hợp khi chúng ta cần chuyển đổi rõ ràng một giá trị để đặt mọi thứ đúng.

<br>

> ---

**📌 Chưa nói về objects vội**

Trong chương này, chúng ta chưa bao gồm các object. Ở đây chúng ta nghiên cứu nguyên thủy (primitives) trước. Sau đó, sau khi chúng ta tìm hiểu về các object, chúng ta sẽ biết cách object chuyển đổi như nào trong chương **object-toprimitive**.

> ---

<br>

## ToString

Chuyển đổi chuỗi xảy ra khi chúng ta cần dạng chuỗi của một giá trị.

Ví dụ, `alert(value)` thực hiện chuyển đổi để hiển thị giá trị.

Chúng ta cũng có thể sử dụng hàm `String(value)` cho điều đó:

```js
      let value = true;
      alert(typeof value); // boolean

      value = String(value); // now value is a string "true"
      alert(typeof value); // string
```

Chuyển đổi chuỗi chủ yếu là rõ ràng. Một `false` trở thành `"false"`, `null` trở thành `"null"`, v.v.

## ToNumber

Chuyển đổi số xảy ra trong các hàm và biểu thức toán học tự động.

Ví dụ, khi phép chia `/` được áp dụng cho các số không:

```js
      alert( "6" / "2" ); // 3, strings are converted to numbers
```

Chúng ta có thể sử dụng hàm `Number(value)` để chuyển đổi rõ ràng một `value`:

```js
      let str = "123";
      alert(typeof str); // string

      let num = Number(str); // becomes a number 123

      alert(typeof num); // number
```

Chuyển đổi rõ ràng (explicit conversion) thường được yêu cầu khi chúng ta đọc một giá trị từ nguồn dựa trên chuỗi như dạng văn bản, nhưng chúng ta hy vọng một số được nhập.

Nếu chuỗi không phải là số hợp lệ, kết quả của chuyển đổi sẽ là `NaN`, ví dụ:

```js
      let age = Number("an arbitrary string instead of a number");

      alert(age); // NaN, conversion failed
```

Quy tắc chuyển đổi số:

| Value            |  Becomes... |
|------------------|-------------|
| `undefined`      | `NaN`       |
| `null`           | `0`         |
| `true and false` | `1` and `0` |
| `string`         | Khoảng ở đầu và cuối được loại bỏ. Sau đó, nếu chuỗi còn lại trống, kết quả là `0`. Mặt khác, Nếu số được "đọc" từ chuỗi. Sinh ra một lỗi `NaN`. |

Ví dụ:

```js
      alert( Number("   123   ") ); // 123
      alert( Number("123z") );      // NaN (error reading a number at "z")
      alert( Number(true) );        // 1
      alert( Number(false) );       // 0
```

Xin lưu ý rằng `null` và `undefined` hoạt động khác nhau ở đây: `null` trở thành số 0, trong khi `undefined` trở thành `NaN`.

<br>

> ---

**📌 Bổ sung '+' nối chuỗi (Addition '+' concatenates strings)**

Hầu như tất cả các hoạt động toán học chuyển đổi giá trị thành số. Với một ngoại lệ đáng chú ý của phép cộng `+`. Nếu một trong các giá trị được thêm vào là một chuỗi thì một giá trị khác cũng được chuyển đổi thành một chuỗi.

Sau đó, nó kết nối (joins) chúng:

```js
      alert( 1 + '2' ); // '12' (string to the right)
      alert( '1' + 2 ); // '12' (string to the left)
```

Điều đó chỉ xảy ra khi ít nhất một trong các đối số là một chuỗi. Mặt khác, các giá trị được chuyển đổi thành số.

> ---

<br>

## ToBoolean

Chuyển đổi Boolean là đơn giản nhất.

Nó xảy ra trong các hoạt động logic (sau này chúng ta sẽ gặp các bài kiểm tra điều kiện và các thể loại khác), nhưng cũng có thể được thực hiện thủ công với lệnh gọi `Boolean (value)`.

Quy tắc chuyển đổi:

- Các giá trị trực quan "rỗng (empty)", như `0`, một chuỗi rỗng (empty string), `null`, `undefined` và `NaN` trở thành `false`.
- Các giá trị khác trở thành `true`.

Ví dụ:

```js
      alert( Boolean(1) ); // true
      alert( Boolean(0) ); // false

      alert( Boolean("hello") ); // true
      alert( Boolean("") ); // false
```

<br>

> ---

**📌 Xin lưu ý: chuỗi có zero `"0"` là `true`**

Một số ngôn ngữ (cụ thể là PHP) coi `"0"` là `false`. Nhưng trong JavaScript, một chuỗi không trống luôn là `true`.

```js
      alert( Boolean("0") ); // true
      alert( Boolean(" ") ); // spaces, also true (any non-empty string is true)
```

> ---

<br>

## Tóm lược

Ba kiểu chuyển đổi được sử dụng rộng rãi nhất là: to string, to number và boolean.

**`ToString`** -- Xảy ra khi chúng ta xuất (output) một cái gì đó, có thể được thực hiện với `String(value)`. Việc chuyển đổi thành chuỗi thường rõ ràng đối với các giá trị nguyên thủy (primitive values).

**`ToNumber`** -- Xảy ra trong các phép toán, có thể được thực hiện với `Number(value)`.

Việc chuyển đổi tuân theo các quy tắc:

| Value          |  Becomes... |
|----------------|-------------|
| `undefined`    | `NaN`       |
| `null`         | `0`         |
| `true / false` | `1 / 0`     |
| `string`       | Chuỗi được đọc "nguyên trạng", khoảng trắng từ cả hai phía bị bỏ qua. Một chuỗi rỗng trở thành `0`. Một lỗi trả về `NaN`. |

**`ToBoolean`** -- Xảy ra trong các hoạt động logic hoặc có thể được thực hiện với `Boolean(value)`.

Theo các quy tắc:

| Value                                 | Becomes...  |
|---------------------------------------|-------------|
| `0`, `null`, `undefined`, `NaN`, `""` | `false`     |
| bất kỳ giá trị nào khác               | `true`      |


Hầu hết các quy tắc này là dễ hiểu và dễ nhớ. Các ngoại lệ đáng chú ý nơi mọi người thường mắc lỗi là:

- `undefined` là `NaN` dưới dạng số, không phải `0`.
- `"0"` và các space-only string như `"   "` là true như một boolean.

Các object không được đề cập ở đây, chúng ta sẽ quay lại với chúng sau trong chương **object-toprimitive** dành riêng cho các object, sau khi chúng ta tìm hiểu thêm những điều cơ bản về JavaScript.
