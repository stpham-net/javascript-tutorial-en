
# Function object, NFE

Như chúng ta đã biết, các hàm trong JavaScript là các giá trị.

Mỗi giá trị trong JavaScript có một kiểu. Kiểu nào là một function?

Trong JavaScript, các hàm là các objects.

Một cách tốt để tưởng tượng các functions là "action objects" có thể gọi được. Chúng ta không chỉ có thể gọi chúng mà còn coi chúng là các đối tượng: thêm/xóa thuộc tính, chuyển qua tham chiếu, v.v.

## Thuộc tính "name"

Các function objects chứa một vài thuộc tính có thể sử dụng.

Chẳng hạn, tên của hàm có thể truy cập được dưới dạng thuộc tính "name":

```js
function sayHi() {
  alert("Hi");
}

alert(sayHi.name); // sayHi
```

Điều gì buồn cười hơn, logic gán tên rất thông minh. Nó cũng gán tên chính xác cho các hàm được sử dụng trong các phép gán:

```js
let sayHi = function() {
  alert("Hi");
}

alert(sayHi.name); // sayHi (works!)
```

Nó cũng hoạt động nếu việc gán được thực hiện thông qua một giá trị mặc định:

```js
function f(sayHi = function() {}) {
  alert(sayHi.name); // sayHi (works!)
}

f();
```

Trong đặc tả, tính năng này được gọi là "tên theo ngữ cảnh (contextual name)". Nếu hàm không cung cấp một tên, thì trong một phép gán, nó được tìm ra từ ngữ cảnh.

Các phương thức đối tượng cũng có tên:

```js
let user = {

  sayHi() {
    // ...
  },

  sayBye: function() {
    // ...
  }

}

alert(user.sayHi.name); // sayHi
alert(user.sayBye.name); // sayBye
```

Không có phép thuật nào cả. Có những trường hợp khi không có cách nào để tìm ra tên đúng. Trong trường hợp đó, thuộc tính tên trống, như ở đây:

```js
// function created inside array
let arr = [function() {}];

alert( arr[0].name ); // <empty string>
// the engine has no way to set up the right name, so there is none
```

Trong thực tế, tuy nhiên, hầu hết các functions có một tên.

## Thuộc tính "length"

Có một built-in property "length" khác trả về số lượng tham số của hàm, ví dụ:

```js
function f1(a) {}
function f2(a, b) {}
function many(a, b, ...more) {}

alert(f1.length); // 1
alert(f2.length); // 2
alert(many.length); // 2
```

Ở đây chúng ta có thể thấy rằng các rest parameters không được tính.

Thuộc tính `length` đôi khi được sử dụng để hướng nội trong các functions hoạt động trên các functions khác.

Chẳng hạn, trong đoạn mã bên dưới hàm `ask` chấp nhận một `question` để hỏi và một số tùy ý các hàm `handler` để gọi.

Khi một người dùng cung cấp câu trả lời của họ, hàm sẽ gọi các handlers. Chúng ta có thể pass qua hai kiểu của các handlers:

- Một zero-argument function, chỉ được gọi khi người dùng đưa ra positive answer.
- Một function với các arguments, được gọi trong cả hai trường hợp và trả về một câu trả lời.

Ý tưởng là chúng ta có một no-arguments handler syntax đơn giản cho các trường hợp positive (biến thể thường xuyên nhất), nhưng cũng có thể cung cấp các universal handlers.

Để gọi `handlers` đúng cách, chúng ta kiểm tra thuộc tính `length`:

```js
function ask(question, ...handlers) {
  let isYes = confirm(question);

  for(let handler of handlers) {
    if (handler.length == 0) {
      if (isYes) handler();
    } else {
      handler(isYes);
    }
  }

}

// for positive answer, both handlers are called
// for negative answer, only the second one
ask("Question?", () => alert('You said yes'), result => alert(result));
```

Đây là trường hợp cụ thể của cái gọi là [đa hình] (https://en.wikipedia.org/wiki/Polymorphism_(computer_science)) -- xử lý các đối số khác nhau tùy thuộc vào kiểu của chúng hoặc, trong trường hợp của chúng ta tùy thuộc vào độ dài. Ý tưởng này có sử dụng trong các thư viện JavaScript.

## Thuộc tính tùy chỉnh (Custom properties)

Chúng ta cũng có thể thêm các thuộc tính của riêng chúng ta.

Ở đây chúng ta thêm thuộc tính `counter` để theo dõi tổng số cuộc gọi:

```js
function sayHi() {
  alert("Hi");

  // let's count how many times we run
  sayHi.counter++;
}
sayHi.counter = 0; // initial value

sayHi(); // Hi
sayHi(); // Hi

alert( `Called ${sayHi.counter} times` ); // Called 2 times
```

<br>

> ---

**📌 Một thuộc tính không phải là một biến**

Một thuộc tính được gán cho một hàm như `sayHi.counter = 0` *không* định nghĩa một local variable `counter` bên trong nó. Nói cách khác, một thuộc tính `counter` và một biến `let counter` là hai thứ không liên quan.

Chúng ta có thể coi một hàm là một đối tượng, lưu trữ các thuộc tính trong nó, nhưng điều đó không ảnh hưởng gì đến việc thực thi của nó. Các biến không bao giờ sử dụng các thuộc tính hàm và ngược lại. Đây chỉ là những từ song song.

> ---

<br>

Các function properties có thể thay thế closures đôi khi. Chẳng hạn, chúng ta có thể viết lại ví dụ counter function từ chương **closure** để sử dụng một function property:

```js
function makeCounter() {
  // instead of:
  // let count = 0

  function counter() {
    return counter.count++;
  };

  counter.count = 0;

  return counter;
}

let counter = makeCounter();
alert( counter() ); // 0
alert( counter() ); // 1
```

The `count` hiện được lưu trữ trực tiếp trong hàm, không phải trong outer Lexical Environment của nó.

Nó tốt hơn hay tồi tệ hơn việc sử dụng một closure?

Sự khác biệt chính là nếu giá trị của `count` sống trong một outer variable, thì external code không thể truy cập được. Chỉ các hàm lồng nhau có thể sửa đổi nó. Và nếu nó được ràng buộc với một function, thì điều này là có thể:

```js
function makeCounter() {

  function counter() {
    return counter.count++;
  };

  counter.count = 0;

  return counter;
}

let counter = makeCounter();

counter.count = 10;
alert( counter() ); // 10
```

Vì vậy, sự lựa chọn thực hiện phụ thuộc vào mục tiêu của chúng ta.

## Biểu thức hàm được đặt tên (Named Function Expression)

Named Function Expression, hoặc NFE, là một thuật ngữ cho Function Expressions có tên.

Chẳng hạn, hãy lấy một Function Expression thường:

```js
let sayHi = function(who) {
  alert(`Hello, ${who}`);
};
```

Và thêm một tên cho nó:

```js
let sayHi = function func(who) {
  alert(`Hello, ${who}`);
};
```

Chúng ta đã đạt được bất cứ điều gì ở đây? Mục đích của tên `"func"` bổ sung đó là gì?

Trước tiên hãy lưu ý rằng chúng ta vẫn có Function Expression. Việc thêm tên `"func"` sau `function` không làm cho nó trở thành một Function Declaration, bởi vì nó vẫn được tạo như là một phần của biểu thức gán.

Thêm một cái tên như vậy cũng không phá vỡ bất cứ điều gì.

Hàm vẫn có sẵn dưới dạng `sayHi()`:

```js
let sayHi = function func(who) {
  alert(`Hello, ${who}`);
};

sayHi("John"); // Hello, John
```

Có hai điều đặc biệt về cái tên `func`:

1. Nó cho phép các function tham chiếu chính nó trong nội bộ.
2. Nó không thể nhìn thấy bên ngoài function.

Chẳng hạn, hàm `sayHi` bên dưới gọi lại chính nó lần nữa với `"Guest"` nếu không có `who` được cung cấp:

```js
let sayHi = function func(who) {
  if (who) {
    alert(`Hello, ${who}`);
  } else {
    func("Guest"); // use func to re-call itself
  }
};

sayHi(); // Hello, Guest

// But this won't work:
func(); // Error, func is not defined (not visible outside of the function)
```

Tại sao chúng ta sử dụng `func`? Có lẽ chỉ cần sử dụng `sayHi` cho cuộc gọi lồng nhau?


Trên thực tế, trong hầu hết các trường hợp, chúng ta có thể:

```js
let sayHi = function(who) {
  if (who) {
    alert(`Hello, ${who}`);
  } else {
    sayHi("Guest");
  }
};
```

Vấn đề với mã đó là giá trị của `sayHi` có thể thay đổi. Hàm có thể chuyển sang một biến khác và mã sẽ bắt đầu báo lỗi:

```js
let sayHi = function(who) {
  if (who) {
    alert(`Hello, ${who}`);
  } else {
    sayHi("Guest"); // Error: sayHi is not a function
  }
};

let welcome = sayHi;
sayHi = null;

welcome(); // Error, the nested sayHi call doesn't work any more!
```

Điều đó xảy ra bởi vì hàm lấy `sayHi` từ outer lexical environment của nó. Không có local `sayHi`, vì vậy biến ngoài được sử dụng. Và tại thời điểm của cuộc gọi thì outer `sayHi` là `null`.

Tên tùy chọn mà chúng ta có thể đặt vào Function Expression có nghĩa là để giải quyết chính xác các loại vấn đề này.

Hãy sử dụng nó để sửa mã của chúng ta:

```js
let sayHi = function func(who) {
  if (who) {
    alert(`Hello, ${who}`);
  } else {
    func("Guest"); // Now all fine
  }
};

let welcome = sayHi;
sayHi = null;

welcome(); // Hello, Guest (nested call works)
```

Bây giờ nó hoạt động, vì tên `"func"` là function-local. Nó không được lấy từ bên ngoài (và không nhìn thấy ở đó). Các đặc tả đảm bảo rằng nó sẽ luôn luôn tham chiếu các function hiện tại.

Mã bên ngoài vẫn có biến `sayHi` hoặc `welcome`. Và `func` là một "internal function name", làm thế nào hàm có thể tự gọi bên trong.

<br>

> ---

**📌 Không có điều nào như vậy cho Function Declaration**

Tính năng "tên nội bộ" được mô tả ở đây chỉ khả dụng cho Function Expressions, không có cho Function Declarations. Đối với các Function Declarations, không có cú pháp có khả năng để thêm một tên "nội bộ" nữa.

Đôi khi, khi chúng ta cần một tên nội bộ đáng tin cậy, đó là lý do để viết lại Function Declaration thành Named Function Expression.

> ---

<br>

## Tóm lược

Functions là đối tượng.

Ở đây chúng ta bao gồm các thuộc tính của chúng:

- `name` -- tên hàm. Không chỉ tồn tại khi được đưa ra trong định nghĩa hàm, mà còn cho các phép gán và object properties.
- `length` -- số lượng đối số trong định nghĩa hàm. Rest parameters không được tính.

Nếu hàm được khai báo là Function Expression (không phải trong luồng mã chính) và nó mang tên, thì nó được gọi là Named Function Expression. Tên có thể được sử dụng bên trong để tham chiếu chính nó, cho các cuộc gọi đệ quy hoặc như vậy.

Ngoài ra, các functions có thể mang thuộc tính bổ sung. Nhiều thư viện JavaScript nổi tiếng sử dụng tốt tính năng này.

Họ tạo ra một "main" function và gắn nhiều "helper" functions khác vào nó. Chẳng hạn, thư viện [jquery](https://jquery.com) tạo ra một hàm có tên `$`. Thư viện [lodash](https://lodash.com) tạo ra một hàm `_`. Và sau đó thêm `_.clone`, `_.keyBy` và các thuộc tính khác vào (xem [docs](https://lodash.com/docs) khi bạn muốn tìm hiểu thêm về chúng). Trên thực tế, họ làm điều đó để giảm bớt ô nhiễm không gian toàn cầu (global space), do đó một thư viện chỉ cung cấp một global variable. Điều đó làm giảm khả năng đặt tên xung đột (naming conflicts).

Vì vậy, một chức năng có thể tự thực hiện một công việc hữu ích và cũng mang một loạt các chức năng khác trong các thuộc tính.
