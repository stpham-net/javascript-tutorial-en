# Numbers

Tất cả các số trong JavaScript được lưu trữ ở định dạng 64 bit [IEEE-754](http://en.wikipedia.org/wiki/IEEE_754-1985), còn được gọi là "độ chính xác kép (double precision)".

Hãy tóm tắt và mở rộng dựa trên những gì chúng ta hiện đang biết về chúng.

## Nhiều cách hơn để viết một số

Hãy tưởng tượng chúng ta cần viết 1 tỷ. Cách rõ ràng là:

```js
      let billion = 1000000000;
```

Nhưng trong cuộc sống thực, chúng ta thường tránh viết một chuỗi số 0 dài vì dễ gõ nhầm. Ngoài ra, chúng ta lười biếng. Chúng ta thường sẽ viết một cái gì đó như `"1bn"` cho một tỷ hoặc `"7.3bn"` cho 7 tỷ 300 triệu. Điều này cũng đúng với hầu hết các số lớn.

Trong JavaScript, chúng ta rút ngắn một số bằng cách nối thêm chữ `"e"` vào số đó và chỉ định số không:

```js
      let billion = 1e9;  // 1 billion, literally: 1 and 9 zeroes

      alert( 7.3e9 );  // 7.3 billions (7,300,000,000)
```

Nói cách khác, `"e"` nhân số đó với `1` với số lượng không (zeroes) cho trước.

```js
      1e3 = 1 * 1000
      1.23e6 = 1.23 * 1000000 
```


Bây giờ hãy viết một cái gì đó rất nhỏ. Nói, 1 micro giây (một phần triệu giây): 

```js
      let ms = 0.000001;
```

Giống như trước đây, sử dụng `"e"` có thể giúp đỡ. Nếu chúng ta muốn tránh viết các số 0 một cách rõ ràng, chúng ta có thể nói:

```js
      let ms = 1e-6; // six zeroes to the left from 1 
```

Nếu chúng ta đếm các số 0 trong `0,000001`, thì có 6 trong số chúng. Vì vậy, tự nhiên đó là `1e-6`.  

Nói cách khác, một số âm sau `"e"` có nghĩa là chia cho 1 với số 0 đã cho:

```js
      // -3 divides by 1 with 3 zeroes
      1e-3 = 1 / 1000 (=0.001)

      // -6 divides by 1 with 6 zeroes
      1.23e-6 = 1.23 / 1000000 (=0.00000123)
```

### Số hex, nhị phân và số bát phân (Hex, binary and octal numbers)

Các số [Hexadecimal](https://en.wikipedia.org/wiki/Hexadecimal) được sử dụng rộng rãi trong JavaScript để thể hiện màu sắc, mã hóa ký tự và cho nhiều thứ khác. Vì vậy, một cách tự nhiên, tồn tại một cách ngắn hơn để viết chúng: `0x` và sau đó là số.

Ví dụ:

```js
      alert( 0xff ); // 255
      alert( 0xFF ); // 255 (the same, case doesn't matter)
```

Các hệ thống số nhị phân và số bát phân hiếm khi được sử dụng, nhưng cũng được hỗ trợ bằng cách sử dụng tiền tố `0b` và `0o`:


```js
      let a = 0b11111111; // binary form of 255
      let b = 0o377; // octal form of 255

      alert( a == b ); // true, the same number 255 at both sides
```

Chỉ có 3 hệ thống số với sự hỗ trợ như vậy. Đối với các hệ thống số khác, chúng ta nên sử dụng hàm `parseInt` (mà chúng ta sẽ thấy sau trong chương này).

## toString(base)

Phương thức `num.toString(base)` trả về một chuỗi đại diện của `num` trong hệ thống số với `base` đã cho.

Ví dụ:

```js
      let num = 255;

      alert( num.toString(16) );  // ff
      alert( num.toString(2) );   // 11111111
```

`base` có thể thay đổi từ `2` đến `36`. Theo mặc định, nó là `10`.

Các trường hợp sử dụng phổ biến cho việc này là:

- **base=16** được sử dụng cho màu hex, mã hóa ký tự, v.v., chữ số có thể là `0..9` hoặc `A..F`.
- **base=2** chủ yếu là để gỡ lỗi các phép tính bitwise, các chữ số có thể là `0` hoặc `1`.
- **base=36** là mức tối đa, các chữ số có thể là `0..9` hoặc `A..Z`. Toàn bộ bảng chữ cái Latin được sử dụng để đại diện cho một số. Một trường hợp hài hước nhưng hữu ích cho `36` là khi chúng ta cần biến một số nhận dạng số dài thành một cái gì đó ngắn hơn, ví dụ để tạo một url ngắn. Có thể chỉ đơn giản là biểu diễn nó trong hệ thống số với cơ sở `36`:

    ```js
    alert( 123456..toString(36) ); // 2n9c
    ```

<br>

> ---

**📌 Hai dấu chấm để gọi một phương thức**

Xin lưu ý rằng hai dấu chấm trong `123456..toString (36)` không phải là một lỗi đánh máy. Nếu chúng ta muốn gọi một phương thức trực tiếp trên một số, như `toString` trong ví dụ trên, thì chúng ta cần đặt hai dấu chấm `..` sau nó.

Nếu chúng ta đặt một dấu chấm đơn: `123456.toString(36)`, thì sẽ có lỗi, vì cú pháp JavaScript bao hàm phần thập phân sau dấu chấm đầu tiên. Và nếu chúng ta đặt thêm một dấu chấm, thì JavaScript biết rằng phần thập phân trống và bây giờ đi tới phương thức.

Cũng có thể viết `(123456).toString(36)`.

> ---

<br>

## Làm tròn

Một trong những thao tác được sử dụng nhiều nhất khi làm việc với các số là làm tròn số.

Có một số chức năng tích hợp để làm tròn:

**`Math.floor`**

Làm tròn xuống: `3.1` trở thành `3` và `-1.1` trở thành `-2`.

**`Math.ceil`**

Làm tròn lên: `3.1` trở thành `4` và `-1.1` trở thành `-1`.

**`Math.round`**

Làm tròn đến số nguyên gần nhất: `3.1` trở thành `3`, `3.6` trở thành `4` và `-1.1` trở thành `-1`.

**`Math.trunc` (not supported by Internet Explorer)**

Xóa bất cứ thứ gì sau dấu thập phân mà không làm tròn: `3.1` trở thành `3`, `-1.1` trở thành `-1`.

Đây là bảng để tóm tắt sự khác biệt giữa chúng:

| | `Math.floor` | `Toán.ceil` | `Toán.round` | `Toán.trunc` |
| ------ | -------------- | ------------- | ------------- - | -------------- |
| `3.1` | `3` | `4` | `3` | `3` |
| `3.6` | `3` | `4` | `4` | `3` |
| `-1.1` | `-2` | `-1` | `-1` | `-1` |
| `-1.6` | `-2` | `-1` | `-2` | `-1` |


Các hàm này bao gồm tất cả các cách có thể để xử lý phần thập phân của một số. Nhưng điều gì sẽ xảy ra nếu chúng ta muốn làm tròn số thành `n-th` chữ số sau số thập phân?

Chẳng hạn, chúng ta có `1.2345` và muốn làm tròn nó thành 2 chữ số, chỉ nhận được` 1.23`.

Có hai cách để làm như vậy:

1. Nhân và chia.

    Ví dụ, để làm tròn số thành 2 chữ số sau số thập phân, chúng ta có thể nhân số đó với `100`, gọi hàm làm tròn và sau đó chia lại.
    
    ```js
    let num = 1.23456;

    alert( Math.floor(num * 100) / 100 ); // 1.23456 -> 123.456 -> 123 -> 1.23
    ```

2. Phương thức [toFixed(n)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number/toFixed) làm tròn số thành `n` chữ số sau điểm và trả về một chuỗi đại diện của kết quả.
        
    ```js
    let num = 12.34;
    alert( num.toFixed(1) ); // "12.3"
    ```

    Điều này làm tròn lên hoặc xuống giá trị gần nhất, tương tự như `Math.round`:

    ```js
    let num = 12.36;
    alert( num.toFixed(1) ); // "12.4"
    ```

    Xin lưu ý rằng kết quả của `toFixed` là một chuỗi. Nếu phần thập phân ngắn hơn yêu cầu, các số 0 được thêm vào cuối:

    ```js
    let num = 12.34;
    alert( num.toFixed(5) ); // "12.34000", added zeroes to make exactly 5 digits
    ```

    Chúng ta có thể chuyển đổi nó thành một số bằng cách sử dụng unary plus hoặc `Number()`: `+num.toFixed(5)`.

## Tính toán chính xác (Imprecise calculations)

Trong nội bộ, một số được biểu thị ở định dạng 64 bit [IEEE-754](http://en.wikipedia.org/wiki/IEEE_754-1985), do đó, có chính xác 64 bit để lưu trữ một số: 52 trong số chúng được sử dụng để lưu trữ các chữ số, 11 trong số chúng lưu vị trí của dấu thập phân (chúng là 0 đối với số nguyên) và 1 bit dành cho dấu (sign).

Nếu một số quá lớn, nó sẽ tràn bộ nhớ 64 bit, có khả năng mang lại vô hạn (infinity):

```js
      alert( 1e500 ); // Infinity 
```

Điều có thể ít rõ ràng hơn, nhưng xảy ra khá thường xuyên, là sự mất độ chính xác.

Hãy xem xét test này (falsy!): 

```js
      alert( 0.1 + 0.2 == 0.3 ); // *!*false*/!*
```

Điều đó đúng, nếu chúng ta kiểm tra xem tổng của `0.1` và `0.2` có phải là `0.3` hay không, chúng ta sẽ nhận được `false`. 

Lạ thật! Vậy thì nó là gì nếu không phải là `0.3`?

```js
      alert( 0.1 + 0.2 ); // 0.30000000000000004
```

Ôi! Có nhiều hậu quả hơn so với một so sánh không chính xác ở đây. Hãy tưởng tượng bạn đang tạo một trang web e-shopping và khách truy cập đặt hàng `$0.10` và `$0.20` vào rỏ hàng của họ. Tổng số đơn đặt hàng sẽ là `$0.30000000000000004`. Điều đó sẽ làm bất ngờ bất cứ ai.

Nhưng tại sao điều này xảy ra?

Một số được lưu trữ trong bộ nhớ ở dạng nhị phân của nó, một chuỗi các số và số không. Nhưng các phân số như `0.1`, `0.2` trông đơn giản trong hệ thống số thập phân thực sự là các phân số không có hồi kết ở dạng nhị phân của chúng.

Nói cách khác, `0.1` là gì? Nó là một chia cho mười `1/10`, một phần mười. Trong hệ thống số thập phân, số đó dễ dàng được biểu diễn. So sánh nó với một phần ba: `1/3`. Nó trở thành một phần vô tận `0.33333(3)`. 

Vì vậy, phân chia theo powers `10` được đảm bảo hoạt động tốt trong hệ thập phân, nhưng phân chia theo `3` thì không. Vì lý do tương tự, trong hệ thống số nhị phân, sự phân chia theo powers của `2` được đảm bảo để làm việc, nhưng `1/10` trở thành một phân số nhị phân vô tận.

Không có cách nào để lưu trữ *chính xác 0.1* hoặc *chính xác 0.2* bằng hệ thống nhị phân, giống như không có cách nào để lưu trữ một phần ba dưới dạng phân số thập phân.

Định dạng số IEEE-754 giải quyết điều này bằng cách làm tròn đến số gần nhất có thể. Các quy tắc làm tròn này thường không cho phép chúng ta thấy rằng "mất độ chính xác nhỏ", vì vậy con số hiển thị là `0.3`. Nhưng hãy cẩn thận, sự mất mát vẫn tồn tại.

Chúng ta có thể thấy điều này trong hành động:

```js
      alert( 0.1.toFixed(20) ); // 0.10000000000000000555
```

Và khi chúng ta tổng hai số, "tổn thất chính xác" của chúng cộng lại.

Đó là lý do tại sao `0.1 + 0.2` không chính xác là `0.3`.

<br>

> ---

**📌 Không chỉ JavaScript**

Vấn đề tương tự tồn tại trong nhiều ngôn ngữ lập trình khác.

PHP, Java, C, Perl, Ruby cho kết quả chính xác như nhau, vì chúng dựa trên cùng một định dạng số. 

> ---

<br>

Chúng ta có thể làm việc xung quanh vấn đề? Chắc chắn, có một số cách:

1. Chúng ta có thể làm tròn kết quả với sự trợ giúp của phương thức [toFixed(n)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number/toFixed):

    ```js
    let sum = 0.1 + 0.2;
    alert( sum.toFixed(2) ); // 0.30
    ```

    Xin lưu ý rằng `toFixed` luôn trả về một chuỗi. Nó đảm bảo rằng nó có 2 chữ số sau dấu thập phân. Điều đó thực sự tiện lợi nếu chúng ta có một e-shopping và cần hiển thị `$0.30`. Đối với các trường hợp khác, chúng ta có thể sử dụng dấu cộng đơn để ép buộc nó thành một số:

    ```js
    let sum = 0.1 + 0.2;
    alert( +sum.toFixed(2) ); // 0.3
    ```

2. Chúng ta có thể tạm thời biến số thành số nguyên cho toán học và sau đó hoàn nguyên nó. Nó hoạt động như thế này:

    ```js
    alert( (0.1 * 10 + 0.2 * 10) / 10 ); // 0.3
    ```

    Điều này hoạt động vì khi chúng ta thực hiện `0.1 * 10 = 1` và `0.2 * 10 = 2` thì cả hai số đều trở thành số nguyên và không mất độ chính xác. 

3. Nếu chúng ta giao dịch với một cửa hàng, thì giải pháp căn cơ nhất sẽ là lưu trữ tất cả giá bằng xu và không sử dụng phân số nào cả. Nhưng nếu chúng ta áp dụng giảm giá 30% thì sao? Trong thực tế, việc tránh hoàn toàn các phân số hiếm khi khả thi, vì vậy các giải pháp trên giúp tránh được cạm bẫy này.

<br>

> ---

**📌 Điều buồn cười**

Hãy thử chạy nó:

```js
      // Hello! I'm a self-increasing number! 
      alert( 9999999999999999 ); // shows 10000000000000000
```

Điều này chịu cùng một vấn đề: mất độ chính xác. Có 64 bit cho số, 52 trong số chúng có thể được sử dụng để lưu trữ các chữ số, nhưng điều đó là không đủ. Vì vậy, các chữ số ít quan trọng nhất biến mất.

JavaScript không gây ra lỗi trong các sự kiện như vậy. Nó làm hết sức mình để phù hợp với số vào định dạng mong muốn, nhưng thật không may, định dạng này không đủ lớn.

> ---

<br>
<br>

> ---

**📌 Hai số không**

Một hậu quả buồn cười khác của việc biểu diễn bên trong các con số là sự tồn tại của hai số 0: `0` và `-0`.

Đó là bởi vì một dấu được biểu thị bằng một bit đơn, vì vậy mọi số có thể dương hoặc âm, kể cả số không. 

Trong hầu hết các trường hợp, sự khác biệt là không đáng chú ý, bởi vì các toán tử phù hợp để coi chúng là như nhau.

> ---

<br>

## Bài kiểm tra: isFinite và isNaN

Ghi nhớ hai giá trị số đặc biệt này?

- `Infinity` (và `-Infinity`) là một giá trị số đặc biệt lớn hơn (nhỏ hơn) bất cứ thứ gì.
- `NaN` đại diện cho một lỗi.

Chúng thuộc kiểu `number`, nhưng không phải là số "bình thường ", do đó, có các hàm đặc biệt để kiểm tra chúng:

- `isNaN(value)` chuyển đổi đối số của nó thành một số và sau đó kiểm tra xem nó có phải là `NaN`:

    ```js
    alert( isNaN(NaN) ); // true
    alert( isNaN("str") ); // true
    ```

    Nhưng chúng ta có cần chức năng này không? Chúng ta không thể sử dụng phép so sánh `=== NaN` sao? Xin lỗi, nhưng câu trả lời là không. Giá trị `NaN` là duy nhất ở chỗ nó không bằng bất cứ thứ gì, kể cả chính nó:

    ```js
    alert( NaN === NaN ); // false
    ```

- `isFinite(value)` chuyển đổi đối số của nó thành một số và trả về `true` nếu đó là số thông thường, không phải là `NaN/Infinity/-Infinity`:

    ```js
    alert( isFinite("15") ); // true
    alert( isFinite("str") ); // false, because a special value: NaN
    alert( isFinite(Infinity) ); // false, because a special value: Infinity
    ```

Đôi khi `isFinite` được sử dụng để xác thực xem giá trị chuỗi có phải là số thông thường hay không:

```js
      let num = +prompt("Enter a number", '');

      // will be true unless you enter Infinity, -Infinity or not a number
      alert( isFinite(num) );
```

Xin lưu ý rằng một chuỗi trống hoặc chỉ có khoảng trắng được coi là `0` trong tất cả các hàm số bao gồm cả `isFinite`.  

<br>

> ---

**📌 So sánh với `Object.is`**

Có một phương thức tích hợp đặc biệt [Object.is](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/is) để so sánh các giá trị như `===`, nhưng đáng tin cậy hơn cho hai trường hợp góc cạnh:

1. Nó hoạt động với `NaN`: `Object.is(NaN, NaN) === true`, đó là một điều tốt. 
2. Các giá trị `0` và `-0` khác nhau: `Object.is(0, -0) === false`, điều này hiếm khi quan trọng, nhưng về mặt kỹ thuật các giá trị này khác nhau.

Trong tất cả các trường hợp khác, `Object.is(a, b)` giống như `a === b`. 

Cách so sánh này thường được sử dụng trong đặc tả JavaScript. Khi một thuật toán nội bộ cần so sánh hai giá trị giống hệt nhau, nó sử dụng `Object.is` (được gọi nội bộ là [SameValue](https://tc39.github.io/ecma262/#sec-samevalue)).

> ---

<br>

## parseInt và parseFloat

Chuyển đổi số bằng cách sử dụng dấu cộng `+` hoặc `Number()` là nghiêm ngặt. Nếu một giá trị không chính xác là một số, nó sẽ thất bại:

```js
      alert( +"100px" ); // NaN
```

Ngoại lệ duy nhất là khoảng trắng ở đầu hoặc cuối chuỗi, vì chúng bị bỏ qua.

Nhưng trong cuộc sống thực, chúng ta thường có các giá trị theo đơn vị, như ``"100px"` hoặc `"12pt"` trong CSS. Ngoài ra ở nhiều quốc gia, ký hiệu tiền tệ đi sau số tiền, vì vậy chúng ta có `"19€"` và muốn trích xuất một giá trị số từ đó.

Đó là những gì `parseInt` và `parseFloat` dành cho.

Chúng "đọc" một số từ một chuỗi cho đến khi chúng không thể. Trong trường hợp có lỗi, số đã thu thập được trả về. Hàm `parseInt` trả về một số nguyên, trong khi `parseFloat` sẽ trả về một số thập phân:

```js
      alert( parseInt('100px') ); // 100
      alert( parseFloat('12.5em') ); // 12.5

      alert( parseInt('12.3') ); // 12, only the integer part is returned
      alert( parseFloat('12.3.4') ); // 12.3, the second point stops the reading
```

Có những tình huống khi `parseInt/parseFloat` sẽ return `NaN`. Nó xảy ra khi không có chữ số nào có thể được đọc:

```js
      alert( parseInt('a123') ); // NaN, the first symbol stops the process
```

<br>

> ---

**📌 Đối số thứ hai của `parseInt(str, radix)`**

Hàm `parseInt()` có tham số thứ hai tùy chọn. Nó chỉ định cơ sở của hệ thống số, vì vậy `parseInt` cũng có thể phân tích các chuỗi số hex, số nhị phân, v.v.

```js
      alert( parseInt('0xff', 16) ); // 255
      alert( parseInt('ff', 16) ); // 255, without 0x also works

      alert( parseInt('2n9c', 36) ); // 123456
```

> ---

<br>

## Các hàm toán học khác

JavaScript có một built-in object [Math](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Math) chứa một thư viện nhỏ các hàm và hằng toán học.

Một vài ví dụ:

**`Math.random()`**

Trả về một số ngẫu nhiên từ 0 đến 1 (không bao gồm 1)

```js
      alert( Math.random() ); // 0.1234567894322
      alert( Math.random() ); // 0.5435252343232
      alert( Math.random() ); // ... (any random numbers)
```

**`Math.max(a, b, c...)` / `Math.min(a, b, c...)`**

Trả về giá trị lớn nhất/nhỏ nhất từ dãy số tùy ý của các đối số.

```js
      alert( Math.max(3, 5, -10, 0, 1) ); // 5
      alert( Math.min(1, 2) ); // 1
```

**`Math.pow(n, power)`**

Trả về mũ của `n` đã cho

```js
      alert( Math.pow(2, 10) ); // 2 in power 10 = 1024
```

Có nhiều hàm và hằng trong đối tượng `Math`, bao gồm lượng giác, mà bạn có thể tìm thấy trong [docs for the Math](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Math) object.

## Tóm lược

Để viết số lớn:

- Nối `"e"` với số 0 được tính vào số. Giống như: `123e6` là `123` với 6 số không.
- Một số âm sau `"e"` làm cho số đó được chia cho 1 với các số 0 đã cho. Dành cho một phần triệu hoặc tương tự.

Đối với các hệ thống số khác nhau:

- Có thể viết số trực tiếp trong các hệ hex (`0x`), bát phân (`0o`) và nhị phân (`0b`)
- `parseInt(str, base)` phân tích một số nguyên từ bất kỳ hệ thống số nào có base: `2 ≤ base ≤ 36`.
- `num.toString(base)` chuyển đổi một số thành một chuỗi trong hệ thống số với `base` đã cho.

Để chuyển đổi các giá trị như `12pt` và `100px` thành số:

- Sử dụng `parseInt/parseFloat` cho chuyển đổi "mềm", đọc một số từ một chuỗi và sau đó trả về giá trị chúng có thể đọc trước khi xảy ra lỗi. 

Đối với phân số:

- Làm tròn bằng cách sử dụng `Math.floor`, `Math.ceil`, `Math.trunc`, `Math.round` hoặc `num.toFixed(precision)`.
- Đảm bảo nhớ có sự mất độ chính xác khi làm việc với phân số.

Nhiều hàm toán học hơn:

- Xem đối tượng [Math](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Math) khi bạn cần chúng. Thư viện rất nhỏ, nhưng có thể đáp ứng các nhu cầu cơ bản.


