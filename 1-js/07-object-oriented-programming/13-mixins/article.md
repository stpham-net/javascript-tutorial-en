# Mixins

Trong JavaScript, chúng ta chỉ có thể kế thừa từ một đối tượng. Chỉ có thể có một `[[Prototype]]` cho một đối tượng. Và một lớp chỉ có thể extend một lớp khác.

Nhưng đôi khi điều đó cảm thấy hạn chế. Chẳng hạn, tôi có một lớp `StreetSweeper` và một lớp `Bicycle`, và muốn tạo ra một `StreetSweepingBicycle`.

Hoặc, nói về lập trình, chúng ta có một lớp `Renderer` thực hiện việc tạo templating và một lớp `EventEmitter` thực hiện xử lý sự kiện và muốn hợp nhất các chức năng này với một lớp `Page`, để tạo một trang có thể sử dụng các templates và phát ra các sự kiện (emit events).

Có một khái niệm có thể giúp đỡ ở đây, được gọi là "mixins".

Như được định nghĩa trong Wikipedia, [mixin](https://en.wikipedia.org/wiki/Mixin) là một lớp chứa các phương thức được sử dụng bởi các lớp khác mà không phải là lớp cha của các lớp khác.

Nói cách khác, một *mixin* cung cấp các phương thức thực hiện một hành vi nhất định, nhưng chúng ta không sử dụng nó một mình, chúng ta sử dụng nó để thêm hành vi vào các lớp khác.

## Một ví dụ mixin

Cách đơn giản nhất để tạo mixin trong JavaScript là tạo một đối tượng với các phương thức hữu ích, để chúng ta có thể dễ dàng hợp nhất chúng thành một nguyên mẫu của bất kỳ lớp nào.

Ví dụ ở đây, mixin `sayHiMixin` được sử dụng để thêm một số "speech" cho `User`:

```js
// mixin
let sayHiMixin = {
  sayHi() {
    alert(`Hello ${this.name}`);
  },
  sayBye() {
    alert(`Bye ${this.name}`);
  }
};

// usage:
class User {
  constructor(name) {
    this.name = name;
  }
}

// copy the methods
Object.assign(User.prototype, sayHiMixin);

// now User can say hi
new User("Dude").sayHi(); // Hello Dude!
```

Không có sự kế thừa, nhưng một phương pháp sao chép đơn giản. Vì vậy, `User` có thể mở rộng một số lớp khác và cũng bao gồm mixin để "mix-in" các phương thức bổ sung, như sau:

```js
class User extends Person {
  // ...
}

Object.assign(User.prototype, sayHiMixin);
```

Mixins có thể sử dụng sự kế thừa bên trong bản thân chúng.

Chẳng hạn, ở đây `sayHiMixin` kế thừa từ `sayMixin`:

```js
let sayMixin = {
  say(phrase) {
    alert(phrase);
  }
};

let sayHiMixin = {
  __proto__: sayMixin, // (or we could use Object.create to set the prototype here)

  sayHi() {
    // call parent method
    super.say(`Hello ${this.name}`);
  },
  sayBye() {
    super.say(`Bye ${this.name}`);
  }
};

class User {
  constructor(name) {
    this.name = name;
  }
}

// copy the methods
Object.assign(User.prototype, sayHiMixin);

// now User can say hi
new User("Dude").sayHi(); // Hello Dude!
```

Xin lưu ý rằng lệnh gọi phương thức cha `super.say()` từ `sayHiMixin` tìm phương thức trong nguyên mẫu của mixin đó, không phải class.

![](mixin-inheritance.png)

Đó là bởi vì các phương thức từ `sayHiMixin` có `[[HomeObject]]` được đặt cho nó. Vì vậy, `super` thực sự có nghĩa là `sayHiMixin.__proto__`, không phải `User.__proto__`.

## EventMixin

Bây giờ hãy tạo một mixin cho cuộc sống thực.

Các tính năng quan trọng của nhiều đối tượng là làm việc với các sự kiện.

Đó là: một đối tượng nên có một phương thức để "tạo ra một sự kiện" khi một điều quan trọng xảy ra với nó, và các đối tượng khác sẽ có thể "lắng nghe" các sự kiện đó.

Một sự kiện phải có tên và, tùy ý, gói một số dữ liệu bổ sung.

Chẳng hạn, một đối tượng `user` có thể tạo ra một sự kiện `"login"` khi khách truy cập đăng nhập. Và một đối tượng khác `calendar` có thể muốn nhận các sự kiện như vậy để tải lịch cho người đã đăng nhập.

Hoặc, một `menu` có thể tạo ra sự kiện `"select"` khi một mục menu được chọn và các đối tượng khác có thể muốn lấy thông tin đó và phản ứng với sự kiện đó.

Sự kiện là một cách để "chia sẻ thông tin" với bất kỳ ai muốn nó. Chúng có thể hữu ích trong bất kỳ class nào, vì vậy hãy tạo một mixin cho chúng:

```js
let eventMixin = {
  /**
   * Subscribe to event, usage:
   *  menu.on('select', function(item) { ... }
  */
  on(eventName, handler) {
    if (!this._eventHandlers) this._eventHandlers = {};
    if (!this._eventHandlers[eventName]) {
      this._eventHandlers[eventName] = [];
    }
    this._eventHandlers[eventName].push(handler);
  },

  /**
   * Cancel the subscription, usage:
   *  menu.off('select', handler)
   */
  off(eventName, handler) {
    let handlers = this._eventHandlers && this._eventHandlers[eventName];
    if (!handlers) return;
    for (let i = 0; i < handlers.length; i++) {
      if (handlers[i] === handler) {
        handlers.splice(i--, 1);
      }
    }
  },

  /**
   * Generate the event and attach the data to it
   *  this.trigger('select', data1, data2);
   */
  trigger(eventName, ...args) {
    if (!this._eventHandlers || !this._eventHandlers[eventName]) {
      return; // no handlers for that event name
    }

    // call the handlers
    this._eventHandlers[eventName].forEach(handler => handler.apply(this, args));
  }
};
```

Có 3 phương thức ở đây:

1. `.on(eventName, handler)` -- gán hàm `handler` để chạy khi sự kiện có tên đó xảy ra. Các handlers được lưu trữ trong thuộc tính `_eventHandlers`.
2. `.off(eventName, handler)` -- xóa hàm khỏi danh sách handlers.
3. `.trigger(eventName, ...args)` -- tạo sự kiện: tất cả các handlers được gán đều được gọi và `args` được truyền dưới dạng đối số cho chúng.

Sử dụng:

```js
// Make a class
class Menu {
  choose(value) {
    this.trigger("select", value);
  }
}
// Add the mixin
Object.assign(Menu.prototype, eventMixin);

let menu = new Menu();

// call the handler on selection:
menu.on("select", value => alert(`Value selected: ${value}`));

// triggers the event => shows Value selected: 123
menu.choose("123"); // value selected
```

Bây giờ nếu chúng ta có mã thích phản ứng với lựa chọn của người dùng, chúng ta có thể liên kết nó với `menu.on(...)`.

Và `eventMixin` có thể thêm hành vi đó vào bao nhiêu classes mà chúng ta muốn, mà không can thiệp vào chuỗi thừa kế (inheritance chain).

## Tóm lược

*Mixin* -- là một thuật ngữ lập trình hướng đối tượng chung: một lớp có chứa các phương thức cho các lớp khác.

Một số ngôn ngữ khác như ví dụ python cho phép tạo mixin bằng cách sử dụng nhiều kế thừa. JavaScript không hỗ trợ nhiều kế thừa, nhưng mixin có thể được thực hiện bằng cách sao chép chúng vào nguyên mẫu.

Chúng ta có thể sử dụng mixin như một cách để tăng cường một lớp bằng nhiều hành vi, như xử lý sự kiện như chúng ta đã thấy ở trên.

Mixins có thể trở thành một điểm xung đột nếu đôi khi chúng ghi đè lên các phương thức lớp gốc. Vì vậy, nhìn chung người ta nên suy nghĩ tốt về việc đặt tên cho một mixin, để giảm thiểu khả năng đó.
