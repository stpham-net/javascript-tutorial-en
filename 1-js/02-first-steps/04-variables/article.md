# Biến

Hầu hết thời gian, một ứng dụng JavaScript cần phải làm việc với thông tin. Dưới đây là 2 ví dụ:
1. Cửa hàng trực tuyến - thông tin có thể bao gồm hàng hóa được bán và giỏ hàng.
2. Một ứng dụng trò chuyện - thông tin có thể bao gồm người dùng, tin nhắn và nhiều hơn nữa.

Các biến được sử dụng để lưu trữ thông tin này.

## Một biến

Một [biến](https://en.wikipedia.org/wiki/Variable_(computer_science)) là "lưu trữ có tên (named storage)" cho dữ liệu. Chúng tôi có thể sử dụng các biến để lưu trữ goodies, khách truy cập và dữ liệu khác.

Để tạo một biến trong JavaScript, chúng ta cần sử dụng từ khóa `let`.

Câu lệnh dưới đây tạo ra (nói cách khác: *khai báo (declares)* hoặc *định nghĩa (defines)*) một biến có tên "message":

```js
      let message;
```

Bây giờ chúng ta có thể đặt một số dữ liệu vào nó bằng cách sử dụng toán tử gán `=`:

```js
      let message;

      message = 'Hello'; // store the string
```

Chuỗi bây giờ được lưu vào vùng nhớ được liên kết với biến. Chúng ta có thể truy cập nó bằng tên biến:

```js
      let message;
      message = 'Hello!';

      alert(message); // shows the variable content
```

Để ngắn gọn, chúng ta có thể hợp nhất khai báo biến và gán thành một dòng duy nhất:

```js
      let message = 'Hello!'; // define the variable and assign the value

      alert(message); // Hello!
```

Chúng ta cũng có thể khai báo nhiều biến trong một dòng:

```js
      let user = 'John', age = 25, message = 'Hello';
```

Điều đó có vẻ ngắn hơn, nhưng nó không được khuyến khích. Để dễ đọc hơn, vui lòng sử dụng một dòng cho mỗi biến.

Biến thể multiline dài hơn một chút, nhưng dễ đọc hơn:

```js
      let user = 'John';
      let age = 25;
      let message = 'Hello';
```

Một số người cũng viết nhiều biến như thế này:

```js
      let user = 'John',
        age = 25,
        message = 'Hello';
```

...Hoặc thậm chí theo kiểu "dấu phẩy đầu tiên":

```js
      let user = 'John'
        , age = 25
        , message = 'Hello';
```

Về mặt kỹ thuật, tất cả các biến thể này đều làm giống nhau. Vì vậy, đó là vấn đề sở thích cá nhân và thẩm mỹ.

<br>

> ---

**📌 `var` thay vì `let`**

Trong các tập lệnh cũ hơn, bạn cũng có thể tìm thấy một từ khóa khác: `var` thay vì `let`:

```js
      var message = 'Hello';
```

Từ khóa `var` là *gần như* giống như `let`. Nó cũng tuyên bố một biến, nhưng theo một cách hơi khác, "old-school" fashion.

Có sự khác biệt tinh tế giữa `let` và `var`, nhưng chúng chưa quan trọng đối với chúng ta. Chúng ta sẽ đề cập chi tiết về chúng sau, trong chương `var`.

> ---

<br>

## Một sự tương tự ngoài đời thực

Chúng ta có thể dễ dàng nắm bắt khái niệm "biến" nếu chúng ta tưởng tượng nó như một "hộp" cho dữ liệu, với nhãn dán có tên duy nhất trên đó.

Chẳng hạn, biến `message` có thể được tưởng tượng như một hộp có nhãn `"message"` với giá trị `"Xin chào!"` trong đó:

![](variable.png)

Chúng tôi có thể đặt bất kỳ giá trị vào hộp.

Ngoài ra chúng ta có thể thay đổi nó. Giá trị có thể được thay đổi nhiều lần nếu cần:

```js
      let message;

      message = 'Hello!';

      message = 'World!'; // value changed

      alert(message);
```

Khi giá trị được thay đổi, dữ liệu cũ sẽ bị xóa khỏi biến:

![](variable-change.png)

Chúng ta cũng có thể khai báo hai biến và sao chép dữ liệu từ cái này sang cái kia.

```js
      let hello = 'Hello world!';

      let message;

      // copy 'Hello world' from hello into message
      message = hello;

      // now two variables hold the same data
      alert(hello); // Hello world!
      alert(message); // Hello world!
```

<br>

> ---

**📌 Functional languages**

Thật thú vị khi biết rằng cũng tồn tại các ngôn ngữ lập trình [functional](https://en.wikipedia.org/wiki/Feftal_programming) cấm thay đổi giá trị biến. Ví dụ: [Scala](http://www.scala-lang.org/) hoặc [Erlang](http://www.erlang.org/).

Trong các ngôn ngữ như vậy, một khi giá trị được lưu trữ "trong hộp", nó sẽ tồn tại mãi mãi. Nếu chúng ta cần lưu trữ một cái gì đó khác, ngôn ngữ buộc chúng ta phải tạo một hộp mới (khai báo một biến mới). Chúng ta không thể tái sử dụng cái cũ.

Mặc dù có vẻ hơi kỳ lạ ngay từ cái nhìn đầu tiên, những ngôn ngữ này hoàn toàn có khả năng phát triển nghiêm túc. Hơn thế nữa, có những lĩnh vực như tính toán song song trong đó giới hạn này mang lại lợi ích nhất định. Nghiên cứu một ngôn ngữ như vậy (ngay cả khi không có kế hoạch sử dụng nó sớm) được khuyến khích để mở rộng tâm trí.

> ---

<br>

## Đặt tên biến

Có hai giới hạn cho một tên biến trong JavaScript:

1. Tên chỉ được chứa các chữ cái, chữ số, ký hiệu `$` và `_`.
2. Ký tự đầu tiên không được là chữ số.

Tên hợp lệ, ví dụ:

```js
      let userName;
      let test123;
```

Khi tên chứa nhiều từ, [camelCase](https://en.wikipedia.org/wiki/CamelCase) thường được sử dụng. Đó là: các từ đi nối tiếp nhau, mỗi từ bắt đầu bằng chữ in hoa: `myVeryLongName`.

Điều thú vị - ký hiệu đô la `'$'` và dấu gạch dưới `'_'` cũng có thể được sử dụng trong tên. Chúng là những biểu tượng thông thường, giống như chữ cái, không có bất kỳ ý nghĩa đặc biệt nào.

Những tên này là hợp lệ:

```js
      let $ = 1; // declared a variable with the name "$"
      let _ = 2; // and now a variable with the name "_"

      alert($ + _); // 3
```

Ví dụ về tên biến không chính xác:

```js
      let 1a; // cannot start with a digit

      let my-name; // a hyphen '-' is not allowed in the name
```

<br>

> ---

**📌 Trường hợp quan trọng**

Các biến có tên `apple` và `AppLE` - là hai biến khác nhau.

> ---

<br>
<br>

> ---

**📌 Các chữ cái không phải tiếng Anh được cho phép, nhưng không được đề xuất**

Có thể sử dụng bất kỳ ngôn ngữ nào, bao gồm cả chữ cái cyrillic hoặc thậm chí chữ tượng hình, như thế này:

```js
      let имя = '...';
      let 我 = '...';
```

Về mặt kỹ thuật, không có lỗi ở đây, những tên như vậy được cho phép, nhưng có một truyền thống quốc tế để sử dụng tiếng Anh trong tên biến. Ngay cả khi chúng tôi đang viết một kịch bản nhỏ, nó có thể có một cuộc sống lâu dài phía trước. Mọi người từ các quốc gia khác có thể cần phải đọc nó một thời gian.

> ---

<br>
<br>

> ---

**📌 Tên dành riêng**

Có [danh sách các từ dành riêng](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Lexical_grammar#Keywords), không thể được sử dụng làm tên biến, vì chúng được sử dụng bởi ngôn ngữ

Ví dụ: các từ `let`, `class`, `return`, `function` được bảo lưu.

Mã dưới đây đưa ra một lỗi cú pháp:

```js
      let let = 5; // can't name a variable "let", error!
      let return = 5; // also can't name it "return", error!
```

> ---

<br>
<br>

> ---

**📌 Một bài tập không có `sử dụng nghiêm ngặt`**

Thông thường, chúng ta cần xác định một biến trước khi sử dụng nó. Nhưng trong thời đại cũ, về mặt kỹ thuật có thể tạo ra một biến bằng cách gán giá trị đơn thuần, không có `let`. Điều này vẫn hoạt động ngay bây giờ nếu chúng ta không đặt 'sử dụng nghiêm ngặt'. Các hành vi được giữ cho tương thích với các kịch bản cũ.

```js
      // note: no "use strict" in this example

      num = 5; // the variable "num" is created if didn't exist

      alert(num); // 5
```

Đó là một thực tiễn xấu, nó sẽ báo lỗi ở chế độ nghiêm ngặt:

```js
      "use strict";

      num = 5; // error: num is not defined
```

> ---

<br>

## Hằng

Để khai báo một biến không thay đổi, người ta có thể sử dụng `const` thay vì` let`:

```js
      const myBirthday = '18.04.1982';
```

Các biến được khai báo sử dụng `const` được gọi là "hằng (constants)". Họ không thể thay đổi. Một nỗ lực để làm điều đó sẽ gây ra lỗi:

```js
      const myBirthday = '18.04.1982';

      myBirthday = '01.01.2001'; // error, can't reassign the constant!
```

Khi một lập trình viên chắc chắn rằng biến không bao giờ thay đổi, họ có thể sử dụng `const` để đảm bảo nó và cũng để hiển thị rõ ràng thực tế đó cho mọi người.


### Hằng số in hoa

Có một thực tiễn phổ biến để sử dụng các hằng số làm bí danh cho các giá trị khó nhớ được biết trước khi thực hiện.

Các hằng số như vậy được đặt tên bằng chữ in hoa và dấu gạch dưới.

Như thế này:

```js
      const COLOR_RED = "#F00";
      const COLOR_GREEN = "#0F0";
      const COLOR_BLUE = "#00F";
      const COLOR_ORANGE = "#FF7F00";

      // ...when we need to pick a color
      let color = COLOR_ORANGE;
      alert(color); // #FF7F00
```

Lợi ích:

- `COLOR_ORANGE` dễ nhớ hơn nhiều so với `"#FF7F00"`.
- Việc nhập nhầm vào `"#FF7F00"` dễ dàng hơn nhiều so với trong`COLOR_ORANGE`.
- Khi đọc mã, `COLOR_ORANGE` có ý nghĩa hơn nhiều so với `#FF7F00`.

Khi nào chúng ta nên sử dụng chữ viết hoa cho một hằng số, và khi nào chúng ta nên đặt tên chúng bình thường? Hãy làm rõ điều đó.

Trở thành "hằng số" chỉ có nghĩa là giá trị không bao giờ thay đổi. Nhưng có những hằng số được biết trước khi thực thi (execution) (như giá trị thập lục phân (hexadecimal value) cho màu đỏ) và có những hằng số *được tính (calculated)* trong thời gian chạy, trong khi thực thi (execution), nhưng không thay đổi sau khi gán.

Ví dụ:

```js
      const pageLoadTime = /* time taken by a webpage to load */;
```

Giá trị của `pageLoadTime` không được biết trước khi tải trang, vì vậy nó được đặt tên bình thường. Nhưng nó vẫn là một hằng số, bởi vì nó không thay đổi sau khi gán.

Nói cách khác, hằng số viết hoa (capital-named) chỉ được sử dụng làm bí danh cho các giá trị "mã hóa cứng (hard-coded)".  

## Đặt tên đúng

Nói về các biến, có một điều cực kỳ quan trọng.

Hãy đặt tên cho các biến hợp lý. Hãy dành thời gian để suy nghĩ nếu cần.

Đặt tên biến là một trong những kỹ năng quan trọng và phức tạp nhất trong lập trình. Nhìn lướt qua tên biến có thể tiết lộ mã nào được viết bởi người mới bắt đầu và mã nào được phát triển bởi nhà phát triển có kinh nghiệm.

Trong một dự án thực tế, phần lớn thời gian được dành cho việc sửa đổi và mở rộng cơ sở mã hiện có, thay vì viết một cái gì đó hoàn toàn tách biệt khỏi đầu. Và khi chúng tôi trở lại mã sau một thời gian làm việc khác, việc tìm kiếm thông tin được dán nhãn tốt sẽ dễ dàng hơn nhiều. Hay nói cách khác, khi các biến có tên tốt.

Hãy dành một chút thời gian suy nghĩ về tên đúng cho một biến trước khi khai báo nó. Điều này sẽ trả ơn bạn rất nhiều.

Một số quy tắc tốt để tuân theo là:

- Sử dụng tên người có thể đọc được như `userName` hoặc `shoppingCart`.
- Tránh xa các chữ viết tắt hoặc tên ngắn như `a`,` b`, `c`, trừ khi bạn thực sự biết bạn đang làm gì.
- Làm cho tên mô tả tối đa và súc tích. Ví dụ về tên xấu là `data` và `value`. Một cái tên như vậy không nói gì. Chỉ có thể sử dụng chúng nếu nó đặc biệt rõ ràng trong bối cảnh dữ liệu hoặc giá trị có nghĩa là gì.
- Đồng ý với các điều khoản trong nhóm của bạn và trong tâm trí của riêng bạn. Nếu khách truy cập trang web được gọi là "user" thì chúng ta nên đặt tên cho các biến liên quan như `currentUser` hoặc `newUser`, nhưng không phải `currentVisitor` hoặc `newManInTown`.

Nghe có vẻ đơn giản? Thật vậy, nhưng tạo ra những cái tên mô tả và súc tích tốt trong thực tế thì không. Go for it.

<br>

> ---

** Tái sử dụng hay tạo?**

Và lưu ý cuối cùng. Có một số lập trình viên lười biếng, thay vì khai báo một biến mới, có xu hướng sử dụng lại các biến hiện có.

Kết quả là, biến giống như một hộp nơi mọi người ném những thứ khác nhau mà không thay đổi nhãn dán. Những gì bên trong nó bây giờ? Ai biết... Chúng ta cần đến gần hơn và kiểm tra.

Một lập trình viên như vậy tiết kiệm một chút khi khai báo biến, nhưng mất nhiều hơn mười lần khi gỡ lỗi mã.

Một biến phụ là tốt, không xấu.

Các JavaScript minifiers và trình duyệt hiện đại tối ưu hóa mã đủ tốt, do đó nó sẽ không tạo ra các vấn đề về hiệu suất. Sử dụng các biến khác nhau cho các giá trị khác nhau thậm chí có thể giúp động cơ (engine) tối ưu hóa.

> ---

<br>

## Tóm lược

Chúng ta có thể khai báo các biến để lưu trữ dữ liệu. Điều đó có thể được thực hiện bằng cách sử dụng `var` hoặc `let` hoặc `const`.

- `let` - là một khai báo biến hiện đại. Mã phải ở chế độ nghiêm ngặt để sử dụng `let` trong Chrome (V8).
- `var` - là một khai báo biến old-school. Thông thường chúng ta hoàn toàn không sử dụng nó, nhưng chúng ta sẽ đề cập đến những khác biệt tinh tế từ `let` trong chương `var`, chỉ trong trường hợp bạn cần chúng.
- `const` - giống như `let`, nhưng giá trị của biến không thể thay đổi.

Các biến nên được đặt tên theo cách cho phép chúng ta dễ dàng hiểu những gì bên trong.

1 result is available, use up and down arrow keys to navigate.
