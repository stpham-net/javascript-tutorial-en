# Function binding

Khi sử dụng `setTimeout` với các phương thức đối tượng hoặc chuyển các phương thức đối tượng đi cùng, có một vấn đề đã biết: "mất `this`".

Đột nhiên, `this` chỉ dừng hoạt động. Tình huống này là điển hình cho các nhà phát triển mới tập sự, nhưng cũng xảy ra với những người có kinh nghiệm.

## Losing "this"

Chúng ta đã biết rằng trong JavaScript, thật dễ dàng để mất `this`. Khi một phương thức được truyền đi đâu đó tách biệt khỏi đối tượng -- `this` bị mất.

Đây là cách nó có thể xảy ra với `setTimeout`:

```js
let user = {
  firstName: "John",
  sayHi() {
    alert(`Hello, ${this.firstName}!`);
  }
};

setTimeout(user.sayHi, 1000); // Hello, undefined!
```

Như chúng ta có thể thấy, đầu ra hiển thị không phải là "John" như `this.firstName`, mà là `undefined`!

Đó là bởi vì `setTimeout` có function `user.sayHi`, tách biệt với đối tượng. Dòng cuối cùng có thể được viết lại thành:

```js
let f = user.sayHi;
setTimeout(f, 1000); // lost user context
```

Phương thức `setTimeout` trong trình duyệt hơi đặc biệt: nó đặt `this=window` cho lệnh gọi hàm (đối với Node.JS, `this` trở thành đối tượng hẹn giờ, nhưng không thực sự quan trọng ở đây). Vì vậy, đối với `this.firstName`, nó cố gắng lấy `window.firstName`, không tồn tại. Trong các trường hợp tương tự khác như chúng ta sẽ thấy, thông thường `this` sẽ trở thành `undefined`.

Nhiệm vụ này khá điển hình - chúng ta muốn truyền một phương thức đối tượng ở một nơi khác (ở đây -- cho bộ lập lịch) nơi nó sẽ được gọi. Làm thế nào để đảm bảo rằng nó sẽ được gọi trong đúng ngữ cảnh?

## Solution 1: a wrapper

Giải pháp đơn giản nhất là sử dụng wrapping function:

```js
let user = {
  firstName: "John",
  sayHi() {
    alert(`Hello, ${this.firstName}!`);
  }
};

setTimeout(function() {
  user.sayHi(); // Hello, John!
}, 1000);
```

Bây giờ nó hoạt động, bởi vì nó nhận được `user` từ outer lexical environment, và sau đó gọi phương thức bình thường.

Giống nhau, nhưng ngắn hơn:

```js
setTimeout(() => user.sayHi(), 1000); // Hello, John!
```

Có vẻ tốt, nhưng một lỗ hổng nhỏ xuất hiện trong cấu trúc mã của chúng ta.

Điều gì xảy ra nếu trước khi `setTimeout` kích hoạt (có độ trễ một giây!) `user` thay đổi giá trị? Sau đó, đột nhiên, nó sẽ gọi sai đối tượng!

```js
let user = {
  firstName: "John",
  sayHi() {
    alert(`Hello, ${this.firstName}!`);
  }
};

setTimeout(() => user.sayHi(), 1000);

// ...within 1 second
user = { sayHi() { alert("Another user in setTimeout!"); } };

// Another user in setTimeout?!?
```

Giải pháp tiếp theo đảm bảo rằng điều đó sẽ không xảy ra.

## Solution 2: bind

Các hàm cung cấp một built-in method [bind](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/bind) cho phép fix `this`.

Cú pháp cơ bản là:

```js
// more complex syntax will be little later
let boundFunc = func.bind(context);
```

Kết quả của `func.bind(context)` là một special function-like "exotic (ngoại lai) object", có thể gọi là hàm và chuyển rõ ràng cuộc gọi đến `func` đặt `this=context`.

Nói cách khác, việc gọi `boundFunc` giống như `func` với fixed `this`.

Chẳng hạn, ở đây `funcUser` chuyển một cuộc gọi đến `func` với `this=user`:

```js
let user = {
  firstName: "John"
};

function func() {
  alert(this.firstName);
}

let funcUser = func.bind(user);
funcUser(); // John  
```

Ở đây `func.bind(user)` là một "biến thể bị ràng buộc" của `func`, với fixed `this=user`.

Tất cả các đối số được truyền cho `func` "as is", ví dụ:

```js
let user = {
  firstName: "John"
};

function func(phrase) {
  alert(phrase + ', ' + this.firstName);
}

// bind this to user
let funcUser = func.bind(user);

funcUser("Hello"); // Hello, John (argument "Hello" is passed, and this=user)
```

Bây giờ hãy thử với một phương thức đối tượng:

```js
let user = {
  firstName: "John",
  sayHi() {
    alert(`Hello, ${this.firstName}!`);
  }
};

let sayHi = user.sayHi.bind(user); // (*)

sayHi(); // Hello, John!

setTimeout(sayHi, 1000); // Hello, John!
```

Trong dòng `(*)` chúng ta lấy phương thức `user.sayHi` và liên kết nó với `user`. `SayHi` là một hàm "bị ràng buộc", có thể được gọi một mình hoặc được chuyển đến `setTimeout` -- không thành vấn đề, bối cảnh sẽ đúng.

Ở đây chúng ta có thể thấy rằng các đối số được truyền "nguyên trạng", chỉ `this` được sửa bởi `bind`:

```js
let user = {
  firstName: "John",
  say(phrase) {
    alert(`${phrase}, ${this.firstName}!`);
  }
};

let say = user.say.bind(user);

say("Hello"); // Hello, John ("Hello" argument is passed to say)
say("Bye"); // Bye, John ("Bye" is passed to say)
```

<br>

> ---

**📌 Phương thức tiện lợi: `bindAll`**

Nếu một đối tượng có nhiều phương thức và chúng ta dự định chủ động vượt qua nó, thì chúng ta có thể liên kết tất cả chúng trong một vòng lặp:

```js
for (let key in user) {
  if (typeof user[key] == 'function') {
    user[key] = user[key].bind(user);
  }
}
```

Thư viện JavaScript cũng cung cấp các hàm để liên kết hàng loạt thuận tiện, ví dụ [\_.bindAll(obj)](http://lodash.com/docs#bindAll) trong lodash.

> ---

<br>

## Tóm lược

Phương thức `func.bind(context, ...args)` trả về một "biến thể bị ràng buộc" của hàm `func` để sửa ngữ cảnh `this` và các đối số đầu tiên nếu được đưa ra.

Thông thường chúng ta áp dụng `bind` để sửa `this` trong một phương thức đối tượng, để chúng ta có thể chuyển nó đi đâu đó. Ví dụ, để `setTimeout`. Có nhiều lý do hơn để `bind` trong sự phát triển hiện đại, chúng ta sẽ gặp chúng sau.
