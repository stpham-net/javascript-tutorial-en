
# Classes

Cấu trúc "class" cho phép định nghĩa các prototype-based classes với cú pháp rõ ràng, đẹp mắt.

## The "class" syntax

Cú pháp `class` rất linh hoạt, trước tiên chúng ta sẽ bắt đầu với một ví dụ đơn giản.

Đây là một prototype-based class `User`:

```js
function User(name) {
  this.name = name;
}

User.prototype.sayHi = function() {
  alert(this.name);
}

let user = new User("John");
user.sayHi();
```

...Và điều đó cũng tương tự khi sử dụng cú pháp `class`:

```js
class User {

  constructor(name) {
    this.name = name;
  }

  sayHi() {
    alert(this.name);
  }

}

let user = new User("John");
user.sayHi();
```

Thật dễ dàng để thấy rằng hai ví dụ giống nhau. Chỉ xin lưu ý rằng các phương thức trong một class không có dấu phẩy giữa chúng. Các nhà phát triển tập sự đôi khi quên nó và đặt dấu phẩy giữa các phương thức lớp và như vậy không hoạt động. Đó không phải là một literal object, mà là một class syntax.

Vậy, chính xác thì `class` làm gì? Chúng ta có thể nghĩ rằng nó định nghĩa một thực thể cấp ngôn ngữ (language-level) mới, nhưng điều đó là sai.

`class User {...}` ở đây thực sự có hai điều:

1. Khai báo một biến `User` tham chiếu đến hàm có tên `"constructor"`.
2. Đặt các phương thức được liệt kê trong định nghĩa vào `User.prototype`. Ở đây, nó bao gồm `sayHi` và `constructor`.

Đây là mã để đào sâu vào class và thấy rằng:

```js
class User {
  constructor(name) { this.name = name; }
  sayHi() { alert(this.name); }
}

// proof: User is the "constructor" function
alert(User === User.prototype.constructor); // true

// proof: there are two methods in its "prototype"
alert(Object.getOwnPropertyNames(User.prototype)); // constructor, sayHi
```

Dưới đây là minh họa về những gì `class User` tạo ra:

![](class-user.png)

Vì vậy, `class` là một cú pháp đặc biệt để định nghĩa một constructor cùng với các phương thức prototype của nó.

...Nhưng không chỉ có thế. Có một số điều chỉnh nhỏ ở đây và đó:

**Constructors require `new`**

Không giống như một hàm thông thường, một class `constructor` không thể được gọi mà không có `new`:

```js
class User {
  constructor() {}
}

alert(typeof User); // function
User(); // Error: Class constructor User cannot be invoked without 'new'
```

**Different string output**

Nếu chúng ta xuất nó như `alert(User)`, một số engines hiển thị `"class User..."`, trong khi những cái khác hiển thị `"function User..."`.

Xin đừng nhầm lẫn: biểu diễn chuỗi có thể khác nhau, nhưng đó vẫn là một hàm, không có thực thể "class" riêng biệt trong ngôn ngữ JavaScript.

**Class methods are non-enumerable**

Một định nghĩa class đặt cờ `enumerable` thành `false` cho tất cả các phương thức trong `"prototype"`. Điều đó tốt, bởi vì nếu chúng ta `for..in` qua một đối tượng, chúng ta thường không muốn các phương thức class của nó.

**Classes have a default `constructor() {}`**

Nếu không có `constructor` trong cấu trúc `class`, thì một hàm rỗng được tạo ra, giống như khi chúng ta đã viết `constructor() {}`.

**Classes always `use strict`**

Tất cả các mã bên trong cấu trúc class được tự động ở chế độ nghiêm ngặt.

### Getters/setters

Các classes cũng có thể bao gồm getters/setters. Đây là một ví dụ với `user.name` được triển khai bằng cách sử dụng chúng:

```js
class User {

  constructor(name) {
    // invokes the setter
    this.name = name;
  }

  get name() {
    return this._name;
  }

  set name(value) {
    if (value.length < 4) {
      alert("Name is too short.");
      return;
    }
    this._name = value;
  }

}

let user = new User("John");
alert(user.name); // John

user = new User(""); // Name too short.
```

Trong nội bộ, getters và setters cũng được tạo ra trên `User` prototype, như thế này:

```js
Object.defineProperties(User.prototype, {
  name: {
    get() {
      return this._name
    },
    set(name) {
      // ...
    }
  }
});
```

### Only methods

Không giống như các object literals, không có phép gán `property:value` bên trong `class`. Chỉ có phương thức và getters/setters. Có một số công việc đang diễn ra trong đặc tả để nâng cao giới hạn đó, nhưng nó vẫn chưa có.

Nếu chúng ta thực sự cần đặt một giá trị phi hàm (non-function value) vào nguyên mẫu (prototype), thì chúng ta có thể thay đổi `prototype` bằng tay, như thế này:

```js
class User { }

User.prototype.test = 5;

alert( new User().test ); // 5
```

Vì vậy, về mặt kỹ thuật là có thể, nhưng chúng ta nên biết tại sao chúng ta làm việc đó. Các thuộc tính như vậy sẽ được chia sẻ giữa tất cả các đối tượng của class.

Một thay thế "trong lớp (in-class)" là sử dụng một getter:

```js
class User {
  get test() {
    return 5;
  }
}

alert( new User().test ); // 5
```

Từ mã bên ngoài, việc sử dụng là như nhau. Nhưng biến thể getter chậm hơn một chút.

## Class Expression

Cũng giống như các hàm, các classes có thể được định nghĩa bên trong một biểu thức khác, được truyền xung quanh, trả về, v.v.

Đây là một class-returning function ("class factory"):

```js
function makeClass(phrase) {
  // declare a class and return it
  return class {
    sayHi() {
      alert(phrase);
    };
  };
}

let User = makeClass("Hello");

new User().sayHi(); // Hello
```

Điều đó khá bình thường nếu chúng ta nhớ lại rằng `class` chỉ là một dạng đặc biệt của định nghĩa function-with-prototype.

Và, giống như Named Function Expressions, các classes như vậy cũng có thể có một tên, chỉ hiển thị bên trong class đó:

```js
// "Named Class Expression" (alas, no such term, but that's what's going on)
let User = class MyClass {
  sayHi() {
    alert(MyClass); // MyClass is visible only inside the class
  }
};

new User().sayHi(); // works, shows MyClass definition

alert(MyClass); // error, MyClass not visible outside of the class
```

## Static methods

Chúng ta cũng có thể gán các phương thức cho class function, chứ không phải cho `"prototype"`. Các phương thức như vậy được gọi là *static*.

An example:

```js
class User {
  static staticMethod() {
    alert(this === User);
  }
}

User.staticMethod(); // true
```

Điều đó thực sự giống như việc gán nó như một thuộc tính hàm:

```js
function User() { }

User.staticMethod = function() {
  alert(this === User);
};
```

Giá trị của `this` bên trong `User.staticMethod()` là chính class constructor `User` (quy tắc "đối tượng trước dấu chấm (object before dot)").

Thông thường, các phương thức tĩnh (static methods) được sử dụng để thực hiện các hàm thuộc về class, nhưng không thuộc về bất kỳ đối tượng cụ thể nào của nó.

Chẳng hạn, chúng ta có các đối tượng `Article` và cần một hàm để so sánh chúng. Sự lựa chọn tự nhiên sẽ là `Article.compare`, như thế này:

```js
class Article {
  constructor(title, date) {
    this.title = title;
    this.date = date;
  }

  static compare(articleA, articleB) {
    return articleA.date - articleB.date;
  }
}

// usage
let articles = [
  new Article("Mind", new Date(2016, 1, 1)),
  new Article("Body", new Date(2016, 0, 1)),
  new Article("JavaScript", new Date(2016, 11, 1))
];

articles.sort(Article.compare);

alert( articles[0].title ); // Body
```

Ở đây `Article.compare` đứng "trên" các articles, như một phương tiện để so sánh chúng. Đây không phải là một phương thức của một article, mà là của cả class.

Một ví dụ khác là phương pháp "factory". Hãy tưởng tượng, chúng ta cần một vài cách để tạo một article:

1. Tạo bởi các tham số đã cho (`title`, `date` etc).
2. Tạo một empty article với today's date.
3. ...

Cách đầu tiên có thể được thực hiện bởi constructor. Và đối với cách thứ hai, chúng ta có thể tạo một static method của class.

Like `Article.createTodays()` here:

```js
class Article {
  constructor(title, date) {
    this.title = title;
    this.date = date;
  }

  static createTodays() {
    // remember, this = Article
    return new this("Today's digest", new Date());
  }
}

let article = Article.createTodays();

alert( article.title ); // Todays digest
```

Bây giờ mỗi khi chúng ta cần tạo một today's digest, chúng ta có thể gọi `Article.createTodays()`. Một lần nữa, đó không phải là một phương thức của một article, mà là một phương thức của cả class.

Các phương thức tĩnh cũng được sử dụng trong các lớp liên quan đến cơ sở dữ liệu để tìm kiếm/lưu/xóa các entries khỏi cơ sở dữ liệu, như sau:

```js
// assuming Article is a special class for managing articles
// static method to remove the article:
Article.remove({id: 12345});
```

## Tóm lược

Cú pháp lớp cơ bản trông như thế này:

```js
class MyClass {
  constructor(...) {
    // ...
  }
  method1(...) {}
  method2(...) {}
  get something(...) {}
  set something(...) {}
  static staticMethod(..) {}
  // ...
}
```

The value của `MyClass` là một hàm được cung cấp như là `constructor`. Nếu không có `constructor`, thì một empty function.

Trong mọi trường hợp, các phương thức được liệt kê trong class declaration trở thành thành viên của `prototype`, ngoại trừ các static methods được ghi vào chính hàm và có thể gọi là `MyClass.staticMethod()`. Các phương thức tĩnh được sử dụng khi chúng ta cần một hàm ràng buộc với một lớp, nhưng không phải với bất kỳ đối tượng nào của lớp đó.

Trong chương tiếp theo, chúng ta sẽ tìm hiểu thêm về các classes, bao gồm cả kế thừa.
