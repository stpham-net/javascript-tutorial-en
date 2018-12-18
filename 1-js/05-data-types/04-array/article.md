# Mảng 

Các đối tượng cho phép bạn lưu trữ các bộ sưu tập giá trị có khóa. Đó là tốt.

Nhưng khá thường xuyên chúng ta thấy rằng chúng ta cần một *bộ sưu tập được xắp xếp (ordered collection)*, trong đó chúng ta có phần tử thứ 1, thứ 2, thứ 3, v.v. Ví dụ: chúng ta cần điều đó để lưu trữ một danh sách một cái gì đó: người dùng, hàng hóa, các phần tử HTML, v.v. 

Không thuận tiện khi sử dụng một đối tượng ở đây, vì nó không cung cấp phương thức nào để quản lý thứ tự các phần tử. Chúng ta không thể chèn một thuộc tính mới "giữa các" cái hiện có. Đối tượng chỉ là không có nghĩa cho việc sử dụng như vậy.

Tồn tại một cấu trúc dữ liệu đặc biệt có tên là `Array`, để lưu trữ các bộ sưu tập theo thứ tự. 

## Declaration

Có hai cú pháp để tạo một mảng trống:

```js
      let arr = new Array();
      let arr = [];
```

Hầu như tất cả thời gian, cú pháp thứ hai được sử dụng. Chúng ta có thể cung cấp các phần tử ban đầu trong ngoặc:

```js
      let fruits = ["Apple", "Orange", "Plum"];
```

Các phần tử mảng được đánh số, bắt đầu bằng không.

Chúng ta có thể lấy một phần tử bằng số của nó trong ngoặc vuông:

```js
      let fruits = ["Apple", "Orange", "Plum"];

      alert( fruits[0] ); // Apple
      alert( fruits[1] ); // Orange
      alert( fruits[2] ); // Plum
```

Chúng ta có thể thay thế một phần tử:

```js
      fruits[2] = 'Pear'; // now ["Apple", "Orange", "Pear"]
```

...Hoặc thêm một cái mới vào mảng:

```js
      fruits[3] = 'Lemon'; // now ["Apple", "Orange", "Pear", "Lemon"]
```

Tổng số phần tử trong mảng là `length`:

```js
      let fruits = ["Apple", "Orange", "Plum"];

      alert( fruits.length ); // 3
```

Chúng ta cũng có thể sử dụng `alert` để hiển thị toàn bộ mảng.

```js
      let fruits = ["Apple", "Orange", "Plum"];

      alert( fruits ); // Apple,Orange,Plum
```

Một mảng có thể lưu trữ các phần tử của bất kỳ kiểu nào.

Ví dụ:

```js
      // mix of values
      let arr = [ 'Apple', { name: 'John' }, true, function() { alert('hello'); } ];

      // get the object at index 1 and then show its name
      alert( arr[1].name ); // John

      // get the function at index 3 and run it
      arr[3](); // hello
```

<br>

> ---

**📌 Trailing comma**

Một mảng, giống như một đối tượng, có thể kết thúc bằng dấu phẩy:

```js 
      let fruits = [
        "Apple", 
        "Orange", 
        "Plum",
      ];
```

Kiểu "trailing comma" giúp việc chèn/xóa các mục dễ dàng hơn vì tất cả các dòng đều giống nhau.

> ---

<br>

## Phương thức pop/push, shift/unshift

Một [Hàng đợi (queue)](https://en.wikipedia.org/wiki/Queue_(abstract_data_type)) là một trong những cách sử dụng phổ biến nhất của một mảng. Trong khoa học máy tính, điều này có nghĩa là một tập hợp các phần tử hỗ trợ hai thao tác:

- `push` nối thêm một phần tử vào cuối.
- `shift` lấy một phần tử từ đầu, tiến lên hàng đợi, để phần tử thứ 2 trở thành phần tử thứ nhất.

![](queue.png)

Mảng hỗ trợ cả hai thao tác.

Trong thực tế, chúng ta gặp nó rất thường xuyên. Ví dụ, một hàng các tin nhắn cần được hiển thị trên màn hình.

Có một trường hợp sử dụng khác cho mảng -- cấu trúc dữ liệu có tên [stack](https://en.wikipedia.org/wiki/Stack_(abstract_data_type)). 

Nó hỗ trợ hai thao tác:

- `push` thêm một phần tử vào cuối.
- `pop` lấy một phần tử từ cuối.

Vì vậy, các phần tử mới được thêm hoặc lấy luôn từ "kết thúc".

Một ngăn xếp (stack) thường được minh họa dưới dạng một gói thẻ (pack of cards): thẻ mới được thêm vào đầu hoặc được lấy từ đầu:

![](stack.png)

Đối với ngăn xếp, item đẩy mới nhất được nhận trước, đó cũng được gọi là nguyên tắc LIFO (Last-In-First-Out). Đối với hàng đợi, chúng ta có FIFO (First-In-First-Out).

Mảng trong JavaScript có thể hoạt động như một hàng đợi và như một ngăn xếp. Chúng cho phép bạn thêm/xóa các phần tử cả đến/từ đầu hoặc cuối. 

Trong khoa học máy tính, cấu trúc dữ liệu cho phép nó được gọi là [deque](https://en.wikipedia.org/wiki/Double-ended_queue).

**Các phương thức hoạt động với phần cuối của mảng:**

**`pop`**

Trích xuất phần tử cuối cùng của mảng và trả về nó:

```js
      let fruits = ["Apple", "Orange", "Pear"];

      alert( fruits.pop() ); // remove "Pear" and alert it

      alert( fruits ); // Apple, Orange
```

**`push`**

Nối phần tử vào cuối mảng:

```js
      let fruits = ["Apple", "Orange"];

      fruits.push("Pear");

      alert( fruits ); // Apple, Orange, Pear
```

Cuộc gọi `fruits.push(...)` bằng với `fruits[fruits.length] = ...`.

**Các phương thức hoạt động với phần đầu của mảng:**

**`shift`**

Trích xuất phần tử đầu tiên của mảng và trả về nó:

```js
      let fruits = ["Apple", "Orange", "Pear"];

      alert( fruits.shift() ); // remove Apple and alert it

      alert( fruits ); // Orange, Pear
```

**`unshift`**

Thêm phần tử vào đầu mảng:

```js
      let fruits = ["Orange", "Pear"];

      fruits.unshift('Apple');

      alert( fruits ); // Apple, Orange, Pear
```

Các phương thức `push` và `unshift` có thể thêm nhiều phần tử cùng một lúc:

```js
      let fruits = ["Apple"];

      fruits.push("Orange", "Peach");
      fruits.unshift("Pineapple", "Lemon");

      // ["Pineapple", "Lemon", "Apple", "Orange", "Peach"]
      alert( fruits );
```

## Nội bộ (Internals)

Một mảng là một loại đối tượng đặc biệt. Dấu ngoặc vuông được sử dụng để truy cập thuộc tính `arr[0]` thực sự đến từ cú pháp đối tượng. Số được sử dụng làm chìa khóa (keys). 

Chúng mở rộng các đối tượng cung cấp các phương thức đặc biệt để làm việc với các bộ sưu tập dữ liệu theo thứ tự và cả thuộc tính `length`. Nhưng cốt lõi nó vẫn là một đối tượng.

Hãy nhớ rằng, chỉ có 7 loại cơ bản trong JavaScript. Mảng là một đối tượng và do đó hoạt động như một đối tượng. 

Ví dụ, nó được sao chép bằng cách tham chiếu:

```js
      let fruits = ["Banana"]

      let arr = fruits; // copy by reference (two variables reference the same array)

      alert( arr === fruits ); // true

      arr.push("Pear"); // modify the array by reference

      alert( fruits ); // Banana, Pear - 2 items now
```

...Nhưng điều làm cho mảng thực sự đặc biệt là đại diện nội bộ của chúng. The engine cố gắng lưu trữ các phần tử của nó trong vùng nhớ liền kề nhau, giống như được mô tả trên hình minh họa trong chương này và cũng có những tối ưu hóa khác, để làm cho mảng hoạt động rất nhanh.

Nhưng tất cả đều break nếu chúng ta bỏ làm việc với một mảng như với một "bộ sưu tập được xắp xếp" và bắt đầu làm việc với nó như thể nó là một đối tượng thông thường.

Ví dụ, về mặt kỹ thuật, chúng ta có thể làm điều này:

```js
      let fruits = []; // make an array

      fruits[99999] = 5; // assign a property with the index far greater than its length

      fruits.age = 25; // create a property with an arbitrary name
```

Điều đó là có thể, bởi vì mảng là các đối tượng ở cơ sở của chúng. Chúng ta có thể thêm bất kỳ thuộc tính nào cho chúng.

Nhưng the engine sẽ thấy rằng chúng ta đang làm việc với mảng như với một đối tượng thông thường. Tối ưu hóa cụ thể mảng (Array-specific optimizations) không phù hợp cho các trường hợp như vậy và sẽ bị tắt, lợi ích của chúng biến mất.

Các cách để sử dụng sai một mảng:

- Thêm một thuộc tính không phải là số như `arr.test = 5`. 
- Tạo các lỗ (Make holes), như: thêm `arr[0]` và sau đó `arr[1000]` (và không có gì giữa chúng).
- Điền vào mảng theo thứ tự ngược lại, như `arr[1000]`, `arr[999]`, v.v.

Vui lòng nghĩ về mảng như các cấu trúc đặc biệt để làm việc với *dữ liệu được xắp xếp*. Chúng cung cấp các phương thức đặc biệt cho điều đó. Mảng được điều chỉnh cẩn thận bên trong các JavaScript engines để làm việc với dữ liệu được sắp xếp liền kề, vui lòng sử dụng chúng theo cách này. Và nếu bạn cần các khóa tùy ý, rất có thể bạn thực sự cần một đối tượng thông thường `{}`.

## Hiệu suất (Performance)

Các phương thức `push/pop` chạy nhanh, trong khi `shift/unshift` thì chậm.

![](array-speed.png)

Tại sao nó nhanh hơn để làm việc với phần cuối của một mảng so với phần đầu của nó? Hãy xem điều gì xảy ra trong quá trình thực thi:

```js
      fruits.shift(); // take 1 element from the start
```

Không đủ để lấy và xóa phần tử có số `0`. Các yếu tố khác cũng cần phải được đánh số lại.

Hoạt động `shift` phải thực hiện 3 điều:

1. Loại bỏ phần tử có chỉ mục `0`.
2. Di chuyển tất cả các yếu tố sang trái, đánh số lại chúng từ chỉ mục `1` sang `0`, từ `2` sang `1`, v.v.
3. Cập nhật thuộc tính `length`.

![](array-shift.png)

**Càng nhiều phần tử trong mảng, càng cần nhiều thời gian để di chuyển chúng, càng nhiều thao tác trong bộ nhớ.**

Điều tương tự xảy ra với `unshift`: để thêm một phần tử vào đầu mảng, trước tiên chúng ta cần di chuyển các phần tử hiện có sang bên phải, tăng chỉ mục của chúng.

Và những gì với `push/pop`? Chúng không cần phải di chuyển bất cứ điều gì. Để trích xuất một phần tử từ cuối, phương thức `pop` làm sạch chỉ mục và rút ngắn `length`.

Các hành động cho thao tác `pop`:

```js
      fruits.pop(); // take 1 element from the end
```

![](array-pop.png)

**Phương thức `pop` không cần di chuyển bất cứ thứ gì, bởi vì các phần tử khác giữ chỉ mục của chúng. Đó là lý do tại sao nó rất nhanh.**

Điều tương tự với phương thức `push`.

## Vòng lặp (Loops)

Một trong những cách lâu đời nhất để xoay vòng các mục mảng là vòng lặp `for` trên các chỉ mục:

```js
      let arr = ["Apple", "Orange", "Pear"];

      for (let i = 0; i < arr.length; i++) {
        alert( arr[i] );
      }
```

Nhưng đối với mảng có một dạng vòng lặp khác, `for..of`:

```js
      let fruits = ["Apple", "Orange", "Plum"];

      // iterates over array elements
      for (let fruit of fruits) {
        alert( fruit ); 
      }
```

`For..of` không cấp quyền truy cập vào số phần tử hiện tại, chỉ là giá trị của nó, nhưng trong hầu hết các trường hợp là đủ. Và nó ngắn hơn.

Về mặt kỹ thuật, vì các mảng là các đối tượng, nên cũng có thể sử dụng `for..in`:

```js
      let arr = ["Apple", "Orange", "Pear"];

      for (let key in arr) {
        alert( arr[key] ); // Apple, Orange, Pear
      }
```

Nhưng đó thực sự là một ý tưởng tồi. Có những vấn đề tiềm ẩn với nó:

1. Vòng lặp `for..in` lặp lại *tất cả các thuộc tính*, không chỉ các số.

    Có những đối tượng được gọi là "giống như mảng" trong trình duyệt và trong các môi trường khác, *trông giống như mảng*. Đó là, chúng có các thuộc tính `length` và index, nhưng chúng cũng có thể có các thuộc tính và phương thức không phải là số khác mà chúng ta thường không cần. Vòng lặp `for..in` sẽ liệt kê chúng. Vì vậy, nếu chúng ta cần làm việc với các đối tượng giống như mảng, thì các thuộc tính "phụ" này có thể trở thành một vấn đề.

2. Vòng lặp `for..in` được tối ưu hóa cho các đối tượng chung, không phải mảng và do đó chậm hơn 10 - 100 lần. Tất nhiên, nó vẫn rất nhanh. Việc tăng tốc có thể chỉ quan trọng trong các nút cổ chai hoặc chỉ không liên quan. Nhưng chúng ta vẫn nên nhận thức được sự khác biệt.

Nói chung, chúng ta không nên sử dụng `for..in` cho mảng.


## Một từ về "length"

Thuộc tính `length` tự động cập nhật khi chúng ta sửa đổi mảng. Nói chính xác, nó thực sự không phải là số lượng giá trị trong mảng, mà là chỉ số số lớn nhất cộng với một.

Chẳng hạn, một phần tử duy nhất với chỉ số lớn cho big length:

```js
      let fruits = [];
      fruits[123] = "Apple";

      alert( fruits.length ); // 124
```

Lưu ý rằng chúng ta thường không sử dụng mảng như thế. 

Một điều thú vị khác về thuộc tính `length` là nó có thể ghi được.

Nếu chúng ta tăng nó bằng tay, không có gì thú vị xảy ra. Nhưng nếu chúng ta giảm nó, mảng bị cắt ngắn. Quá trình là không thể đảo ngược, đây là ví dụ:

```js
      let arr = [1, 2, 3, 4, 5];

      arr.length = 2; // truncate to 2 elements
      alert( arr ); // [1, 2]

      arr.length = 5; // return length back
      alert( arr[3] ); // undefined: the values do not return
```

Vì vậy, cách đơn giản nhất để xóa mảng là: `arr.length = 0;`.


## new Array()

Có thêm một cú pháp để tạo một mảng:

```js
      let arr = new Array("Apple", "Pear", "etc");
```

Nó hiếm khi được sử dụng, bởi vì dấu ngoặc vuông `[]` ngắn hơn. Ngoài ra có một tính năng khó khăn với nó.

Nếu `new Array` được gọi với một đối số là một số, thì nó tạo ra một mảng *không có các items, nhưng với độ dài đã cho*.

Chúng ta hãy xem làm thế nào người ta có thể tự bắn vào chân mình:

```js
      let arr = new Array(2); // will it create an array of [2] ?

      alert( arr[0] ); // undefined! no elements.

      alert( arr.length ); // length 2
```

Trong đoạn mã trên, `new Array(number)` có tất cả các phần tử `undefined`.

Để tránh những bất ngờ như vậy, chúng ta thường sử dụng dấu ngoặc vuông, trừ khi chúng ta thực sự biết những gì chúng ta đang làm.

## Mảng nhiều chiều (Multidimensional arrays)

Mảng có thể có các items cũng là mảng. Chúng ta có thể sử dụng nó cho các mảng nhiều chiều, để lưu trữ ma trận:

```js
      let matrix = [
        [1, 2, 3],
        [4, 5, 6],
        [7, 8, 9]
      ];

      alert( matrix[1][1] ); // the central element
```

## toString

Mảng có cách triển khai riêng của phương thức `toString` trả về danh sách các phần tử được phân tách bằng dấu phẩy.

Ví dụ:

```js
      let arr = [1, 2, 3];

      alert( arr ); // 1,2,3
      alert( String(arr) === '1,2,3' ); // true
```

Ngoài ra, hãy thử điều này:

```js
      alert( [] + 1 ); // "1"
      alert( [1] + 1 ); // "11"
      alert( [1,2] + 1 ); // "1,21"
```

Mảng không có `Symbol.toPrimitive`, không phải là `valueOf` khả thi, chúng chỉ thực hiện chuyển đổi `toString`, vì vậy ở đây `[]` trở thành một chuỗi rỗng, `[1]` trở thành `"1"` và `[1,2]` trở thành `"1,2"`.

Khi toán tử nhị phân cộng `"+"` thêm một cái gì đó vào một chuỗi, nó cũng chuyển đổi nó thành một chuỗi, vì vậy bước tiếp theo sẽ như sau:

```js
      alert( "" + 1 ); // "1"
      alert( "1" + 1 ); // "11"
      alert( "1,2" + 1 ); // "1,21"
```

## Tóm lược

Array là một loại đối tượng đặc biệt, phù hợp để lưu trữ và quản lý các mục dữ liệu theo thứ tự.

- Khai báo:

    ```js
    // square brackets (usual)
    let arr = [item1, item2...];

    // new Array (exceptionally rare)
    let arr = new Array(item1, item2...);
    ```

    Lệnh gọi `new Array(number)` tạo ra một mảng có độ dài cho trước, nhưng không có phần tử.

- Thuộc tính `length` là độ dài mảng hoặc, chính xác hơn, chỉ số số cuối cùng của nó cộng với một. Nó được điều chỉnh tự động bằng các phương thức mảng. 

- Nếu chúng ta rút ngắn `length` bằng tay, mảng bị cắt ngắn.

Chúng ta có thể sử dụng một mảng như một deque với các hoạt động sau:

- `push(...items)` thêm `items' vào cuối.
- `pop()` loại bỏ phần tử từ cuối và trả về nó.
- `shift()` loại bỏ phần tử từ đầu và trả về nó.
- `unshift(...items)` thêm các mục vào đầu.

Để lặp qua các phần tử của mảng:

  - `for (let i=0; i<arr.length; i++)` -- hoạt động nhanh nhất, tương thích với trình duyệt cũ.
  - `for (let item of arr)` -- cú pháp hiện đại chỉ cho các mục,
  - `for (let i in arr)` -- không bao giờ sử dụng.

Chúng ta sẽ trở lại mảng và nghiên cứu thêm các phương thức để thêm, xóa, trích xuất các phần tử và sắp xếp các mảng trong chương **array-methods**.

