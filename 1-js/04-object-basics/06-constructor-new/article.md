# Constructor, operator "new"

Cú pháp `{...}` thông thường cho phép tạo một đối tượng. Nhưng thường thì chúng ta cần tạo nhiều đối tượng tương tự, như nhiều người dùng hoặc các mục menu, v.v.

Điều đó có thể được thực hiện bằng cách sử dụng các hàm tạo và toán tử `"new"`.

## Constructor function

Constructor functions về mặt kỹ thuật là các hàm thông thường. Có hai quy ước:

1. Chúng được đặt tên bằng chữ in hoa đầu tiên.
2. Chúng chỉ nên được thực hiện với toán tử `"new"`.

Ví dụ:

```js
      function User(name) {
        this.name = name;
        this.isAdmin = false;
      }

      let user = new User("Jack");

      alert(user.name); // Jack
      alert(user.isAdmin); // false
```

Khi một hàm được thực thi là `new User(...)`, nó sẽ thực hiện các bước sau:

1. Một empty object mới được tạo và gán cho `this`.
2. Các function body thực thi. Thông thường nó sửa đổi `this`, thêm các thuộc tính mới cho nó.
3. Giá trị của `this` được trả về.

Nói cách khác, `new User(...)` thực hiện một số thứ như:

```js
      function User(name) {
        // this = {};  (implicitly)

        // add properties to this
        this.name = name;
        this.isAdmin = false;

        // return this;  (implicitly)
      }
```

Vì vậy, kết quả của `new User("Jack")` là cùng một đối tượng như:

```js
      let user = {
        name: "Jack",
        isAdmin: false
      };
```

Bây giờ nếu chúng ta muốn tạo người dùng khác, chúng ta có thể gọi `new User("Ann")`, `new User("Alice")`, v.v. Ngắn hơn nhiều so với việc sử dụng literals mỗi lần, và cũng dễ đọc.

Đó là mục đích chính của các constructors -- để thực hiện mã tạo đối tượng có thể sử dụng lại.

Chúng ta hãy lưu ý một lần nữa -- về mặt kỹ thuật, bất kỳ function nào cũng có thể được sử dụng như một constructor. Tức là: bất kỳ hàm nào cũng có thể được chạy với `new` và nó sẽ thực thi thuật toán ở trên. "Chữ hoa đầu tiên" là một thỏa thuận chung, để làm rõ rằng một function sẽ được chạy với `new`.

<br>

> ---

**📌 new function() { ... }**

Nếu chúng ta có nhiều dòng mã về việc tạo một đối tượng phức tạp duy nhất, chúng ta có thể gói chúng trong hàm tạo (constructor function), như thế này:

```js
      let user = new function() {
        this.name = "John";
        this.isAdmin = false;

        // ...other code for user creation
        // maybe complex logic and statements
        // local variables etc
      };
```

Constructor này không thể được gọi lại, bởi vì nó không được lưu ở bất cứ đâu, chỉ đã được tạo và được gọi. Vì vậy, thủ thuật này nhằm mục đích đóng gói mã xây dựng đối tượng duy nhất, mà không sử dụng lại trong tương lai.

> ---

<br>

## Dual-syntax constructors: new.target

<br>

> ---

**📌 Nội dung nâng cao**

Cú pháp từ phần này hiếm khi được sử dụng, bỏ qua nó trừ khi bạn muốn biết mọi thứ.

> ---

<br>

Bên trong một hàm, chúng ta có thể kiểm tra xem nó được gọi với `new` hay không, bằng cách sử dụng thuộc tính `new.target` đặc biệt.

Nó empty đối với các cuộc gọi thông thường và bằng với hàm nếu được gọi với `new`:

```js
      function User() {
        alert(new.target);
      }

      // without "new":
      User(); // undefined

      // with "new":
      new User(); // function User { ... }
```

Điều đó có thể được sử dụng để cho phép cả hai cuộc gọi `new` và cuộc gọi thông thường hoạt động như nhau. Đó là, tạo cùng một đối tượng:

```js
      function User(name) {
        if (!new.target) { // if you run me without new
          return new User(name); // ...I will add new for you
        }

        this.name = name;
      }

      let john = User("John"); // redirects call to new User
      alert(john.name); // John
```

Cách tiếp cận này đôi khi được sử dụng trong các thư viện để làm cho cú pháp linh hoạt hơn. Vì vậy, mọi người có thể gọi hàm có hoặc không có `new` và nó vẫn hoạt động.

Có lẽ không phải là một thứ tốt để sử dụng ở mọi nơi, bởi vì việc bỏ qua `new` làm cho nó ít rõ ràng hơn những gì đang diễn ra. Với `new` chúng ta đều biết rằng đối tượng mới đang được tạo.

## Return from constructors

Thông thường, các constructor không có câu lệnh `return`. Nhiệm vụ của họ là viết tất cả những thứ cần thiết vào `this`, và nó tự động trở thành kết quả.

Nhưng nếu có câu lệnh `return`, thì quy tắc rất đơn giản:

- Nếu `return` được gọi với đối tượng, thì nó được trả về thay vì `this`.
- Nếu `return` được gọi với một nguyên thủy, nó sẽ bị bỏ qua.

Nói cách khác, `return` với một đối tượng trả về đối tượng đó, trong tất cả các trường hợp khác `this` được trả về.

Chẳng hạn, ở đây `return` ghi đè `this` bằng cách trả về một đối tượng:

```js
      function BigUser() {

        this.name = "John";

        return { name: "Godzilla" };  // <-- returns an object
      }

      alert( new BigUser().name );  // Godzilla, got that object ^^
```

Và đây là một ví dụ với một empty `return` (hoặc chúng ta có thể đặt một nguyên thủy sau nó, không thành vấn đề):

```js
      function SmallUser() {

        this.name = "John";

        return; // finishes the execution, returns this

        // ...

      }

      alert( new SmallUser().name );  // John
```

Thông thường các constructors không có câu lệnh `return`. Ở đây chúng ta đề cập đến hành vi đặc biệt với các đối tượng returning chủ yếu vì mục đích hoàn chỉnh.

<br>

> ---

**📌 Bỏ dấu ngoặc đơn (Omitting parentheses)**

Nhân tiện, chúng ta có thể bỏ qua dấu ngoặc đơn sau `new`, nếu nó không có đối số:

```js
      let user = new User; // <-- no parentheses
      // same as
      let user = new User();
```

Bỏ dấu ngoặc đơn ở đây không được coi là "good style", nhưng cú pháp được đặc tả cho phép.

> ---

<br>

## Các phương thức trong constructor

Sử dụng các constructor functions để tạo các đối tượng mang lại sự linh hoạt cao. The constructor function có thể có các tham số xác định cách xây dựng (construct) đối tượng và những gì cần đặt trong đó.

Tất nhiên, chúng ta có thể thêm vào `this` không chỉ các thuộc tính, mà cả các phương thức.

Chẳng hạn, `new User(name)` bên dưới tạo một đối tượng với `name` đã cho và phương thức `sayHi`:

```js
      function User(name) {
        this.name = name;

        this.sayHi = function() {
          alert( "My name is: " + this.name );
        };
      }

      let john = new User("John");

      john.sayHi(); // My name is: John

      /*
      john = {
         name: "John",
         sayHi: function() { ... }
      }
      */
```

## Tóm lược

- Constructor functions hoặc, ngắn gọn, constructors, là các hàm thông thường, nhưng trước tiên có một thỏa thuận chung để đặt tên chúng bằng chữ in hoa.
- Các constructor functions chỉ nên được gọi bằng cách sử dụng `new`. Một cuộc gọi như vậy ngụ ý việc tạo ra `this` trống lúc bắt đầu và trả lại cái được điền vào cuối.

Chúng ta có thể sử dụng các constructor functions để tạo nhiều đối tượng tương tự.

JavaScript cung cấp các constructor functions cho nhiều built-in language objects: như `Date` cho ngày, `Set` cho các tập hợp và các cái khác mà chúng ta dự định sẽ nghiên cứu.

<br>

> ---

**📌 Đối tượng, chúng ta sẽ trở lại!**

Trong chương này, chúng ta chỉ đề cập đến những điều cơ bản về các đối tượng và các constructors. Chúng rất cần thiết để tìm hiểu thêm về các kiểu dữ liệu và functions trong các chương tiếp theo.

Sau khi chúng ta biết điều đó, trong chương **lập trình hướng đối tượng** chúng ta quay trở lại các đối tượng và trình bày chúng sâu hơn, bao gồm cả kế thừa và các lớp.

> ---
