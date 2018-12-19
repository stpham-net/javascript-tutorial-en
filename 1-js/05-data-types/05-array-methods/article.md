# Array methods

Mảng cung cấp rất nhiều phương thức. Để làm cho mọi thứ dễ dàng hơn, trong chương này, chúng được chia thành các nhóm.

## Add/remove items

Chúng ta đã biết các phương thức thêm và xóa các mục từ đầu hoặc cuối:

- `arr.push(...items)` -- thêm các items vào cuối,
- `arr.pop()` -- trích xuất một item từ cuối,
- `arr.shift()` -- trích xuất một item từ đầu,
- `arr.unshift(...items)` -- thêm các items vào đầu.

Đây là một vài phương thức khác.

### splice

Làm thế nào để xóa một phần tử từ mảng?

Các mảng là các đối tượng, vì vậy chúng ta có thể thử sử dụng `delete`:

```js
      let arr = ["I", "go", "home"];

      delete arr[1]; // remove "go"

      alert( arr[1] ); // undefined

      // now arr = ["I",  , "home"];
      alert( arr.length ); // 3
```

Phần tử đã bị xóa, nhưng mảng vẫn có 3 phần tử, chúng ta có thể thấy rằng `arr.length == 3`.

Điều đó là tự nhiên, bởi vì `delete obj.key` sẽ xóa một giá trị bằng `key`. Đó là tất cả những gì nó làm. Tốt cho objects. Nhưng đối với các mảng, chúng ta thường muốn các phần tử còn lại dịch chuyển và chiếm vị trí tự do. Chúng ta hy vọng sẽ có một mảng ngắn hơn bây giờ.

Vì vậy, nên sử dụng các phương thức đặc biệt.

Phương thức [arr.splice(str)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/splice) là một con dao quân đội cho các mảng. Nó có thể làm mọi thứ: thêm, xóa và chèn các phần tử.

Cú pháp là:

```js
      arr.splice(index[, deleteCount, elem1, ..., elemN])
```

Nó bắt đầu từ vị trí `index`: loại bỏ các phần tử `deleteCount` và sau đó chèn `elem1, ..., elemN` vào vị trí của chúng. Trả về mảng các phần tử bị loại bỏ.

Phương pháp này dễ nắm bắt bằng các ví dụ.

Hãy bắt đầu với việc xóa:

```js
      let arr = ["I", "study", "JavaScript"];

      arr.splice(1, 1); // from index 1 remove 1 element

      alert( arr ); // ["I", "JavaScript"]
```

Dễ chứ nhỉ? Bắt đầu từ chỉ mục `1` nó đã loại bỏ phần tử `1`.

Trong ví dụ tiếp theo, chúng ta loại bỏ 3 phần tử và thay thế chúng bằng hai phần tử còn lại:

```js
      let arr = ["I", "study", "JavaScript", "right", "now"];

      // remove 3 first elements and replace them with another
      arr.splice(0, 3, "Let's", "dance");

      alert( arr ) // now ["Let's", "dance", "right", "now"]
```

Ở đây chúng ta có thể thấy rằng `splice` trả về mảng các phần tử bị loại bỏ:

```js
      let arr = ["I", "study", "JavaScript", "right", "now"];

      // remove 2 first elements
      let removed = arr.splice(0, 2);

      alert( removed ); // "I", "study" <-- array of removed elements
```

Phương thức `splice` cũng có thể chèn các phần tử mà không cần loại bỏ. Vì vậy, chúng ta cần đặt `deleteCount` thành `0`:

```js
      let arr = ["I", "study", "JavaScript"];

      // from index 2
      // delete 0
      // then insert "complex" and "language"
      arr.splice(2, 0, "complex", "language");

      alert( arr ); // "I", "study", "complex", "language", "JavaScript"
```

<br>

> ---

**📌 Chỉ mục âm được phép (Negative indexes allowed)**

Ở đây và trong các phương thức mảng khác, các Chỉ mục âm được cho phép. Họ chỉ định vị trí từ cuối mảng, như ở đây:

```js
      let arr = [1, 2, 5];

      // from index -1 (one step from the end)
      // delete 0 elements,
      // then insert 3 and 4
      arr.splice(-1, 0, 3, 4);

      alert( arr ); // 1,2,3,4,5
```

> ---

<br>

### slice

Phương thức [arr.slice](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/slice) đơn giản hơn nhiều so với `arr.splice` tương tự.

Cú pháp là:

```js
      arr.slice(start, end)
```

Nó trả về một mảng mới nơi nó sao chép tất cả các items bắt đầu từ chỉ mục `"start"` tới `"end"` (không bao gồm `"end"`). Cả `start` và `end` đều có thể âm, trong trường hợp đó, vị trí từ cuối mảng được giả sử.

Nó hoạt động như `str.slice`, nhưng tạo các phân đoạn (subarrays) thay vì các chuỗi con (substrings).

Ví dụ:

```js
      let str = "test";
      let arr = ["t", "e", "s", "t"];

      alert( str.slice(1, 3) ); // es
      alert( arr.slice(1, 3) ); // e,s

      alert( str.slice(-2) ); // st
      alert( arr.slice(-2) ); // s,t
```

### concat

Phương thức [arr.concat](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/concat) kết hợp mảng với các mảng khác và/hoặc các items khác.

Cú pháp là:

```js
      arr.concat(arg1, arg2...)
```

Nó chấp nhận bất kỳ số lượng đối số -- các mảng hoặc các giá trị.

Kết quả là một mảng mới chứa các items từ `arr`, sau đó `arg1`, `arg2`, v.v.

Nếu một đối số là một mảng hoặc có thuộc tính `Symbol.isConcatSpreadable`, thì tất cả các phần tử của nó được sao chép. Mặt khác, chính đối số được sao chép.

Ví dụ:

```js
      let arr = [1, 2];

      // merge arr with [3,4]
      alert( arr.concat([3, 4])); // 1,2,3,4

      // merge arr with [3,4] and [5,6]
      alert( arr.concat([3, 4], [5, 6])); // 1,2,3,4,5,6

      // merge arr with [3,4], then add values 5 and 6
      alert( arr.concat([3, 4], 5, 6)); // 1,2,3,4,5,6
```

Thông thường, nó chỉ sao chép các phần tử từ các mảng ("mở rộng (spreads)" chúng). Các đối tượng khác, ngay cả khi chúng trông giống như các mảng, được thêm vào như một tổng thể:

```js
      let arr = [1, 2];

      let arrayLike = {
        0: "something",
        length: 1
      };

      alert( arr.concat(arrayLike) ); // 1,2,[object Object]
      //[1, 2, arrayLike]
```

...Nhưng nếu một đối tượng giống như mảng có thuộc tính `Symbol.isConcatSpreadable`, thì các phần tử của nó được thêm vào:

```js
      let arr = [1, 2];

      let arrayLike = {
        0: "something",
        1: "else",
        [Symbol.isConcatSpreadable]: true,
        length: 2
      };

      alert( arr.concat(arrayLike) ); // 1,2,something,else
```

## Searching in array

Đây là những phương pháp để tìm kiếm một cái gì đó trong một mảng.

### indexOf/lastIndexOf and includes

Các phương thức [arr.indexOf](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/indexOf), [arr.lastIndexOf](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/lastIndexOf) và [arr.includes](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/includes) có cùng một cú pháp và về cơ bản giống như các đối tác chuỗi của chúng, nhưng hoạt động trên các items thay vì các ký tự:

- `arr.indexOf(item, from)` tìm kiếm `item` bắt đầu từ chỉ mục `from` và trả về chỉ mục nơi nó được tìm thấy, nếu không thì `-1`.
- `arr.lastIndexOf(item, from)` -- giống nhau, nhưng tìm từ phải sang trái.
- `arr.includes(item, from)` -- tìm kiếm `item` bắt đầu từ chỉ mục `from`, trả về `true` nếu tìm thấy.

Ví dụ:

```js
      let arr = [1, 0, false];

      alert( arr.indexOf(0) ); // 1
      alert( arr.indexOf(false) ); // 2
      alert( arr.indexOf(null) ); // -1

      alert( arr.includes(1) ); // true
```

Lưu ý rằng các phương thức sử dụng so sánh `===`. Vì vậy, nếu chúng ta tìm kiếm `false`, nó sẽ tìm thấy chính xác `false` chứ không phải số không.

Nếu chúng ta muốn kiểm tra sự bao gồm và không muốn biết chỉ số chính xác, thì `arr.includes` được ưu tiên.

Ngoài ra, một điểm khác biệt rất nhỏ của `includes` là nó xử lý chính xác `NaN`, không giống như `indexOf/lastIndexOf`:

```js
      const arr = [NaN];
      alert( arr.indexOf(NaN) ); // -1 (should be 0, but === equality doesn't work for NaN)
      alert( arr.includes(NaN) );// true (correct)
```

### find and findIndex

Hãy tưởng tượng chúng ta có một mảng các đối tượng. Làm thế nào để chúng ta tìm thấy một đối tượng với quy tắc cụ thể?

Ở đây, phương thức [arr.find](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/find) có ích.

Cú pháp là:

```js
      let result = arr.find(function(item, index, array) {
        // should return true if the item is what we are looking for
      });
```

Hàm được gọi là lặp lại cho từng phần tử của mảng:

- `item` là phần tử.
- `index` là chỉ mục của nó.
- `mảng` là mảng chính nó.

Nếu nó trả về `true`, tìm kiếm bị dừng lại ,`item` được trả về. Nếu không tìm thấy gì, `undefined` được trả về.

Ví dụ: chúng ta có một mảng người dùng, mỗi người có các trường `id` và `name`. Hãy tìm một cái có `id == 1`:

```js
      let users = [
        {id: 1, name: "John"},
        {id: 2, name: "Pete"},
        {id: 3, name: "Mary"}
      ];

      let user = users.find(item => item.id == 1);

      alert(user.name); // John
```

Trong thực tế các mảng của các đối tượng là một điều phổ biến, vì vậy phương thức `find` rất hữu ích.

Lưu ý rằng trong ví dụ chúng ta cung cấp cho `find` một hàm đối số duy nhất `item => item.id == 1`. Các tham số khác của `find` hiếm khi được sử dụng.

Phương thức [arr.findIndex](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/findIndex) về cơ bản là giống nhau, nhưng nó trả về chỉ mục nơi phần tử được tìm thấy thay vì các phần tử.

### filter

Phương thức `find` tìm kiếm một phần tử (đầu tiên) làm cho hàm trả về `true`.

Nếu có thể có nhiều, chúng ta có thể sử dụng [arr.filter(fn)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/filter).

Cú pháp gần giống như `find`, nhưng nó trả về một mảng các phần tử khớp:

```js
      let results = arr.filter(function(item, index, array) {
        // should return true if the item passes the filter
      });
```

Ví dụ:

```js
      let users = [
        {id: 1, name: "John"},
        {id: 2, name: "Pete"},
        {id: 3, name: "Mary"}
      ];

      // returns array of the first two users
      let someUsers = users.filter(item => item.id < 3);

      alert(someUsers.length); // 2
```

## Transform an array

Phần này là về các phương thức chuyển đổi hoặc sắp xếp lại mảng.


### map

Phương thức [arr.map](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/map) là một trong những phương thức hữu ích và thường được sử dụng nhất.

Cú pháp là:

```js
      let result = arr.map(function(item, index, array) {
        // returns the new value instead of item
      })
```

Nó gọi hàm cho từng phần tử của mảng và trả về mảng kết quả.

Chẳng hạn, ở đây chúng ta biến đổi từng phần tử thành chiều dài của nó:

```js
      let lengths = ["Bilbo", "Gandalf", "Nazgul"].map(item => item.length);
      alert(lengths); // 5,7,6
```

### sort(fn)

Phương thức [arr.sort](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/sort) sắp xếp mảng *tại chỗ*.

Ví dụ:

```js
      let arr = [ 1, 2, 15 ];

      // the method reorders the content of arr (and returns it)
      arr.sort();

      alert( arr );  // 1, 15, 2
```

Bạn có nhận thấy điều gì kỳ lạ trong kết quả không?

Thứ tự trở thành `1, 15, 2`. Sai. Nhưng tại sao?

**Các items được sắp xếp theo chuỗi theo mặc định.**

Theo nghĩa đen, tất cả các yếu tố được chuyển đổi thành chuỗi và sau đó so sánh. Vì vậy, thứ tự từ điển được áp dụng và thực sự `"2" > "15"`.

Để sử dụng thứ tự sắp xếp riêng của chúng ta, chúng ta cần cung cấp một hàm gồm hai đối số làm đối số của `arr.sort()`.

Hàm nên hoạt động như thế này:

```js
      function compare(a, b) {
        if (a > b) return 1;
        if (a == b) return 0;
        if (a < b) return -1;
      }
```

Ví dụ:

```js
      function compareNumeric(a, b) {
        if (a > b) return 1;
        if (a == b) return 0;
        if (a < b) return -1;
      }

      let arr = [ 1, 2, 15 ];

      arr.sort(compareNumeric);

      alert(arr);  // 1, 2, 15
```

Bây giờ nó hoạt động như dự định.

Hãy bước sang một bên và nghĩ những gì đang xảy ra. `arr` có thể là mảng của bất cứ thứ gì, phải không? Nó có thể chứa số hoặc chuỗi hoặc phần tử html hoặc bất cứ điều gì. Chúng ta có một bộ *cái gì đó*. Để sắp xếp nó, chúng ta cần một *function sắp xếp* biết cách so sánh các yếu tố của nó. Mặc định là một thứ tự chuỗi.

Phương thức `arr.sort(fn)` có built-in thực hiện thuật toán sắp xếp. Chúng ta không cần quan tâm làm thế nào nó hoạt động chính xác (một [quicksort](https://en.wikipedia.org/wiki/Quicksort) được tối ưu hóa hầu hết thời gian). Nó sẽ đi theo mảng, so sánh các phần tử của nó bằng cách sử dụng hàm được cung cấp và sắp xếp lại chúng, tất cả những gì chúng ta cần là cung cấp `fn` để so sánh.

Nhân tiện, nếu chúng ta muốn biết những yếu tố nào được so sánh -- không có gì ngăn cản alerting chúng:

```js
      [1, -2, 15, 2, 0, 8].sort(function(a, b) {
        alert( a + " <> " + b );
      });
```

Thuật toán có thể so sánh một yếu tố nhiều lần trong quy trình, nhưng nó cố gắng thực hiện càng ít so sánh càng tốt.

<br>

> ---

**📌 Hàm so sánh có thể trả về bất kỳ số nào**

Trên thực tế, một hàm so sánh chỉ được yêu cầu trả về một số dương để nói "lớn hơn" và một số âm để nói "ít hơn".

Điều đó cho phép viết các hàm ngắn hơn:

```js
      let arr = [ 1, 2, 15 ];

      arr.sort(function(a, b) { return a - b; });

      alert(arr);  // 1, 2, 15
```

> ---

<br>
<br>

> ---

**📌 Arrow functions cho tốt nhất (Arrow functions for the best)**

Còn nhớ **arrow functions**? Chúng ta có thể sử dụng chúng ở đây để sắp xếp gọn gàng hơn:

```js
      arr.sort( (a, b) => a - b );
```

Điều này hoạt động chính xác như phiên bản khác, dài hơn, ở trên.

> ---

<br>

### reverse

Phương thức [arr.reverse](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/reverse) đảo ngược thứ tự các phần tử trong `arr`.

Ví dụ:

```js
      let arr = [1, 2, 3, 4, 5];
      arr.reverse();

      alert( arr ); // 5,4,3,2,1
```

Nó cũng trả về mảng `arr` sau khi đảo ngược.

### split and join

Đây là tình huống từ cuộc sống thực. Chúng ta đang viết một ứng dụng nhắn tin và người này enters danh sách người nhận được phân tách bằng dấu phẩy: `John, Pete, Mary`. Nhưng đối với chúng ta, một loạt các tên sẽ thoải mái hơn nhiều so với một chuỗi. Làm thế nào để có được nó?

Phương thức [str.split(delim)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/split) thực hiện chính xác điều đó. Nó phân tách chuỗi thành một mảng bởi dấu phân cách đã cho `delim`.

Trong ví dụ dưới đây, chúng ta phân chia bằng dấu phẩy theo sau là khoảng trắng:

```js
      let names = 'Bilbo, Gandalf, Nazgul';

      let arr = names.split(', ');

      for (let name of arr) {
        alert( `A message to ${name}.` ); // A message to Bilbo  (and other names)
      }
```

Phương thức `split` có một đối số số thứ hai tùy chọn -- giới hạn về độ dài mảng. Nếu nó được cung cấp, sau đó các phần tử bổ sung được bỏ qua. Trong thực tế, nó hiếm khi được sử dụng:

```js
      let arr = 'Bilbo, Gandalf, Nazgul, Saruman'.split(', ', 2);

      alert(arr); // Bilbo, Gandalf
```

<br>

> ---

**📌 Chia thành các chữ cái (Split into letters)**

Cuộc gọi tới `split(s)` với một `s` trống sẽ chia chuỗi thành một mảng các chữ cái:

```js
      let str = "test";

      alert( str.split('') ); // t,e,s,t
```

> ---

<br>

Cuộc gọi [arr.join(str)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/join) thực hiện ngược lại với `split`. Nó tạo ra một chuỗi các `arr` items được dán bởi `str` giữa chúng.

Ví dụ:

```js
      let arr = ['Bilbo', 'Gandalf', 'Nazgul'];

      let str = arr.join(';');

      alert( str ); // Bilbo;Gandalf;Nazgul
```

### reduce/reduceRight

Khi chúng ta cần lặp lại qua một mảng --  chúng ta có thể sử dụng `forEach`.

Khi chúng ta cần lặp lại và trả lại dữ liệu cho từng phần tử -- chúng ta có thể sử dụng `map`.

Các phương thức [arr.reduce](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/reduce) và arr.reduceRight](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/reduceRight) cũng thuộc dạng đó, nhưng phức tạp hơn một chút. Chúng được sử dụng để tính toán một giá trị duy nhất dựa trên mảng.

Cú pháp là:

```js
      let value = arr.reduce(function(previousValue, item, index, arr) {
        // ...
      }, initial);
```

Function được áp dụng cho các yếu tố. Bạn có thể nhận thấy các đối số quen thuộc, bắt đầu từ thứ 2:

- `item` - là mục mảng hiện tại.
- `index` - là vị trí của nó.
- `arr` - là mảng.

Cho đến nay, trông giống như `forEach/map`. Nhưng có thêm một đối số:

- `previousValue` -- là kết quả của lệnh gọi hàm trước, `initial` cho cuộc gọi đầu tiên.

Cách dễ nhất để nắm bắt đó là ví dụ.

Ở đây chúng ta nhận được một mảng của một dòng:

```js
      let arr = [1, 2, 3, 4, 5];

      let result = arr.reduce((sum, current) => sum + current, 0);

      alert(result); // 15
```

Ở đây, chúng ta đã sử dụng biến thể phổ biến nhất của `reduce` chỉ sử dụng 2 đối số.

Hãy xem chi tiết những gì đang diễn ra.

1. Trong lần chạy đầu tiên, `sum` là giá trị ban đầu (đối số cuối cùng của `reduce`), bằng `0` và `current` là phần tử mảng đầu tiên, bằng `1`. Vì vậy, kết quả là `1`.
2. Trong lần chạy thứ hai, `sum = 1`, chúng ta thêm phần tử mảng thứ hai (`2`) vào nó và trả về.
3. Trong lần chạy thứ 3, `sum = 3` và chúng ta thêm một phần tử nữa vào đó, v.v.

Luồng tính toán:

![](reduce.png)

Hoặc ở dạng bảng, trong đó mỗi hàng đại diện là một lệnh gọi hàm trên phần tử mảng tiếp theo:

| | `sum` |` hiện tại` | `result` |
| --------------- | ----- | --------- | --------- |
| cuộc gọi đầu tiên | `0` |` 1` | `1` |
| cuộc gọi thứ hai | `1` |` 2` | `3` |
| cuộc gọi thứ ba | `3` |` 3` | `6` |
| cuộc gọi thứ tư | `6` |` 4` | `10` |
| cuộc gọi thứ năm | `10` |` 5` | `15` |

Như chúng ta có thể thấy, kết quả của cuộc gọi trước trở thành đối số đầu tiên của cuộc gọi tiếp theo.

Chúng ta cũng có thể bỏ qua giá trị ban đầu:

```js
      let arr = [1, 2, 3, 4, 5];

      // removed initial value from reduce (no 0)
      let result = arr.reduce((sum, current) => sum + current);

      alert( result ); // 15
```

Kết quả là như nhau. Đó là bởi vì nếu không có khởi tạo ban đầu, thì `reduce` lấy phần tử đầu tiên của mảng làm giá trị ban đầu và bắt đầu phép lặp từ phần tử thứ 2.

Bảng tính toán giống như trên, trừ hàng đầu tiên.

Nhưng việc sử dụng như vậy đòi hỏi một sự cẩn thận cao độ. Nếu mảng trống, thì cuộc gọi `reduce` không có giá trị ban đầu sẽ báo lỗi.

Đây là một ví dụ:

```js
      let arr = [];

      // Error: Reduce of empty array with no initial value
      // if the initial value existed, reduce would return it for the empty arr.
      arr.reduce((sum, current) => sum + current);
```


Vì vậy, nên luôn luôn chỉ định giá trị ban đầu.

Phương thức [arr.reduceRight](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/reduceRight) cũng tương tự, nhưng đi từ phải sang trái.

## Iterate: forEach

Phương thức [arr.forEach](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/forEach) cho phép chạy một hàm cho mọi phần tử của mảng.

Cú pháp:

```js
      arr.forEach(function(item, index, array) {
        // ... do something with item
      });
```

Ví dụ, điều này cho thấy từng phần tử của mảng:

```js
      // for each element call alert
      ["Bilbo", "Gandalf", "Nazgul"].forEach(alert);
```

Và mã này chi tiết hơn về vị trí của chúng trong mảng mục tiêu:

```js
      ["Bilbo", "Gandalf", "Nazgul"].forEach((item, index, array) => {
        alert(`${item} is at index ${index} in ${array}`);
      });
```

Kết quả của hàm (nếu nó trả về bất kỳ) bị bỏ đi và bỏ qua.

## Array.isArray

Mảng không tạo thành một kiểu ngôn ngữ riêng biệt. Chúng dựa trên các đối tượng.

Vì vậy, `typeof` không giúp phân biệt một đối tượng đơn giản với một mảng:

```js
      alert(typeof {}); // object
      alert(typeof []); // same
```

...Nhưng các mảng được sử dụng thường xuyên đến nỗi có một phương thức đặc biệt cho chúng: [Array.isArray(value)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/isArray). Nó trả về `true` nếu `value` là một mảng và `false` nếu không.

```js
      alert(Array.isArray({})); // false

      alert(Array.isArray([])); // true
```

## Hầu hết các phương thức hỗ trợ "thisArg"

Hầu như tất cả các phương thức mảng gọi các hàm -- như `find`, `filter`, `map`, với một ngoại lệ đáng chú ý là `sort`, chấp nhận một tham số bổ sung tùy chọn `thisArg`.

Tham số đó không được giải thích trong các phần trên, vì nó hiếm khi được sử dụng. Nhưng để hoàn thiện chúng ta phải bao gồm nó.

Đây là cú pháp đầy đủ của các phương thức này:

```js
      arr.find(func, thisArg);
      arr.filter(func, thisArg);
      arr.map(func, thisArg);
      // ...
      // thisArg is the optional last argument
```

Giá trị của tham số `thisArg` trở thành `this` cho `func`.

Ví dụ, ở đây chúng ta sử dụng một phương thức đối tượng làm bộ lọc và `thisArg` có ích:

```js
      let user = {
        age: 18,
        younger(otherUser) {
          return otherUser.age < this.age;
        }
      };

      let users = [
        {age: 12},
        {age: 16},
        {age: 32}
      ];

      // find all users younger than user
      let youngerUsers = users.filter(user.younger, user);

      alert(youngerUsers.length); // 2
```

Trong cuộc gọi ở trên, chúng ta sử dụng `user.younger` làm bộ lọc và cũng cung cấp `user` làm bối cảnh cho nó. Nếu chúng ta không cung cấp ngữ cảnh, `users.filter(user.younger)` sẽ gọi `user.younger` là một hàm độc lập, với `this=undefined`. Điều đó có nghĩa là một lỗi ngay lập tức.

## Tóm lược

Một loạt các phương thức mảng:

- Để add/remove các phần tử:
  - `push(...items)` -- thêm các items vào cuối,
  - `pop()` -- trích xuất một item từ cuối,
  - `shift()` -- trích xuất một item từ đầu,
  - `unshift(...items)` -- thêm các mục vào đầu.
  - `splice(pos, deleteCount, ...items)` -- tại chỉ mục `pos` xóa các phần tử `deleteCount` và chèn `items`.
  - `slice(start, end)` -- tạo một mảng mới, sao chép các phần tử từ vị trí `start` đến `end` (không bao gồm) vào nó.
  - `concat(...items)` -- trả về một mảng mới: sao chép tất cả các thành viên của mảng hiện tại và thêm `items` vào nó. Nếu bất kỳ `items` nào là một mảng, thì các phần tử của nó được lấy.

- Để tìm kiếm giữa các phần tử:
  - `indexOf/lastIndexOf(item, pos)` -- tìm kiếm `item` bắt đầu từ vị trí `pos`, trả về chỉ mục hoặc `-1` nếu không tìm thấy.
  - `includes(value)` -- trả về `true` nếu mảng có `value`, nếu không thì `false`.
  - `find/filter(func)` -- lọc các phần tử thông qua hàm, trả về đầu tiên/tất cả các giá trị làm cho nó trả về `true`.
  - `findIndex` giống như `find`, nhưng trả về chỉ mục thay vì giá trị.

- Để chuyển đổi mảng:
  - `map(func)` -- tạo ra một mảng mới từ kết quả của việc gọi `func` cho mọi phần tử.
  - `sort(func)` -- sắp xếp mảng tại chỗ, sau đó trả về nó.
  - `reverse()` -- đảo ngược mảng tại chỗ, sau đó trả về nó.
  - `split/join` -- chuyển đổi một chuỗi thành mảng và trả lại.
  - `reduce(func, initial)` -- tính một giá trị đơn trên mảng bằng cách gọi `func` cho mỗi phần tử và truyền kết quả trung gian giữa các lệnh gọi.

- Để lặp lại các phần tử:
  - `forEach(func)` -- gọi `func` cho mọi phần tử, không trả về bất cứ thứ gì.

- Ngoài ra:
  - `Array.isArray(arr)` kiểm tra `arr` là một mảng.

Xin lưu ý rằng các phương thức `sort`, `reverse` và `splice` tự sửa đổi mảng.

Những phương thức này là những phương thức được sử dụng nhiều nhất, chúng bao gồm 99% trường hợp sử dụng. Nhưng có một vài cái khác:

- [arr.some(fn)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/some)/[arr.every(fn)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/every) kiểm tra mảng.

  Hàm `fn` được gọi trên mỗi phần tử của mảng tương tự như `map`. Nếu bất kỳ/tất cả các kết quả là `true`, trả về `true`, nếu không thì `false`.

- [arr.fill(value, start, end)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/fill) -- lấp đầy mảng bằng cách lặp lại `value` từ chỉ mục `start` đến `end`.

- [arr.copyWithin(target, start, end)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/copyWithin) -- sao chép các phần tử của nó từ vị trí `start` cho đến vị trí `end` thành *chính nó*, tại vị trí `target` (ghi đè lên hiện tại).

Để biết danh sách đầy đủ, xem [hướng dẫn](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array).

Ngay từ cái nhìn đầu tiên có vẻ như có rất nhiều phương thức, khá khó nhớ. Nhưng thật ra điều đó dễ hơn nhiều so với vẻ ngoài của nó.

Nhìn qua chiếc áo chỉ để nhận ra chúng. Sau đó, giải quyết các nhiệm vụ của chương này để thực hành, để bạn có kinh nghiệm với các phương thức mảng.

Sau đó, bất cứ khi nào bạn cần làm một cái gì đó với một mảng, và bạn không biết làm thế nào - hãy đến đây, nhìn vào chiếc áo choàng và tìm phương thức phù hợp. Ví dụ sẽ giúp bạn viết nó một cách chính xác. Bạn sẽ sớm tự động ghi nhớ các phương thức mà không cần nỗ lực cụ thể từ phía bạn.
