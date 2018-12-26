
# Class patterns

> Trong lập trình hướng đối tượng, một *class* là program-code-template mở rộng để tạo đối tượng, cung cấp các giá trị ban đầu (initial values) cho trạng thái (state) (biến thành viên (member variables)) và triển khai hành vi (member functions hoặc methods). - Wikipedia

Có một special syntax construct và một từ khóa `class` trong JavaScript. Nhưng trước khi nghiên cứu nó, chúng ta nên xem xét thuật ngữ "class" xuất phát từ lý thuyết lập trình hướng đối tượng. Định nghĩa được trích dẫn ở trên, và nó độc lập với ngôn ngữ.

Trong JavaScript, có một số mẫu lập trình nổi tiếng để tạo các classes ngay cả khi không sử dụng từ khóa `class`. Và ở đây chúng ta sẽ nói về chúng trước.

Cấu trúc `class` sẽ được mô tả trong chương tiếp theo, nhưng trong JavaScript, đó là "syntax sugar" và là phần mở rộng của một trong các mẫu mà chúng ta sẽ nghiên cứu ở đây.

## Functional class pattern

The constructor function bên dưới có thể được coi là một "class" theo định nghĩa:

```js
function User(name) {
  this.sayHi = function() {
    alert(name);
  };
}

let user = new User("John");
user.sayHi(); // John
```

Nó tuân theo tất cả các phần của định nghĩa:

1. Nó là "program-code-template" để tạo các đối tượng (có thể gọi bằng `new`).
2. Nó cung cấp các giá trị ban đầu (initial values) cho trạng thái (state) (`name` từ các tham số).
3. Nó cung cấp các phương thức (`sayHi`).

Đây được gọi là *functional class pattern*.

Trong functional class pattern, các biến cục bộ và các hàm lồng nhau bên trong `User`, không được gán cho `this`, có thể nhìn thấy từ bên trong, nhưng mã bên ngoài không thể truy cập được.

Vì vậy, chúng ta có thể dễ dàng thêm các hàm và biến nội bộ, như `calcAge()` ở đây:

```js
function User(name, birthday) {
  // only visible from other methods inside User
  function calcAge() {
    return new Date().getFullYear() - birthday.getFullYear();
  }

  this.sayHi = function() {
    alert(`${name}, age:${calcAge()}`);
  };
}

let user = new User("John", new Date(2000, 0, 1));
user.sayHi(); // John, age:17
```

Trong mã này, các biến `name`, `birthday` và hàm `calcAge()` là nội bộ, *private* cho đối tượng. Chúng chỉ có thể nhìn thấy từ bên trong của nó.

Mặt khác, `sayHi` là bên ngoài, phương thức *công khai*. Mã bên ngoài tạo ra `user` có thể truy cập nó.

Bằng cách này, chúng ta có thể ẩn chi tiết triển khai nội bộ và các helper methods khỏi mã bên ngoài. Chỉ những gì được gán cho `this` mới được hiển thị bên ngoài.

## Factory class pattern

Chúng ta có thể tạo một lớp mà không cần sử dụng `new`.

Như thế này:

```js
function User(name, birthday) {
  // only visible from other methods inside User
  function calcAge() {
    return new Date().getFullYear() - birthday.getFullYear();
  }

  return {
    sayHi() {
      alert(`${name}, age:${calcAge()}`);
    }
  };
}

let user = User("John", new Date(2000, 0, 1));
user.sayHi(); // John, age:17
```

Như chúng ta có thể thấy, hàm `User` trả về một đối tượng có các thuộc tính và phương thức công khai. Lợi ích duy nhất của phương pháp này là chúng ta có thể bỏ qua `new`: viết `let user = User(...)` thay vì `let user = new User(...)`. Trong các khía cạnh khác, nó gần giống như functional pattern.

## Các lớp dựa trên nguyên mẫu (Prototype-based classes)

Các lớp dựa trên nguyên mẫu là quan trọng nhất và nói chung là tốt nhất. Các mẫu Functional và factory class hiếm khi được sử dụng trong thực tế.

Chẳng mấy chốc bạn sẽ thấy tại sao.

Đây là cùng một lớp được viết lại bằng cách sử dụng các prototypes:

```js
function User(name, birthday) {
  this._name = name;
  this._birthday = birthday;
}

User.prototype._calcAge = function() {
  return new Date().getFullYear() - this._birthday.getFullYear();
};

User.prototype.sayHi = function() {
  alert(`${this._name}, age:${this._calcAge()}`);
};

let user = new User("John", new Date(2000, 0, 1));
user.sayHi(); // John, age:17
```

Cấu trúc mã:

- The constructor `User` chỉ khởi tạo trạng thái đối tượng hiện tại.
- Các phương thức được thêm vào `User.prototype`.

Như chúng ta có thể thấy, các phương thức không phải là lexically bên trong `function User`, chúng không chia sẻ một lexical environment chung. Nếu chúng ta khai báo các biến bên trong `function User`, thì chúng sẽ không hiển thị cho các phương thức.

Vì vậy, có một thỏa thuận được biết đến rộng rãi rằng các thuộc tính và phương thức nội bộ được thêm vào một dấu gạch dưới `"_"`. Giống như `_name` hoặc `_calcAge()`. Về mặt kỹ thuật, đó chỉ là một thỏa thuận, mã bên ngoài vẫn có thể truy cập chúng. Nhưng hầu hết các nhà phát triển nhận ra ý nghĩa của `"_"` và cố gắng không chạm vào các thuộc tính và phương thức có tiền tố trong mã bên ngoài.

Dưới đây là những ưu điểm so với functional pattern:

- Trong functional pattern, mỗi đối tượng có bản sao riêng của mọi phương thức. Chúng ta gán một bản sao riêng của `this.sayHi = function() {...}` và các phương thức khác trong constructor.
- Trong prototypal pattern, tất cả các phương thức đều nằm trong `User.prototype` được chia sẻ giữa tất cả các user objects. Một đối tượng chỉ lưu trữ dữ liệu.

Vì vậy, prototypal pattern là bộ nhớ hiệu quả (memory-efficient) hơn.

...Nhưng không chỉ có thế. Prototypes cho phép chúng ta thiết lập sự kế thừa một cách thực sự hiệu quả. Tất cả các built-in JavaScript objects đều sử dụng các prototypes. Ngoài ra còn có một cấu trúc cú pháp đặc biệt: "class" cung cấp cú pháp đẹp mắt cho chúng. Và còn nhiều nữa, vì vậy hãy tiếp tục với chúng.

## Kế thừa dựa trên nguyên mẫu cho các lớp (Prototype-based inheritance for classes)

Giả sử chúng ta có hai lớp dựa trên nguyên mẫu (prototype-based classes).

`Rabbit`:

```js
function Rabbit(name) {
  this.name = name;
}

Rabbit.prototype.jump = function() {
  alert(`${this.name} jumps!`);
};

let rabbit = new Rabbit("My rabbit");
```

![](rabbit-animal-independent-1.png)

...Và `Animal`:

```js
function Animal(name) {
  this.name = name;
}

Animal.prototype.eat = function() {
  alert(`${this.name} eats.`);
};

let animal = new Animal("My animal");
```

![](rabbit-animal-independent-2.png)

Ngay bây giờ chúng hoàn toàn độc lập.

Nhưng chúng ta muốn `Rabbit` mở rộng `Animal`. Nói cách khác, thỏ nên dựa vào động vật, có quyền truy cập vào các phương thức của `Animal` và mở rộng chúng bằng các phương thức riêng của nó.

Nó có nghĩa gì trong ngôn ngữ của các nguyên mẫu?

Ngay bây giờ các phương thức cho các đối tượng `rabbit` nằm trong `Rabbit.prototype`. Chúng ta muốn `rabbit` sử dụng `Animal.prototype` làm "dự phòng", nếu phương thức không được tìm thấy trong `Rabbit.prototype`.

Vì vậy, prototype chain phải là `rabbit` -> `Rabbit.prototype` -> `Animal.prototype`.

Như thế này:

![](class-inheritance-rabbit-animal.png)

Mã để thực hiện điều đó:

```js
// Same Animal as before
function Animal(name) {
  this.name = name;
}

// All animals can eat, right?
Animal.prototype.eat = function() {
  alert(`${this.name} eats.`);
};

// Same Rabbit as before
function Rabbit(name) {
  this.name = name;
}

Rabbit.prototype.jump = function() {
  alert(`${this.name} jumps!`);
};

// setup the inheritance chain
Rabbit.prototype.__proto__ = Animal.prototype; // (*)

let rabbit = new Rabbit("White Rabbit");
rabbit.eat(); // rabbits can eat too
rabbit.jump();
```

Dòng `(*)` thiết lập prototype chain. Vì vậy, `rabbit` đầu tiên tìm kiếm các phương thức trong `Rabbit.prototype`, sau đó là `Animal.prototype`. Và sau đó, để hoàn thiện, hãy đề cập rằng nếu phương thức không được tìm thấy trong `Animal.prototype`, thì tìm kiếm tiếp tục trong `Object.prototype`, vì `Animal.prototype` là một đối tượng đơn giản thông thường, do đó, nó thừa kế từ nó.

Vì vậy, đây là hình ảnh đầy đủ:

![](class-inheritance-rabbit-animal-2.png)

## Tóm lược

Thuật ngữ "class" xuất phát từ lập trình hướng đối tượng. Trong JavaScript nó thường có nghĩa là functional class pattern hoặc the prototypal pattern. The prototypal pattern mạnh hơn và tiết kiệm bộ nhớ hơn, do đó, nên sử dụng mẫu này.

Theo prototypal pattern:
1. Các phương thức được lưu trữ trong `Class.prototype`.
2. Prototypes kế thừa từ nhau.

Trong chương tiếp theo, chúng ta sẽ nghiên cứu từ khóa `class` và construct. Nó cho phép viết các prototypal classes ngắn hơn và cung cấp một số lợi ích bổ sung.
