# Arrow functions được xem lại

Hãy xem lại các arrow functions.

Các arrow functions không chỉ là "tốc ký" để viết các small stuff.

JavaScript chứa đầy các tình huống mà chúng ta cần viết một hàm nhỏ, được thực thi ở một nơi khác.

Ví dụ:

- `arr.forEach(func)` -- `func` được thực thi bởi `forEach` cho mọi array item.
- `setTimeout(func)` -- `func` được thực thi bởi built-in scheduler.
- ...there are more.

Đó là tinh thần của JavaScript để tạo ra một hàm và chuyển nó đi đâu đó.

Và trong các functions như vậy, chúng ta thường không muốn rời khỏi bối cảnh hiện tại.

## Arrow functions không có "this"

Như chúng ta nhớ từ chương **object-methods**, các arrow functions không có `this`. Nếu `this` được truy cập, nó được lấy từ bên ngoài.

Chẳng hạn, chúng ta có thể sử dụng nó để lặp lại (iterate) bên trong một phương thức đối tượng:

```js
let group = {
  title: "Our Group",
  students: ["John", "Pete", "Alice"],

  showList() {
    this.students.forEach(
      student => alert(this.title + ': ' + student)
    );
  }
};

group.showList();
```

Ở đây trong `forEach`, the arrow function được sử dụng, vì vậy `this.title` trong nó hoàn toàn giống như trong phương thức bên ngoài `showList`. Đó là: `group.title`.

Nếu chúng ta sử dụng function "thông thường", sẽ có lỗi:

```js
let group = {
  title: "Our Group",
  students: ["John", "Pete", "Alice"],

  showList() {
    this.students.forEach(function(student) {
      // Error: Cannot read property 'title' of undefined
      alert(this.title + ': ' + student)
    });
  }
};

group.showList();
```

Lỗi xảy ra do `forEach` chạy các hàm với `this=undefined` theo mặc định, do đó, nỗ lực truy cập `undefined.title` được thực hiện.

Điều đó không ảnh hưởng đến các arrow functions, vì chúng không có `this`.

<br>

> ---

**📌 Các arrow functions không thể chạy với `new`**

Không có `this` tự nhiên có nghĩa là một giới hạn khác: các arrow functions không thể được sử dụng như các constructors. Chúng không thể được gọi với `new`.

> ---

<br>
<br>

> ---

**📌 Arrow functions VS bind**

Có một sự khác biệt tinh tế giữa một arrow function `=>` và một hàm thông thường được gọi với `.bind(this)`:

- `.bind(this)` tạo ra "phiên bản ràng buộc" của hàm.
- The arrow `=>` không tạo ra bất kỳ ràng buộc nào. Hàm đơn giản là không có `this`. Việc tra cứu `this` được thực hiện chính xác giống như tìm kiếm biến thông thường: trong outer lexical environment.

> ---

<br>

## Arrows have no "arguments"

Các arrow functions cũng không có biến `arguments`.

Điều đó thật tuyệt vời cho các decorators, khi chúng ta cần chuyển tiếp một cuộc gọi với các đối số `this` và `arguments`.

Chẳng hạn, `defer(f, ms)` nhận một hàm và trả về một wrapper xung quanh nó làm trì hoãn cuộc gọi bằng `ms` mili giây:

```js
function defer(f, ms) {
  return function() {
    setTimeout(() => f.apply(this, arguments), ms)
  };
}

function sayHi(who) {
  alert('Hello, ' + who);
}

let sayHiDeferred = defer(sayHi, 2000);
sayHiDeferred("John"); // Hello, John after 2 seconds
```

Điều tương tự mà không dùng arrow function sẽ giống như:

```js
function defer(f, ms) {
  return function(...args) {
    let ctx = this;
    setTimeout(function() {
      return f.apply(ctx, args);
    }, ms);
  };
}
```

Ở đây chúng ta phải tạo thêm các biến `args` và `ctx` để hàm bên trong `setTimeout` có thể lấy chúng.

## Tóm lược

Các hàm mũi tên (arrow functions):

- Do not have `this`.
- Do not have `arguments`.
- Không thể được gọi với `new`.
- (Chúng cũng không có `super`, nhưng chúng ta chưa nghiên cứu về nó. Sẽ có trong chương **class-inheritance**).

Đó là bởi vì chúng có nghĩa là cho các đoạn mã ngắn không có "bối cảnh" riêng, mà hoạt động trong mã hiện tại. Và chúng thực sự tỏa sáng trong trường hợp sử dụng đó.
