
# Lặp lại (Iterables)

Các đối tượng *iterable* là một tổng quát hóa (hoặc cũng gọi là sự suy rộng (generalization)) của các mảng. Đó là một khái niệm cho phép làm cho bất kỳ đối tượng nào có thể sử dụng được trong một vòng lặp `for..of`.

Tất nhiên, Mảng là lặp đi lặp lại. Nhưng có nhiều built-in objects khác, cũng có thể lặp lại. Chẳng hạn, Strings cũng có thể lặp lại. Như chúng ta sẽ thấy, nhiều built-in toán tử và phương thức dựa vào chúng.

Nếu một đối tượng đại diện cho một bộ sưu tập (danh sách, tập hợp) của một cái gì đó, thì `for..of` là một cú pháp tuyệt vời để lặp lại nó, vì vậy hãy xem cách làm cho nó làm việc.


## Symbol.iterator

Chúng ta có thể dễ dàng nắm bắt khái niệm lặp đi lặp lại (concept of iterables) bằng cách tạo ra một trong những cái chúng ta sở hữu.

Chẳng hạn, chúng ta có một đối tượng, đó không phải là một mảng, nhưng có vẻ phù hợp với `for..of`.

Giống như một đối tượng `range` đại diện cho một khoảng số:

```js
      let range = {
        from: 1,
        to: 5
      };

      // We want the for..of to work:
      // for(let num of range) ... num=1,2,3,4,5
```

Để làm cho `range` có thể lặp lại (và để cho `for..of` làm việc), chúng ta cần thêm một phương thức vào đối tượng có tên là `Symbol.iterator` (một built-in symbol đặc biệt chỉ dành cho điều đó).

1. Khi `for..of` bắt đầu, nó gọi phương thức đó một lần (hoặc lỗi nếu không tìm thấy). Phương thức phải trả về một *iterator* -- một đối tượng có phương thức `next`.
2. Tử đó trở đi, `for..of` làm việc *chỉ với đối tượng được trả về*.
3. Khi `for..of` muốn giá trị tiếp theo, nó gọi `next()` trên đối tượng đó.
4. Kết quả của `next()` phải có dạng `{done: Boolean, value: any}`, trong đó `done=true` có nghĩa là việc lặp lại đã kết thúc, nếu không thì `value` phải là giá trị mới.

Đây là cách thực hiện đầy đủ cho `range`:

```js
      let range = {
        from: 1,
        to: 5
      };

      // 1. call to for..of initially calls this
      range[Symbol.iterator] = function() {

        // ...it returns the iterator object:
        // 2. Onward, for..of works only with this iterator, asking it for next values
        return {
          current: this.from,
          last: this.to,      

          // 3. next() is called on each iteration by the for..of loop
          next() {
            // 4. it should return the value as an object {done:.., value :...}
            if (this.current <= this.last) {
              return { done: false, value: this.current++ };
            } else {
              return { done: true };
            }
          }
        };
      };

      // now it works!
      for (let num of range) {
        alert(num); // 1, then 2, 3, 4, 5
      }
```

Xin lưu ý tính năng cốt lõi của iterables: một [separation of concerns](https://en.wikipedia.org/wiki/Separation_of_concerns) quan trọng:

- Bản thân `range` không có phương thức `next()`.
- Thay vào đó, một đối tượng khác, cái gọi là "iterator" được tạo bởi lệnh gọi tới `range[Symbol.iterator]()` và nó xử lý toàn bộ lần lặp (iteration).

Vì vậy, đối tượng iterator tách biệt với đối tượng mà nó lặp đi lặp lại.

Về mặt kỹ thuật, chúng ta có thể hợp nhất chúng và sử dụng chính `range` làm trình lặp (iterator) để làm cho mã đơn giản hơn.

Như thế này:

```js
      let range = {
        from: 1,
        to: 5,

        [Symbol.iterator]() {
          this.current = this.from;
          return this;
        },

        next() {
          if (this.current <= this.to) {
            return { done: false, value: this.current++ };
          } else {
            return { done: true };
          }
        }
      };

      for (let num of range) {
        alert(num); // 1, then 2, 3, 4, 5
      }
```

Bây giờ `range[Symbol.iterator]()` trả về chính đối tượng `range`: nó có phương thức `next()` cần thiết và ghi nhớ tiến trình lặp hiện tại trong `this.current`. Ngắn hơn? Vâng. Và đôi khi điều đó cũng tốt.

Nhược điểm là bây giờ không thể có hai vòng lặp `for..of` chạy trên đối tượng cùng một lúc: chúng sẽ chia sẻ trạng thái lặp, bởi vì chỉ có một vòng lặp -- chính đối tượng đó. Nhưng hai for-ofs song song là một điều hiếm gặp, có thể được thực hiện với một số tình huống không đồng bộ.

<br>

> ---

**📌 Lặp lại vô hạn (Infinite iterators)**

Lặp đi lặp lại vô hạn cũng có thể. Chẳng hạn, `range` trở thành vô hạn với `range.to = Infinity`. Hoặc chúng ta có thể tạo một iterable object để tạo ra một chuỗi vô số các số giả ngẫu nhiên. Cũng có thể hữu ích.

Không có giới hạn nào về `next`, nó có thể trả về ngày càng nhiều giá trị, điều đó là bình thường.

Tất nhiên, the `for..of` loop trên một vòng lặp như vậy sẽ là vô tận. Nhưng chúng ta luôn có thể stop nó bằng cách sử dụng `break`.

> ---

<br>

## Chuỗi có thể lặp lại (String is iterable)

Mảng và chuỗi được sử dụng built-in iterables rộng rãi nhất.

Đối với một chuỗi, `for..of` lặp trên các ký tự của nó:

```js
      for (let char of "test") {
        // triggers 4 times: once for each character
        alert( char ); // t, then e, then s, then t
      }
```

Và nó hoạt động chính xác với các cặp thay thế (surrogate pairs)!

```js
      let str = '𝒳😂';
      for (let char of str) {
          alert( char ); // 𝒳, and then 😂
      }
```

## Gọi một trình lặp rõ ràng (iterator explicitly)

Thông thường, phần bên trong của iterables được ẩn khỏi mã bên ngoài. Có một vòng lặp `for..of`, hoạt động, đó là tất cả những gì nó cần biết.

Nhưng để hiểu mọi thứ sâu hơn một chút, hãy xem cách tạo ra một trình lặp một cách rõ ràng.

Chúng ta sẽ lặp lại một chuỗi giống như `for..of`, nhưng với các cuộc gọi trực tiếp. Mã này nhận được một trình vòng lặp chuỗi và gọi nó là "thủ công (manually)":

```js
      let str = "Hello";

      // does the same as
      // for (let char of str) alert(char);

      let iterator = str[Symbol.iterator]();

      while (true) {
        let result = iterator.next();
        if (result.done) break;
        alert(result.value); // outputs characters one by one
      }
```

Điều đó hiếm khi cần thiết, nhưng cho chúng ta nhiều quyền kiểm soát quá trình hơn là 'for..of`. Chẳng hạn, chúng ta có thể phân chia quá trình lặp: lặp lại một chút, sau đó dừng lại, làm một cái gì đó khác, và sau đó tiếp tục lại sau.

## Iterables and array-likes

Có hai thuật ngữ chính thức trông giống nhau, nhưng rất khác nhau. Hãy chắc chắn rằng bạn hiểu rõ về chúng để tránh nhầm lẫn.

- *Iterables* là các đối tượng triển khai phương thức `Symbol.iterator`, như được mô tả ở trên.
- **Array-likes* là các đối tượng có chỉ mục và `length`, vì vậy chúng trông giống như mảng.

Đương nhiên, các tính chất này có thể kết hợp. Chẳng hạn, các chuỗi đều có thể iterable (`for..of` hoạt động trên chúng) và array-like (chúng có các chỉ mục số và `length`).

Nhưng một iterable có thể không array-like. Và ngược lại, một array-like có thể không iterable.

Ví dụ, `range` trong ví dụ trên là có thể iterable, nhưng không array-like, vì nó không có các thuộc tính được lập chỉ mục và `length`.

Và đây là đối tượng array-like, nhưng không iterable:

```js
      let arrayLike = { // has indexes and length => array-like
        0: "Hello",
        1: "World",
        length: 2
      };

      // Error (no Symbol.iterator)
      for (let item of arrayLike) {}
```

Chúng có đặc điểm gì chung? Cả iterables và array-likes thường *không phải mảng*, chúng không có `push`, `pop`, v.v ... Điều đó khá bất tiện nếu chúng ta có một đối tượng như vậy và muốn làm việc với nó như với một mảng.

## Array.from

Có một phương thức phổ biến [Array.from](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/from) mang chúng lại với nhau. Nó nhận một giá trị iterable hoặc array-like và tạo ra một `Array` "thật" từ nó. Sau đó chúng ta có thể gọi các phương thức mảng trên nó.

Ví dụ:

```js
      let arrayLike = {
        0: "Hello",
        1: "World",
        length: 2
      };

      let arr = Array.from(arrayLike); // (*)
      alert(arr.pop()); // World (method works)
```

`Array.from` tại dòng `(*)` lấy đối tượng, kiểm tra xem nó có phải là dạng iterable hoặc array-like không, sau đó tạo một mảng mới và sao chép tất cả các items.

Điều tương tự cũng xảy ra đối với một iterable:

```js
      // assuming that range is taken from the example above
      let arr = Array.from(range);
      alert(arr); // 1,2,3,4,5 (array toString conversion works)
```

Cú pháp đầy đủ cho `Array.from` cho phép cung cấp một tùy chọn function "ánh xạ (mapping)":

```js
      Array.from(obj[, mapFn, thisArg])
```

Đối số thứ hai `mapFn` phải là hàm để áp dụng cho từng phần tử trước khi thêm vào mảng và `thisArg` cho phép đặt `this` cho nó.

Ví dụ:

```js
      // assuming that range is taken from the example above

      // square each number
      let arr = Array.from(range, num => num * num);

      alert(arr); // 1,4,9,16,25
      ```

      Ở đây chúng ta sử dụng `Array.from` để biến một chuỗi thành một mảng các ký tự:

      ```js
      let str = '𝒳😂';

      // splits str into array of characters
      let chars = Array.from(str);

      alert(chars[0]); // 𝒳
      alert(chars[1]); // 😂
      alert(chars.length); // 2
```

Không giống như `str.split`, nó dựa vào tính chất iterable của chuỗi và do đó, giống như `for..of`, hoạt động chính xác với các cặp thay thế.

Về mặt kỹ thuật, nó hoạt động giống như:

```js
      let str = '𝒳😂';

      let chars = []; // Array.from internally does the same loop
      for (let char of str) {
        chars.push(char);
      }

      alert(chars);
```

...Nhưng ngắn hơn.    

Chúng ta thậm chí có thể xây dựng surrogate-aware `slice` trên nó:

```js
      function slice(str, start, end) {
        return Array.from(str).slice(start, end).join('');
      }

      let str = '𝒳😂𩷶';

      alert( slice(str, 1, 3) ); // 😂𩷶

      // native method does not support surrogate pairs
      alert( str.slice(1, 3) ); // garbage (two pieces from different surrogate pairs)
```

## Tóm lược

Các đối tượng có thể được sử dụng trong `for..of` được gọi là *iterable*.

- Về mặt kỹ thuật, các iterables phải thực hiện phương thức có tên `Symbol.iterator`.
    - Kết quả của `obj[Symbol.iterator]` được gọi là *iterator*. Nó xử lý quá trình lặp đi lặp lại.
    - Trình lặp phải có phương thức có tên `next()` trả về một đối tượng `{done: Boolean, value: any}`, ở đây `done:true` biểu thị kết thúc lặp, nếu không thì `value` là giá trị tiếp theo.
- Phương thức `Symbol.iterator` được gọi tự động bởi `for..of`, nhưng chúng ta cũng có thể trực tiếp thực hiện nó.
- Các built-in iterables như chuỗi hoặc mảng, cũng triển khai `Symbol.iterator`.
- String iterator biết về các cặp thay thế.

Các đối tượng có thuộc tính được lập chỉ mục (indexed properties) và `length` được gọi là *array-like*. Các đối tượng như vậy cũng có thể có các thuộc tính và phương thức khác, nhưng thiếu các built-in methods của mảng.

Nếu chúng ta nhìn vào bên trong đặc tả -- chúng ta sẽ thấy rằng hầu hết các built-in methods đều cho rằng chúng hoạt động với các iterables hoặc array-likes thay vì mảng "thực", vì điều đó trừu tượng hơn.

`Array.from(obj[, mapFn, thisArg])` tạo một `Array` thật từ một iterable hoặc array-like `obj`, và chúng ta có thể sử dụng array methods trên nó. Các đối số tùy chọn `mapFn` và `thisArg` cho phép chúng ta áp dụng một hàm cho mỗi item.
