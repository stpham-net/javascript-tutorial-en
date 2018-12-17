
# Kiểu symbol

Theo đặc tả, các khóa thuộc tính đối tượng có thể là kiểu chuỗi hoặc kiểu symbol. Không phải số, không phải booleans, chỉ có chuỗi hoặc symbols, hai loại này.

Cho đến bây giờ chúng ta đã biết các chuỗi. Bây giờ hãy xem những lợi thế mà các symbol có thể mang lại cho chúng ta.

## Symbols

Giá trị "Symbol" đại diện cho một định danh độc nhất.

Một giá trị của kiểu này có thể được tạo bằng cách sử dụng `Symbol()`:

```js
      // id is a new symbol
      let id = Symbol();
```

Chúng ta cũng có thể cung cấp cho symbol một mô tả (còn được gọi là tên symbol), chủ yếu hữu ích cho mục đích gỡ lỗi:

```js
      // id is a symbol with the description "id"
      let id = Symbol("id");
```

Symbols được đảm bảo là độc nhất. Ngay cả khi chúng ta tạo ra nhiều symbols có cùng mô tả, chúng là các giá trị khác nhau. Mô tả chỉ là một label không ảnh hưởng đến bất cứ điều gì.

Chẳng hạn, đây là hai symbols có cùng mô tả - chúng không bằng nhau:

```js
      let id1 = Symbol("id");
      let id2 = Symbol("id");

      alert(id1 == id2); // false
```

Nếu bạn quen thuộc với Ruby hoặc một ngôn ngữ khác cũng có một số "symbols" - xin đừng nhầm lẫn. Ssymbols JavaScript là khác nhau.

<br>

> ---

**📌 Các Symbol không tự động chuyển đổi thành chuỗi**

Hầu hết các giá trị trong JavaScript đều hỗ trợ chuyển đổi ngầm định thành một chuỗi. Chẳng hạn, chúng ta có thể `alert` gần như bất kỳ giá trị nào, và nó sẽ hoạt động. Symbols là đặc biệt. Chúng không tự động chuyển đổi.

Chẳng hạn, `alert` này sẽ hiển thị lỗi:

```js
      let id = Symbol("id");
      alert(id); // TypeError: Cannot convert a Symbol value to a string
```

Nếu chúng ta thực sự muốn hiển thị một symbol, chúng ta cần gọi `.toString()` trên nó, như ở đây:

```js
      let id = Symbol("id");
      alert(id.toString()); // Symbol(id), now it works
```

Đó là một "người bảo vệ ngôn ngữ" chống lại sự lộn xộn, bởi vì các chuỗi và các symbol khác nhau về cơ bản và đôi khi không nên chuyển đổi cái này thành cái khác.

> ---

<br>

## Thuộc tính "Ẩn" ("Hidden" properties)

Các symbol cho phép chúng ta tạo các thuộc tính "ẩn" của một đối tượng, mà đôi khi không có phần nào khác của mã có thể truy cập hoặc ghi đè.

Chẳng hạn, nếu chúng ta muốn lưu trữ một "định danh" cho đối tượng `user`, chúng ta có thể sử dụng một symbol làm khóa cho nó:

```js
      let user = { name: "John" };
      let id = Symbol("id");

      user[id] = "ID Value";
      alert( user[id] ); // we can access the data using the symbol as the key
```

Lợi ích của việc sử dụng `Symbol("id")` thay vì một chuỗi `"id"` là gì?

Hãy làm cho ví dụ sâu hơn một chút để thấy điều đó.

Hãy tưởng tượng rằng một tập lệnh khác muốn có thuộc tính "id" của riêng nó bên trong `user`, cho mục đích riêng của nó. Đó có thể là một thư viện JavaScript khác, vì vậy các tập lệnh hoàn toàn không biết về nhau.

Sau đó, tập lệnh đó có thể tạo `Symbol("id")` của riêng nó, như thế này:

```js
      // ...
      let id = Symbol("id");

      user[id] = "Their id value";
```

Sẽ không có xung đột, bởi vì các symbol luôn khác nhau, ngay cả khi chúng có cùng tên.

Bây giờ lưu ý rằng nếu chúng ta sử dụng một chuỗi `"id"` thay vì một symbol cho cùng một mục đích, thì *sẽ* là một xung đột:

```js
      let user = { name: "John" };

      // our script uses "id" property
      user.id = "ID Value";

      // ...if later another script the uses "id" for its purposes...

      user.id = "Their id value"
      // boom! overwritten! it did not mean to harm the colleague, but did it!
```

### Symbols trong một literal

Nếu chúng ta muốn sử dụng một symbol trong một object literal, chúng ta cần dấu ngoặc vuông.

Như thế này:

```js
      let id = Symbol("id");

      let user = {
        name: "John",
        [id]: 123 // not just "id: 123"
      };
```
Đó là bởi vì chúng ta cần giá trị từ biến `id` làm khóa chứ không phải chuỗi "id".

### Symbols được bỏ qua bởi for..in

Các symbolic properties không tham gia vào vòng lặp `for..in`.

Ví dụ:

```js
      let id = Symbol("id");
      let user = {
        name: "John",
        age: 30,
        [id]: 123
      };

      for (let key in user) alert(key); // name, age (no symbols)

      // the direct access by the symbol works
      alert( "Direct: " + user[id] );
```

Đó là một phần của khái niệm "ẩn" nói chung. Nếu một script khác hoặc một thư viện lặp lại đối tượng của chúng ta, nó sẽ không truy cập bất ngờ vào một symbolic property.

Mặt khác, [Object.assign](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/assign) sao chép cả thuộc tính chuỗi và symbol:

```js
      let id = Symbol("id");
      let user = {
        [id]: 123
      };

      let clone = Object.assign({}, user);

      alert( clone[id] ); // 123
```

Không có nghịch lý ở đây. Đó là do thiết kế. Ý tưởng là khi chúng ta sao chép một đối tượng hoặc hợp nhất các đối tượng, chúng ta thường muốn *tất cả* các thuộc tính được sao chép (bao gồm các symbol như `id`).

<br>

> ---

**📌 Khóa thuộc tính của các kiểu khác bị ép buộc thành chuỗi**

Chúng ta chỉ có thể sử dụng chuỗi hoặc symbols làm khóa trong các đối tượng. Các kiểu khác được chuyển đổi thành chuỗi.

Chẳng hạn, một số `0` trở thành một chuỗi `"0"` khi được sử dụng làm khóa thuộc tính:

```js
      let obj = {
        0: "test" // same as "0": "test"
      };

      // both alerts access the same property (the number 0 is converted to string "0")
      alert( obj["0"] ); // test
      alert( obj[0] ); // test (same property)
```

> ---

<br>

## Global symbols

Như chúng ta đã thấy, thông thường tất cả các symbol đều khác nhau, ngay cả khi chúng có cùng tên. Nhưng đôi khi chúng ta muốn các symbol cùng tên là cùng một thực thể.

Chẳng hạn, các phần khác nhau trong ứng dụng của chúng ta muốn truy cập symbol `"id"` có nghĩa chính xác là cùng một thuộc tính.

Để đạt được điều đó, tồn tại một *global symbol registry*. Chúng ta có thể tạo các symbol trong đó và truy cập chúng sau này và nó đảm bảo rằng các truy cập lặp lại có cùng tên trả về chính xác cùng một symbol.

Để tạo hoặc đọc một symbol trong registry, hãy sử dụng `Symbol.for(key)`.

Cuộc gọi đó kiểm tra global registry và nếu có một symbol được mô tả là `key`, thì trả về nó, nếu không sẽ tạo một symbol mới `Symbol(key)` và lưu trữ nó trong registry bằng `key` đã cho.

Ví dụ:

```js
      // read from the global registry
      let id = Symbol.for("id"); // if the symbol did not exist, it is created

      // read it again
      let idAgain = Symbol.for("id");

      // the same symbol
      alert( id === idAgain ); // true
```

Các symbol bên trong registry được gọi là *global symbols*. Nếu chúng ta muốn một symbol trên toàn ứng dụng, có thể truy cập ở mọi nơi trong mã -- đó là những gì chúng dành cho việc đó.

<br>

> ---

**📌 Nghe giống Ruby**

Trong một số ngôn ngữ lập trình, như Ruby, có một symbol cho mỗi tên.

Trong JavaScript, như chúng ta có thể thấy, điều đó phù hợp với các global symbol.

> ---

<br>

### Symbol.keyFor

Đối với các global symbols, không chỉ `Symbol.for(key)` trả về một symbol theo tên, mà còn có một cuộc gọi ngược lại: `Symbol.keyFor(sym)`, thực hiện ngược lại: trả về tên theo global symbol.

Ví dụ:

```js
      let sym = Symbol.for("name");
      let sym2 = Symbol.for("id");

      // get name from symbol
      alert( Symbol.keyFor(sym) ); // name
      alert( Symbol.keyFor(sym2) ); // id
```

`Symbol.keyFor` trong nội bộ sử dụng global symbol registry để tra cứu khóa cho biểu symbol. Vì vậy, nó không hoạt động cho các non-global symbols. Nếu symbol không phải là global, nó sẽ không thể tìm thấy nó và trả về `undefined`.

Ví dụ:

```js
      alert( Symbol.keyFor(Symbol.for("name")) ); // name, global symbol

      alert( Symbol.keyFor(Symbol("name2")) ); // undefined, the argument isn't a global symbol
```

## System symbols

Tồn tại nhiều "system" symbols mà JavaScript sử dụng bên trong và chúng ta có thể sử dụng chúng để tinh chỉnh các khía cạnh khác nhau của các đối tượng.

Chúng được liệt kê trong đặc tả trong bảng [Symbols thường được biết tới](https://tc39.github.io/ecma262/#sec-well-known-symbols):

- `Symbol.hasInstance`
- `Symbol.isConcatSpreadable`
- `Symbol.iterator`
- `Symbol.toPrimitive`
- ...and so on.

Chẳng hạn, `Symbol.toPrimitive` cho phép chúng ta mô tả đối tượng để chuyển đổi nguyên thủy. Chúng ta sẽ thấy việc sử dụng nó rất sớm.

Các symbol khác cũng sẽ trở nên quen thuộc khi chúng ta nghiên cứu các tính năng ngôn ngữ tương ứng.

## Tóm lược

`Symbol` là một kiểu nguyên thủy cho các định danh duy nhất.

Các symbol được tạo bằng lệnh gọi `Symbol()` với một mô tả tùy chọn.

Các symbol luôn có giá trị khác nhau, ngay cả khi chúng có cùng tên. Nếu chúng ta muốn các symbol cùng tên bằng nhau, thì chúng ta nên sử dụng global registry: `Symbol.for(key)` trả về (tạo nếu cần) một global symbol với tên `key` làm tên. Nhiều cuộc gọi của `Symbol.for` trả về chính xác cùng một symbol.

Các symbol có hai trường hợp sử dụng chính:

1. Thuộc tính đối tượng "ẩn". Nếu chúng ta muốn thêm một thuộc tính vào một đối tượng "thuộc" một tập lệnh hoặc thư viện khác, chúng ta có thể tạo một symbol và sử dụng nó làm khóa thuộc tính. Một symbolic property không xuất hiện trong `for..in`, vì vậy nó sẽ không được liệt kê. Ngoài ra, nó sẽ không được truy cập trực tiếp, bởi vì tập lệnh khác không có symbol của chúng ta, vì vậy đôi khi nó sẽ không can thiệp vào những hành động của nó.

    Vì vậy, chúng ta có thể "ngấm ngầm" giấu thứ gì đó vào các đối tượng mà chúng ta cần, nhưng những người khác không nên nhìn thấy, sử dụng các symbolic properties.

2. Có nhiều system symbols được sử dụng bởi JavaScript có thể truy cập dưới dạng `Symbol.*`. Chúng ta có thể sử dụng chúng để thay đổi một số hành vi tích hợp. Chẳng hạn, sau này trong hướng dẫn, chúng ta sẽ sử dụng `Symbol.iterator` cho **iterable**, `Symbol.toPrimitive` để thiết lập **object-to-primitive conversion**, v.v.

Về mặt kỹ thuật, các symbol không được ẩn 100%. Có một phương thức tích hợp [Object.getOwnPropertySymbols(obj)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/getOwnPropertySymbols) cho phép chúng ta get tất cả các symbols. Ngoài ra, có một phương thức có tên [Reflect.ownKeys(obj)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Reflect/ownKeys) trả về *tất cả* khóa của một đối tượng bao gồm cả những symbolic. Vì vậy, chúng không thực sự ẩn. Nhưng hầu hết các thư viện, các phương thức tích hợp và cấu trúc cú pháp đều tuân thủ một thỏa thuận chung mà chúng là. Và người gọi rõ ràng các phương pháp đã nói ở trên có thể hiểu rõ những gì anh ta đang làm.
