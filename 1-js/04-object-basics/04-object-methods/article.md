# Object methods, "this"

Các đối tượng thường được tạo để đại diện cho các thực thể của thế giới thực, như người dùng, đơn đặt hàng, v.v.

```js
      let user = {
        name: "John",
        age: 30
      };
```

Và, trong thế giới thực, người dùng có thể *hành động*: chọn thứ gì đó từ giỏ hàng, đăng nhập, đăng xuất, v.v.

Các hành động được thể hiện bằng JavaScript bởi các hàm trong thuộc tính.

## Ví dụ về phương thức

Để bắt đầu, hãy dạy `user` nói xin chào:

```js
      let user = {
        name: "John",
        age: 30
      };

      user.sayHi = function() {
        alert("Hello!");
      };

      user.sayHi(); // Hello!
```

Ở đây chúng ta vừa sử dụng Biểu thức hàm (Function Expression) để tạo hàm và gán nó cho thuộc tính `user.sayHi` của đối tượng.

Sau đó chúng ta có thể gọi nó. Người dùng bây giờ có thể nói!

Một hàm là thuộc tính của một đối tượng được gọi là *phương thức (method)* của nó.

Vì vậy, ở đây chúng ta đã có một phương thức `sayHi` của đối tượng `user`.

Tất nhiên, chúng ta có thể sử dụng hàm được khai báo trước như một phương thức, như thế này:

```js
      let user = {
        // ...
      };

      // first, declare
      function sayHi() {
        alert("Hello!");
      };

      // then add as a method
      user.sayHi = sayHi;

      user.sayHi(); // Hello!
```

<br>

> ---

**📌 Lập trình hướng đối tượng (Object-oriented programming)**

Khi chúng ta viết mã bằng cách sử dụng các đối tượng để biểu diễn các thực thể, đó gọi là [lập trình hướng đối tượng](https://en.wikipedia.org/wiki/Object-oriented_programming), nói ngắn gọn: "OOP".

OOP là một thứ lớn, một khoa học thú vị của riêng nó. Làm thế nào để chọn đúng thực thể? Làm thế nào để tổ chức sự tương tác giữa chúng? Đó là kiến trúc, và có những cuốn sách hay về chủ đề đó, như "Các mẫu thiết kế: Các yếu tố của phần mềm hướng đối tượng có thể tái sử dụng" của E.Gamma, R.Helm, R.Johnson, J.Vissides hoặc "Phân tích và thiết kế hướng đối tượng với Ứng dụng" của G.Booch, và nhiều hơn nữa. Chúng ta sẽ xem xét bề mặt của chủ đề đó sau trong chương **lập trình hướng đối tượng**.

> ---

<br>

### Method shorthand

Tồn tại một cú pháp ngắn hơn cho các phương thức trong một object literal:

```js
      // these objects do the same

      let user = {
        sayHi: function() {
          alert("Hello");
        }
      };

      // method shorthand looks better, right?
      let user = {
        sayHi() { // same as "sayHi: function()"
          alert("Hello");
        }
      };
```

Như đã trình bày, chúng ta có thể bỏ qua `"function"` và chỉ viết `sayHi()`.

Nói thật, các ký hiệu không hoàn toàn giống nhau. Có những khác biệt tinh tế liên quan đến kế thừa đối tượng (sẽ được đề cập sau), nhưng bây giờ chúng không quan trọng. Trong hầu hết các trường hợp, cú pháp ngắn hơn được ưa thích.

## "this" in methods

Điều phổ biến là một object method cần truy cập vào thông tin được lưu trữ trong đối tượng để thực hiện công việc của nó.

Ví dụ, mã bên trong `user.sayHi()` có thể cần tên của `user`.

**Để truy cập đối tượng, một phương thức có thể sử dụng từ khóa `this`.**

Giá trị của `this` là đối tượng "trước dấu chấm", đối tượng được sử dụng để gọi phương thức.

Ví dụ:

```js
      let user = {
        name: "John",
        age: 30,

        sayHi() {
          alert(this.name);
        }

      };

      user.sayHi(); // John
```

Ở đây trong quá trình thực thi `user.sayHi()`, giá trị của `this` sẽ là `user`.

Về mặt kỹ thuật, bạn cũng có thể truy cập vào đối tượng mà không cần `this`, bằng cách tham chiếu nó qua biến ngoài:

```js
      let user = {
        name: "John",
        age: 30,

        sayHi() {
          alert(user.name); // "user" instead of "this"
        }

      };
```

...Nhưng mã như vậy là không đáng tin cậy. Nếu chúng ta quyết định sao chép `user` sang một biến khác, ví dụ `admin = user` và ghi đè `user` bằng một thứ khác, thì nó sẽ truy cập sai đối tượng.

Điều đó đã được chứng minh dưới đây:

```js
      let user = {
        name: "John",
        age: 30,

        sayHi() {
          alert( user.name ); // leads to an error
        }

      };


      let admin = user;
      user = null; // overwrite to make things obvious

      admin.sayHi(); // Whoops! inside sayHi(), the old name is used! error!
```

Nếu chúng ta đã sử dụng `this.name` thay vì `user.name` bên trong `alert`, thì mã sẽ hoạt động.

## "this" không bị ràng buộc

Trong JavaScript, từ khóa "this" hoạt động không giống như hầu hết các ngôn ngữ lập trình khác. Đầu tiên, nó có thể được sử dụng trong bất kỳ function.

Không có lỗi cú pháp trong mã như thế:

```js
      function sayHi() {
        alert( this.name );
      }
```

Giá trị của `this` được ước tính trong thời gian chạy. Và nó có thể là bất cứ điều gì.

Chẳng hạn, cùng một hàm có thể có "this" khác nhau khi được gọi từ các đối tượng khác nhau:

```js
      let user = { name: "John" };
      let admin = { name: "Admin" };

      function sayHi() {
        alert( this.name );
      }

      // use the same functions in two objects
      user.f = sayHi;
      admin.f = sayHi;

      // these calls have different this
      // "this" inside the function is the object "before the dot"
      user.f(); // John  (this == user)
      admin.f(); // Admin  (this == admin)

      admin['f'](); // Admin (dot or square brackets access the method – doesn't matter)
```

Trên thực tế, chúng ta có thể gọi hàm mà không cần một đối tượng nào cả:

```js
      function sayHi() {
        alert(this);
      }

      sayHi(); // undefined
```

Trong trường hợp này `this` là `undefined` trong strict mode. Nếu chúng ta cố gắng truy cập `this.name`, sẽ có một lỗi.

Trong chế độ không nghiêm ngặt (nếu người ta quên `use strict`), giá trị của `this` trong trường hợp đó sẽ là *global object* (`window` trong trình duyệt, chúng ta sẽ tìm hiểu sau). Đây là một hành vi lịch sử mà `"use strict"` sửa lỗi.

Xin lưu ý rằng thông thường một cuộc gọi của một hàm sử dụng `this` mà không có đối tượng là không bình thường, mà là một lỗi lập trình. Nếu một hàm có `this`, thì nó thường được gọi trong ngữ cảnh của một đối tượng.

<br>

> ---

**📌 Hậu quả của việc không ràng buộc (unbound) `this`**

Nếu bạn đến từ một ngôn ngữ lập trình khác, thì có lẽ bạn đã quen với ý tưởng về "ràng buộc `this`", trong đó các phương thức được định nghĩa trong một đối tượng luôn có `this` tham chiếu đến đối tượng đó.

Trong JavaScript `this` là "free", giá trị của nó được đánh giá tại thời điểm cuộc gọi và không phụ thuộc vào nơi phương thức được khai báo, mà phụ thuộc vào đối tượng "trước dấu chấm".

Khái niệm thời gian chạy (run-time) được đánh giá `this` có cả ưu điểm và nhược điểm. Một mặt, một function có thể được sử dụng lại cho các đối tượng khác nhau. Mặt khác, sự linh hoạt cao hơn mở ra một nơi cho những sai lầm.

Ở đây, quan điểm của chúng ta là không đánh giá liệu quyết định thiết kế ngôn ngữ này là tốt hay xấu. Chúng ta sẽ hiểu cách làm việc với nó, làm thế nào để có được lợi ích và tránh các vấn đề.

> ---

<br>

## Nội bộ: Kiểu tham chiếu (Internals: Reference Type)

<br>

> ---

**📌 Tính năng ngôn ngữ chuyên sâu (In-depth language feature)**

Phần này bao gồm một chủ đề nâng cao, để hiểu rõ hơn các trường hợp góc cạnh nhất định.

Nếu bạn muốn tiếp tục nhanh hơn, nó có thể được bỏ qua hoặc hoãn lại.

> ---

<br>

Ví dụ, một cuộc gọi phương thức phức tạp có thể mất `this`:

```js
      let user = {
        name: "John",
        hi() { alert(this.name); },
        bye() { alert("Bye"); }
      };

      user.hi(); // John (the simple call works)

      // now let's call user.hi or user.bye depending on the name
      (user.name == "John" ? user.hi : user.bye)(); // Error!
```

Trên dòng cuối cùng có một toán tử ternary chọn `user.hi` hoặc `user.bye`. Trong trường hợp này, kết quả là `user.hi`.

Phương thức này được gọi ngay với dấu ngoặc đơn `()`. Nhưng nó không hoạt động đúng!

Bạn có thể thấy rằng kết quả cuộc gọi bị lỗi, khiến giá trị của `"this"` bên trong cuộc gọi trở thành `undefined`.

Điều này hoạt động (object dot method):

```js
      user.hi();
```

Điều này không (evaluated method):

```js
      (user.name == "John" ? user.hi : user.bye)(); // Error!
```

Tại sao? Nếu chúng ta muốn hiểu lý do tại sao nó xảy ra, hãy tìm hiểu cách thức hoạt động của cuộc gọi `obj.method()`.

Nhìn kỹ, chúng ta có thể nhận thấy hai thao tác trong câu lệnh `obj.method()`:

1. Đầu tiên, dấu chấm `'.'` lấy thuộc tính `obj.method`.
2. Sau đó, dấu ngoặc đơn `()` thực thi nó.

Vậy, làm thế nào để thông tin về 'this' được chuyển từ phần thứ nhất sang phần thứ hai?

Nếu chúng ta đặt các hoạt động này trên các dòng riêng biệt, thì `this` sẽ bị mất chắc chắn:

```js
      let user = {
        name: "John",
        hi() { alert(this.name); }
      }

      // split getting and calling the method in two lines
      let hi = user.hi;
      hi(); // Error, because this is undefined
```

Ở đây `hi = user.hi` đặt hàm vào biến, và trên dòng cuối cùng, nó hoàn toàn độc lập, và vì vậy không có `this`.

**Để thực hiện được các cuộc gọi `user.hi()`, JavaScript sử dụng một mẹo -- dấu chấm `'.'` trả về không phải là hàm, mà là một giá trị của [Kiểu tham chiếu (Reference Type)](https://tc39.github.io/ecma262/#sec-reference-specification-type) đặc biệt.**

Kiểu tham chiếu là một "kiểu đặc tả". Chúng ta không thể sử dụng nó một cách rõ ràng, nhưng nó được sử dụng nội bộ bởi ngôn ngữ.

Giá trị của Kiểu tham chiếu (Reference Type) là kết hợp ba giá trị `(base, name, strict)`, trong đó:

- `base` là đối tượng.
- `name` là thuộc tính.
- `strict` là đúng nếu `use strict` có hiệu lực.

Kết quả của một truy cập thuộc tính `user.hi` không phải là một hàm, mà là một giá trị của Kiểu tham chiếu. Đối với `user.hi` trong chế độ nghiêm ngặt, đó là:

```js
      // Reference Type value
      (user, "hi", true)
```

Khi các dấu ngoặc đơn `()` được gọi trên Kiểu tham chiếu (Reference Type), chúng sẽ nhận được thông tin đầy đủ về đối tượng và phương thức của nó và có thể đặt quyền `this` (`=user` trong trường hợp này).

Bất kỳ hoạt động nào khác như gán `hi = user.hi` sẽ loại bỏ toàn bộ kiểu tham chiếu (reference type), rồi lấy giá trị của `user.hi` (một hàm) và chuyển nó vào. Vì vậy, bất kỳ hoạt động tiếp theo "mất" `this`.

Vì vậy, kết quả là, giá trị của `this` chỉ được truyền đúng cách nếu hàm được gọi trực tiếp bằng cách sử dụng cú pháp `obj.method()` hoặc dấu ngoặc vuông `obj['method']()` (chúng làm tương tự nhau). Về sau trong loạt hướng dẫn này, chúng ta sẽ tìm hiểu nhiều cách khác nhau để giải quyết vấn đề này, chẳng hạn như **func.bind()**.

## Hàm mũi tên không có "this"

Các hàm mũi tên rất đặc biệt: chúng không có `this` của riêng chúng. Nếu chúng ta tham chiếu `this` từ một hàm như vậy, thì nó được lấy từ hàm "bình thường" bên ngoài.

Chẳng hạn, ở đây `arrow()` sử dụng `this` từ phương thức `user.sayHi()` bên ngoài:

```js
      let user = {
        firstName: "Ilya",
        sayHi() {
          let arrow = () => alert(this.firstName);
          arrow();
        }
      };

      user.sayHi(); // Ilya
```

Đó là một tính năng đặc biệt của các hàm mũi tên, nó hữu ích khi chúng ta thực sự không muốn có một `this` riêng biệt, mà là để lấy nó từ bối cảnh bên ngoài. Ở phần sau của chương **các hàm mũi tên (arrow-functions)** chúng ta sẽ đi sâu hơn vào các hàm mũi tên.

## Tóm lược

- Các hàm được lưu trữ trong các thuộc tính đối tượng được gọi là "phương thức".
- Các phương thức cho phép các đối tượng "hành động" như `object.doSomething()`.
- Các phương thức có thể tham chiếu đối tượng bằng `this`.

Giá trị của `this` được định nghĩa tại thời gian chạy (run-time).
- Khi một hàm được khai báo, nó có thể sử dụng `this`, nhưng `this` không có giá trị cho đến khi hàm được gọi.
- Function đó có thể được sao chép giữa các đối tượng.
- Khi một hàm được gọi theo cú pháp "phương thức": `object.method()`, giá trị của `this` trong khi gọi là `object`.

Xin lưu ý rằng các hàm mũi tên là đặc biệt: chúng không có `this`. Khi `this` được truy cập bên trong một hàm mũi tên, nó được lấy từ bên ngoài.
