# Strings

Trong JavaScript, dữ liệu văn bản được lưu trữ dưới dạng chuỗi. Không có kiểu riêng cho một ký tự.

Định dạng bên trong cho các chuỗi luôn là [UTF-16](https://en.wikipedia.org/wiki/UTF-16), nó không được gắn với mã hóa trang.

## Quotes

Hãy nhớ lại các kiểu trích dẫn.

Các chuỗi có thể được đặt trong một dấu ngoặc đơn, dấu ngoặc kép hoặc backticks:

```js
      let single = 'single-quoted';
      let double = "double-quoted";

      let backticks = `backticks`;
```

Trích dẫn đơn và đôi về cơ bản là giống nhau. Tuy nhiên, Backticks cho phép chúng ta nhúng bất kỳ biểu thức nào vào chuỗi, bao gồm cả các lệnh gọi hàm:

```js
      function sum(a, b) {
        return a + b;
      }

      alert(`1 + 2 = ${sum(1, 2)}.`); // 1 + 2 = 3.
```

Một ưu điểm khác của việc sử dụng backticks là chúng cho phép một chuỗi trải dài trên nhiều dòng:

```js
let guestList = `Guests:
* John
* Pete
* Mary
`;

alert(guestList); // a list of guests, multiple lines
```

Nếu chúng ta cố gắng sử dụng dấu ngoặc đơn hoặc dấu ngoặc kép theo cùng một cách, sẽ là một lỗi:

```js
let guestList = "Guests:  // Error: Unexpected token ILLEGAL
  * John";
```

Các trích dẫn đơn và kép xuất phát từ thời cổ đại của việc tạo ra ngôn ngữ khi nhu cầu về chuỗi đa dòng không được tính đến. Backticks xuất hiện muộn hơn nhiều và do đó linh hoạt hơn.

Backticks cũng cho phép chúng ta chỉ định "template function" trước backtick đầu tiên. Cú pháp là: <code>func`string`</code>. Hàm `func` được gọi tự động, nhận chuỗi và các biểu thức nhúng và có thể xử lý chúng. Bạn có thể đọc thêm về nó trong [docs](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Template_literals#Tagged_template_literals). Đây được gọi là "tagged templates". Tính năng này giúp dễ dàng hơn để bọc chuỗi thành custom templating hoặc functionality khác, nhưng nó hiếm khi được sử dụng.


## Ký tự đặc biệt (Special characters)

Vẫn có thể tạo các chuỗi nhiều dòng với các dấu ngoặc đơn bằng cách sử dụng cái gọi là "ký tự dòng mới", được viết là `\n`, biểu thị ngắt dòng:

```js
      let guestList = "Guests:\n * John\n * Pete\n * Mary";

      alert(guestList); // a multiline list of guests
```

Ví dụ, hai dòng này mô tả giống nhau:

```js
alert( "Hello\nWorld" ); // two lines using a "newline symbol"

// two lines using a normal newline and backticks
alert( `Hello
World` );
```

Có những characters "đặc biệt" khác ít phổ biến hơn. Đây là danh sách:

| Character    | Description   |
|--------------|---------------|
|`\b`          |Backspace      |
|`\f`          |Form feed      |
|`\n`          |New line       |
|`\r`          |Carriage return|
|`\t`          |Tab            |
|`\uNNNN`      |A unicode symbol with the hex code `NNNN`, for instance `\u00A9` -- is a unicode for the copyright symbol `©`. It must be exactly 4 hex digits. |
|`\u{NNNNNNNN}`|Some rare characters are encoded with two unicode symbols, taking up to 4 bytes. This long unicode requires braces around it.|

Ví dụ với unicode:

```js
      alert( "\u00A9" ); // ©
      alert( "\u{20331}" ); // 佫, a rare chinese hieroglyph (long unicode)
      alert( "\u{1F60D}" ); // 😍, a smiling face symbol (another long unicode)
```

Tất cả các ký tự đặc biệt bắt đầu bằng ký tự dấu gạch chéo ngược `\`. Nó cũng được gọi là "escape character".

Chúng ta cũng sẽ sử dụng nó nếu chúng ta muốn chèn một trích dẫn vào chuỗi.

Ví dụ:

```js
      alert( 'I\'m the Walrus!' ); // I'm the Walrus!
```

Như bạn có thể thấy, chúng ta phải thêm vào trích dẫn bên trong bằng dấu gạch chéo ngược `\'`, bởi vì nếu không nó sẽ chỉ ra kết thúc chuỗi.

Tất nhiên, điều đó chỉ nhắm vào các trích dẫn giống như các trích dẫn kèm theo. Vì vậy, như một giải pháp thanh lịch hơn, chúng ta có thể chuyển sang dấu ngoặc kép hoặc backticks thay thế:

```js
      alert( `I'm the Walrus!` ); // I'm the Walrus!
```

Lưu ý rằng dấu gạch chéo ngược `\` phục vụ cho việc đọc chính xác chuỗi bằng JavaScript, sau đó biến mất. Chuỗi trong bộ nhớ không có `\`. Bạn có thể thấy rõ điều đó trong `alert` từ các ví dụ trên.

Nhưng điều gì sẽ xảy ra nếu chúng ta cần hiển thị dấu gạch chéo ngược thực tế `\` trong chuỗi?

Điều đó có thể, nhưng chúng ta cần nhân đôi nó như `\\`:

```js
      alert( `The backslash: \\` ); // The backslash: \
```

## Chiều dài chuỗi


Thuộc tính `length` để lấy độ dài chuỗi:

```js
      alert( `My\n`.length ); // 3
```

Lưu ý rằng `\n` là một ký tự "đặc biệt" đơn lẻ, vì vậy độ dài thực sự là `3`.

<br>

> ---

**📌 `length` là một thuộc tính**

Những người có nền tảng trong một số ngôn ngữ khác đôi khi gõ nhầm bằng cách gọi `str.length()` thay vì chỉ `str.length`. Điều đó không làm việc.

Xin lưu ý rằng `str.length` là một thuộc tính số, không phải là hàm. Không cần thêm dấu ngoặc sau nó.

> ---

<br>

## Truy cập các ký tự (Accessing characters)

Để có được một ký tự ở vị trí `pos`, sử dụng dấu ngoặc vuông `[pos]` hoặc gọi phương thức [str.charAt(pos)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/charAt). Ký tự đầu tiên bắt đầu từ vị trí 0:

```js
      let str = `Hello`;

      // the first character
      alert( str[0] ); // H
      alert( str.charAt(0) ); // H

      // the last character
      alert( str[str.length - 1] ); // o
```

Dấu ngoặc vuông là một cách hiện đại để có được một ký tự, trong khi `charAt` tồn tại chủ yếu vì lý do lịch sử.

Sự khác biệt duy nhất giữa chúng là nếu không tìm thấy ký tự nào, `[]` trả về `undefined` và `charAt` trả về một chuỗi rỗng:

```js
      let str = `Hello`;

      alert( str[1000] ); // undefined
      alert( str.charAt(1000) ); // '' (an empty string)
```

Chúng ta cũng có thể lặp lại các ký tự bằng cách sử dụng `for..of`:

```js
      for (let char of "Hello") {
        alert(char); // H,e,l,l,o (char becomes "H", then "e", then "l" etc)
      }
```

## Chuỗi là bất biến (Strings are immutable)

Chuỗi không thể thay đổi trong JavaScript. Không thể thay đổi một character.

Hãy thử để thấy rằng nó không hoạt động:

```js
      let str = 'Hi';

      str[0] = 'h'; // error
      alert( str[0] ); // doesn't work
```

Cách giải quyết thông thường là tạo một chuỗi hoàn toàn mới và gán nó cho `str` thay vì chuỗi cũ.

Ví dụ:

```js
      let str = 'Hi';

      str = 'h' + str[1];  // replace the string

      alert( str ); // hi
```

Trong các phần sau chúng ta sẽ thấy nhiều ví dụ về điều này.

## Thay đổi trường hợp (Changing the case)

Các phương thức [toLowerCase()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/toLowerCase) và [toUpperCase()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/toUpperCase) thay đổi trường hợp:

```js
      alert( 'Interface'.toUpperCase() ); // INTERFACE
      alert( 'Interface'.toLowerCase() ); // interface
```

Hoặc, nếu chúng ta muốn một ký tự duy nhất trở thành chữ thường:

```js
      alert( 'Interface'[0].toLowerCase() ); // 'i'
```

## Tìm kiếm một chuỗi con (Searching for a substring)

Có nhiều cách để tìm chuỗi con trong chuỗi.

### str.indexOf

Phương thức đầu tiên là [str.indexOf(substr, pos)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/indexOf).

Nó tìm kiếm `substr` trong `str`, bắt đầu từ vị trí đã cho `pos` và trả về vị trí tìm thấy kết quả khớp hoặc `-1` nếu không tìm thấy gì.

Ví dụ:

```js
      let str = 'Widget with id';

      alert( str.indexOf('Widget') ); // 0, because 'Widget' is found at the beginning
      alert( str.indexOf('widget') ); // -1, not found, the search is case-sensitive

      alert( str.indexOf("id") ); // 1, "id" is found at the position 1 (..idget with id)
```

Tham số tùy chọn thứ hai cho phép chúng ta tìm kiếm bắt đầu từ vị trí đã cho.

Chẳng hạn, lần xuất hiện đầu tiên của `"id"` là ở vị trí `1`. Để tìm kiếm sự xuất hiện tiếp theo, hãy bắt đầu tìm kiếm từ vị trí `2`:

```js
      let str = 'Widget with id';

      alert( str.indexOf('id', 2) ) // 12
```


Nếu chúng ta quan tâm đến tất cả các lần xuất hiện, chúng ta có thể chạy `indexOf` trong một vòng lặp. Mỗi cuộc gọi mới được thực hiện với vị trí sau lần tìm thấy trước:


```js
      let str = 'As sly as a fox, as strong as an ox';

      let target = 'as'; // let's look for it

      let pos = 0;
      while (true) {
        let foundPos = str.indexOf(target, pos);
        if (foundPos == -1) break;

        alert( `Found at ${foundPos}` );
        pos = foundPos + 1; // continue the search from the next position
      }
```

Thuật toán tương tự có thể được trình bày ngắn hơn:

```js
      let str = "As sly as a fox, as strong as an ox";
      let target = "as";

      let pos = -1;
      while ((pos = str.indexOf(target, pos + 1)) != -1) {
        alert( pos );
      }
```

<br>

> ---

**📌 `str.lastIndexOf(substr, position)`**

Ngoài ra còn có một phương thức tương tự [str.lastIndexOf(substr, position)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/lastIndexOf) bắt đầu tìm kiếm từ cuối của một chuỗi.

Nó sẽ liệt kê các lần xuất hiện theo thứ tự ngược lại.

> ---

<br>

Có một chút bất tiện với `indexOf` trong bài kiểm tra `if`. Chúng ta không thể đặt nó trong `if` như thế này:

```js
      let str = "Widget with id";

      if (str.indexOf("Widget")) {
          alert("We found it"); // doesn't work!
      }
```

`alert` trong ví dụ trên không hiển thị vì `str.indexOf("Widget")` trả về `0` (có nghĩa là nó tìm thấy kết quả khớp ở vị trí bắt đầu). Đúng, nhưng `if` coi `0` là `false`.

Vì vậy, chúng ta thực sự nên kiểm tra `-1`, như thế này:

```js
      let str = "Widget with id";

      if (str.indexOf("Widget") != -1) {
          alert("We found it"); // works now!
      }
```

<br>

> ---

**📌 The bitwise NOT trick**

Một trong những thủ thuật cũ được sử dụng ở đây là toán tử [bitwise NOT](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Bitwise_Operators#Bitwise_NOT) `~`. Nó chuyển đổi số thành số nguyên 32 bit (loại bỏ phần thập phân nếu tồn tại) và sau đó đảo ngược tất cả các bit trong biểu diễn nhị phân của nó.

Đối với số nguyên 32 bit, lệnh gọi `~n` có nghĩa chính xác giống như `-(n+1)` (do định dạng IEEE-754).

Ví dụ:

```js
      alert( ~2 ); // -3, the same as -(2+1)
      alert( ~1 ); // -2, the same as -(1+1)
      alert( ~0 ); // -1, the same as -(0+1)
      alert( ~-1 ); // 0, the same as -(-1+1)
```

Như chúng ta có thể thấy, `~n` chỉ bằng 0 nếu `n == -1`.

Vì vậy, phép thử `if ( ~str.indexOf("...") )` là đúng rằng kết quả của `indexOf` không phải là `-1`. Nói cách khác, khi có một tìm thấy.

Mọi người sử dụng nó để rút ngắn kiểm tra `indexOf`:

```js
      let str = "Widget";

      if (~str.indexOf("Widget")) {
        alert( 'Found it!' ); // works
      }
```

Thông thường không nên sử dụng các tính năng ngôn ngữ theo cách không rõ ràng, nhưng thủ thuật đặc biệt này được sử dụng rộng rãi trong mã cũ, vì vậy chúng ta nên hiểu nó.

Chỉ cần nhớ: `if (~str.indexOf(...))` đọc là "nếu tìm thấy".

> ---

<br>

### includes, startsWith, endsWith

Phương thức hiện đại hơn [str.includes(substr, pos)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/includes) trả về `true/false` về việc `str` có chứa `substr` bên trong không.

Đó là lựa chọn đúng đắn nếu chúng ta cần kiểm tra tìm thấy, nhưng không cần vị trí của nó:

```js
      alert( "Widget with id".includes("Widget") ); // true

      alert( "Hello".includes("Bye") ); // false
```

Đối số tùy chọn thứ hai của `str.includes` là vị trí để bắt đầu tìm kiếm từ đó:

```js
      alert( "Midget".includes("id") ); // true
      alert( "Midget".includes("id", 3) ); // false, from position 3 there is no "id"
```

Các phương thức [str.startsWith](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/startsWith) và [str.endsWith](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/endsWith) làm chính xác những gì chúng nói:

```js
      alert( "Widget".startsWith("Wid") ); // true, "Widget" starts with "Wid"
      alert( "Widget".endsWith("get") );   // true, "Widget" ends with "get"
```

## Bắt một chuỗi con (Getting a substring)

Có 3 phương thức trong JavaScript để bắt một chuỗi con: `substring`, `substr` và `slice`.

**`str.slice(start [, end])`**

Trả về một phần của chuỗi từ `start` đến (nhưng không bao gồm) `end`.

Ví dụ:

```js
      let str = "stringify";
      alert( str.slice(0, 5) ); // 'strin', the substring from 0 to 5 (not including 5)
      alert( str.slice(0, 1) ); // 's', from 0 to 1, but not including 1, so only character at 0
```

Nếu không có đối số thứ hai, thì `slice` sẽ đi đến cuối chuỗi:

```js
      let str = "stringify";
      alert( str.slice(2) ); // ringify, from the 2nd position till the end
```

Các giá trị âm cho `start/end` cũng có thể. Chúng có nghĩa là vị trí được tính từ cuối chuỗi:

```js
      let str = "stringify";

      // start at the 4th position from the right, end at the 1st from the right
      alert( str.slice(-4, -1) ); // gif
```

**`str.substring(start [, end])`**

Trả về phần của chuỗi *giữa* `start` và `end`.

Điều này gần giống như `slice`, nhưng nó cho phép `start` lớn hơn `end`.

Ví dụ:

```js
      let str = "stringify";

      // these are same for substring
      alert( str.substring(2, 6) ); // "ring"
      alert( str.substring(6, 2) ); // "ring"

      // ...but not for slice:
      alert( str.slice(2, 6) ); // "ring" (the same)
      alert( str.slice(6, 2) ); // "" (an empty string)
```

Các đối số âm (không giống như slice) không được hỗ trợ, chúng được coi là `0`.


**`str.substr(start [, length])`**

Trả về một phần của chuỗi từ `start`, với `length` đã cho.

Ngược lại với các phương thức trước đó, phương thức này cho phép chúng ta chỉ định `length` thay vì vị trí kết thúc:

```js
      let str = "stringify";
      alert( str.substr(2, 4) ); // ring, from the 2nd position get 4 characters
```

Đối số đầu tiên có thể là âm, để tính từ cuối:

```js
      let str = "stringify";
      alert( str.substr(-4, 2) ); // gi, from the 4th position get 2 characters
```

Hãy tóm tắt lại các phương pháp này để tránh mọi nhầm lẫn:

| method                  | selects...                                           | negatives               |
|-------------------------|------------------------------------------------------|-------------------------|
| `slice(start, end)`     | from `start` to `end` (not including `end`)          | allows negatives        |
| `substring(start, end)` | between `start` and `end`                            | negative values mean `0`|
| `substr(start, length)` | from `start` get `length` characters                 | allows negative `start` |

<br>

> ---

**📌 Chọn cái nào?**

Tất cả đều có thể làm được việc. Chính thức, `substr` có một nhược điểm nhỏ: nó được mô tả không phải trong đặc tả JavaScript cốt lõi, mà trong Phụ lục B, bao gồm các tính năng chỉ dành cho trình duyệt tồn tại chủ yếu vì lý do lịch sử. Vì vậy, môi trường không có trình duyệt có thể không hỗ trợ nó. Nhưng trong thực tế nó hoạt động ở khắp mọi nơi.

Tác giả tìm thấy chính chúng bằng cách sử dụng `slice` gần như mọi lúc.

> ---

<br>

## So sánh chuỗi (Comparing strings)

Như chúng ta đã biết từ chương **comparison**, các chuỗi được so sánh theo từng ký tự theo thứ tự bảng chữ cái.

Mặc dù, có một số điều kỳ lạ.

1. Một chữ cái viết thường luôn lớn hơn chữ hoa:

    ```js
    alert( 'a' > 'Z' ); // true
    ```

2. Các chữ cái có dấu phụ là "không theo thứ tự":

    ```js
    alert( 'Österreich' > 'Zealand' ); // true
    ```

    Điều này có thể dẫn đến kết quả lạ nếu chúng ta sắp xếp các tên quốc gia này. Thông thường mọi người sẽ mong đợi `Zealand` sẽ đến sau `Österreich` trong danh sách.

Để hiểu điều gì xảy ra, hãy xem lại miêu tả bên trong của chuỗi trong JavaScript.

Tất cả các chuỗi được mã hóa bằng [UTF-16](https://en.wikipedia.org/wiki/UTF-16). Đó là: mỗi ký tự có một mã số tương ứng. Có các phương thức đặc biệt cho phép lấy ký tự cho mã và trả lại.

**`str.codePointAt(pos)`**

Trả về mã cho ký tự ở vị trí `pos`:

```js
      // different case letters have different codes
      alert( "z".codePointAt(0) ); // 122
      alert( "Z".codePointAt(0) ); // 90
```

**`String.fromCodePoint(code)`**

Tạo một ký tự bằng số `code`

```js
      alert( String.fromCodePoint(90) ); // Z
```

Chúng ta cũng có thể thêm các ký tự unicode bằng mã của chúng bằng cách sử dụng `\u` theo sau là mã hex:

```js
      // 90 is 5a in hexadecimal system
      alert( '\u005a' ); // Z
```

Bây giờ chúng ta hãy xem các ký tự có mã `65..220` (bảng chữ cái Latinh và thêm một chút) bằng cách tạo một chuỗi của chúng:

```js
      let str = '';

      for (let i = 65; i <= 220; i++) {
        str += String.fromCodePoint(i);
      }
      alert( str );
      // ABCDEFGHIJKLMNOPQRSTUVWXYZ[\]^_`abcdefghijklmnopqrstuvwxyz{|}~
      // ¡¢£¤¥¦§¨©ª«¬­®¯°±²³´µ¶·¸¹º»¼½¾¿ÀÁÂÃÄÅÆÇÈÉÊËÌÍÎÏÐÑÒÓÔÕÖ×ØÙÚÛÜ
```

Nhìn xem? Ký tự viết hoa đi trước, sau đó là một vài ký tự đặc biệt, rồi ký tự viết thường.

Bây giờ nó trở nên rõ ràng tại sao `a > Z`.

Các ký tự được so sánh bằng mã số của chúng. Mã lớn hơn có nghĩa là ký tự lớn hơn. Mã cho `a` (97) lớn hơn mã cho `Z` (90).

- Tất cả các chữ cái viết thường đi sau chữ in hoa vì mã của chúng lớn hơn.
- Một số chữ cái như `Ö` đứng ngoài bảng chữ cái chính. Ở đây, mã của nó lớn hơn bất cứ thứ gì từ `a` đến `z`.

### So sánh đúng (Correct comparisons)

Thuật toán "right" để thực hiện so sánh chuỗi phức tạp hơn vẻ ngoài của nó, bởi vì bảng chữ cái là khác nhau cho các ngôn ngữ khác nhau. Chữ cái trông giống nhau có thể được đặt khác nhau trong các bảng chữ cái khác nhau.

Vì vậy, trình duyệt cần biết ngôn ngữ để so sánh.

May mắn thay, tất cả các trình duyệt hiện đại (IE10 - yêu cầu thư viện bổ sung [Intl.JS](https://github.com/andyearnshaw/Intl.js/)) hỗ trợ tiêu chuẩn quốc tế hóa [ECMA 402](http://www.ecma-international.org/ecma-402/1.0/ECMA-402.pdf).

Nó cung cấp một phương thức đặc biệt để so sánh các chuỗi trong các ngôn ngữ khác nhau, tuân theo các quy tắc của chúng.

Cuộc gọi [str.localeCompare(str2)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/localeCompare):

- Trả về `1` nếu `str` lớn hơn `str2` theo quy tắc ngôn ngữ.
- Trả về `-1` nếu `str` nhỏ hơn `str2`.
- Trả về `0` nếu chúng bằng nhau.

Ví dụ:

```js
      alert( 'Österreich'.localeCompare('Zealand') ); // -1
```

Phương thức này thực sự có hai đối số bổ sung được chỉ định trong [tài liệu](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/localeCompare), cho phép nó chỉ định ngôn ngữ (theo mặc định được lấy từ environment) và thiết lập các quy tắc bổ sung như phân biệt chữ hoa chữ thường hoặc nên để `"a"` và `"á"` được coi là như nhau, v.v.

## Nội bộ, Unicode (Internals, Unicode)

<br>

> ---

**📌 Kiến thức nâng cao (Advanced knowledge)**

Phần này đi sâu hơn vào bên trong chuỗi. Kiến thức này sẽ hữu ích cho bạn nếu bạn có kế hoạch đối phó với biểu tượng cảm xúc (emoji), toán học hiếm của các ký tự chữ tượng hình hoặc các biểu tượng hiếm khác.

Bạn có thể bỏ qua phần này nếu bạn không có kế hoạch hỗ trợ chúng.

> ---

<br>

### Cặp thay thế (Surrogate pairs)

Hầu hết các biểu tượng có 2 byte mã. Các chữ cái trong hầu hết các ngôn ngữ châu Âu, số và thậm chí hầu hết các chữ tượng hình đều có biểu diễn 2 byte.

Nhưng 2 byte chỉ cho phép kết hợp 65536 và điều đó là không đủ cho mọi biểu tượng có thể. Vì vậy, các ký hiệu hiếm được mã hóa bằng một cặp ký tự 2 byte được gọi là "một cặp thay thế (a surrogate pair)".

Độ dài của các ký hiệu như vậy là `2`:

```js
      alert( '𝒳'.length ); // 2, MATHEMATICAL SCRIPT CAPITAL X
      alert( '😂'.length ); // 2, FACE WITH TEARS OF JOY
      alert( '𩷶'.length ); // 2, a rare chinese hieroglyph
```

Lưu ý rằng các cặp thay thế không tồn tại tại thời điểm JavaScript được tạo và do đó ngôn ngữ không được xử lý chính xác!

Chúng ta thực sự có một ký hiệu duy nhất trong mỗi chuỗi ở trên, nhưng `length` hiển thị độ dài `2`.

`String.fromCodePoint` và `str.codePointAt` là một vài phương thức hiếm hoi xử lý các cặp thay thế đúng. Chúng đã xuất hiện gần đây trong ngôn ngữ. Trước chúng, chỉ có [String.fromCharCode](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/fromCharCode) và [str.charCodeAt](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/charCodeAt). Các phương thức này thực sự giống như `fromCodePoint/codePointAt`, nhưng không hoạt động với các cặp thay thế.

Nhưng, ví dụ, nhận được một symbol có thể khó khăn, bởi vì các cặp thay thế được coi là hai ký tự:

```js
      alert( '𝒳'[0] ); // strange symbols...
      alert( '𝒳'[1] ); // ...pieces of the surrogate pair
```

Lưu ý rằng các mảnh của cặp thay thế không có ý nghĩa mà không có nhau. Vì vậy, các alerts trong ví dụ trên thực sự hiển thị rác.

Về mặt kỹ thuật, các cặp thay thế cũng có thể được phát hiện bằng mã của chúng: nếu một ký tự có mã trong khoảng `0xd800..0xdbff`, thì đó là phần đầu tiên của cặp thay thế. Ký tự tiếp theo (phần thứ hai) phải có mã trong khoảng `0xdc00..0xdfff`. Các khoảng này được dành riêng cho các cặp thay thế theo tiêu chuẩn.

Trong trường hợp trên:

```js
      // charCodeAt is not surrogate-pair aware, so it gives codes for parts

      alert( '𝒳'.charCodeAt(0).toString(16) ); // d835, between 0xd800 and 0xdbff
      alert( '𝒳'.charCodeAt(1).toString(16) ); // dcb3, between 0xdc00 and 0xdfff
```

Bạn sẽ tìm thấy nhiều cách hơn để đối phó với các cặp thay thế sau trong chương **iterable**. Có lẽ có những thư viện đặc biệt cho điều đó, nhưng không có gì nổi tiếng đủ để đề xuất ở đây.

### Dấu phụ và chuẩn hóa (Diacritical marks and normalization)

Trong nhiều ngôn ngữ, có những symbols được tạo thành từ base character với một đánh dấu (mark) ở trên/dưới nó.

Chẳng hạn, chữ `a` có thể là ký tự cơ sở cho: `àáâäãåā`. Ký tự "tổng hợp" phổ biến nhất có mã riêng trong bảng UTF-16. Nhưng không phải tất cả trong số chúng, bởi vì có quá nhiều kết hợp có thể.

Để hỗ trợ các bố cục tùy ý, UTF-16 cho phép chúng ta sử dụng một số ký tự unicode. The base character và một hoặc nhiều "mark" characters để "trang trí" nó.

Chẳng hạn, nếu chúng ta có `S` theo sau là ký tự "chấm ở trên" đặc biệt (code `\u0307`), thì nó được hiển thị là Ṡ.

```js
      alert( 'S\u0307' ); // Ṡ
```

Nếu chúng ta cần một dấu bổ sung phía trên chữ cái (hoặc bên dưới nó) - không có vấn đề gì, chỉ cần thêm ký tự đánh dấu cần thiết.

Chẳng hạn, nếu chúng ta thêm một ký tự "chấm bên dưới" (code `\u0323`), thì chúng ta sẽ có "S với các dấu chấm ở trên và bên dưới": `Ṩ`.

Ví dụ:

```js
      alert( 'S\u0307\u0323' ); // Ṩ
```

Điều này cung cấp tính linh hoạt cao, nhưng cũng là một vấn đề thú vị: hai characters có thể trông giống nhau, nhưng được thể hiện bằng các thành phần unicode khác nhau.

Ví dụ:

```js
      alert( 'S\u0307\u0323' ); // Ṩ, S + dot above + dot below
      alert( 'S\u0323\u0307' ); // Ṩ, S + dot below + dot above

      alert( 'S\u0307\u0323' == 'S\u0323\u0307' ); // false
```

Để giải quyết vấn đề này, tồn tại thuật toán "chuẩn hóa unicode (unicode normalization)" đưa mỗi chuỗi về dạng "bình thường" duy nhất.

Nó được triển khai bởi [str.normalize()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/normalize).

```js
      alert( "S\u0307\u0323".normalize() == "S\u0323\u0307".normalize() ); // true
```

Thật buồn cười là trong tình huống của chúng ta, `normalize()` thực sự tập hợp một chuỗi gồm 3 ký tự thành một: `\u1e68` (S có hai dấu chấm).

```js
      alert( "S\u0307\u0323".normalize().length ); // 1

      alert( "S\u0307\u0323".normalize() == "\u1e68" ); // true
```

Trong thực tế, điều này không phải lúc nào cũng đúng. Lý do là biểu tượng `Ṩ` là "đủ phổ biến", vì vậy những người tạo UTF-16 đã đưa nó vào bảng chính và đưa mã cho nó.

Nếu bạn muốn tìm hiểu thêm về các quy tắc và biến thể chuẩn hóa - chúng được mô tả trong phần phụ lục của tiêu chuẩn Unicode: [Biểu mẫu chuẩn hóa Unicode](http://www.unicode.org/reports/tr15/), nhưng cho mục đích thiết thực nhất thông tin từ phần này là đủ.

## Tóm lược

- Có 3 loại trích dẫn. Backticks cho phép một chuỗi trải rộng nhiều dòng và nhúng biểu thức.
- Chuỗi trong JavaScript được mã hóa bằng UTF-16.
- Chúng ta có thể sử dụng các ký tự đặc biệt như `\n` và chèn các chữ cái bằng unicode của chúng bằng cách sử dụng `\u...`.
- Để bắt được một ký tự, sử dụng: `[]`.
- Để bắt được một chuỗi con, sử dụng: `slice` hoặc `substring`.
- Để viết thường/viết hoa một chuỗi, sử dụng: `toLowerCase/toUpperCase`.
- Để tìm chuỗi con, sử dụng: `indexOf` hoặc `includes/startsWith/endsWith` để kiểm tra đơn giản.
- Để so sánh các chuỗi theo ngôn ngữ, sử dụng: `localeCompare`, nếu không, chúng được so sánh bằng mã ký tự.

Có một số phương thức hữu ích khác trong chuỗi:

- `str.trim()` -- loại bỏ khoảng trắng ("trims") từ đầu và cuối chuỗi.
- `str.repeat(n)` -- lặp lại chuỗi `n` lần.
- ...và hơn thế nữa. Xem [hướng dẫn](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String) để biết chi tiết.

Chuỗi cũng có các phương thức để thực hiện tìm kiếm/thay thế bằng các biểu thức thông thường. Nhưng chủ đề đó xứng đáng có một chương riêng, vì vậy chúng ta sẽ trở lại vấn đề đó sau.
