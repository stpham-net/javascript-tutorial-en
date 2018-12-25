# Decorators (người trang trí) and forwarding, call/apply

JavaScript mang lại sự linh hoạt đặc biệt khi xử lý các functions. Chúng có thể được truyền (passed) xung quanh, được sử dụng làm đối tượng và bây giờ chúng ta sẽ xem cách *forward* cuộc gọi giữa chúng và *decorate (trang trí)* chúng.

## Bộ nhớ đệm trong suốt (Transparent caching)

Giả sử chúng ta có một hàm `slow(x)` ngốn CPU, nhưng kết quả của nó là ổn định. Nói cách khác, với cùng một `x`, nó luôn trả về cùng một kết quả.

Nếu hàm được gọi thường xuyên, chúng ta có thể muốn lưu trữ (ghi nhớ) các kết quả cho các `x` khác nhau để tránh mất thêm thời gian cho việc tính toán lại.

Nhưng thay vì thêm chức năng đó vào `slow()`, chúng ta sẽ tạo một trình bao bọc (wrapper). Như chúng ta sẽ thấy, có rất nhiều lợi ích của việc đó.

Đây là mã và giải thích theo sau:

```js
function slow(x) {
  // there can be a heavy CPU-intensive job here
  alert(`Called with ${x}`);
  return x;
}

function cachingDecorator(func) {
  let cache = new Map();

  return function(x) {
    if (cache.has(x)) { // if the result is in the map
      return cache.get(x); // return it
    }

    let result = func(x); // otherwise call func

    cache.set(x, result); // and cache (remember) the result
    return result;
  };
}

slow = cachingDecorator(slow);

alert( slow(1) ); // slow(1) is cached
alert( "Again: " + slow(1) ); // the same

alert( slow(2) ); // slow(2) is cached
alert( "Again: " + slow(2) ); // the same as the previous line
```

Trong đoạn mã trên `cachingDecorator` là một *decorator*: một hàm đặc biệt có function khác và thay đổi hành vi của nó.

Ý tưởng là chúng ta có thể gọi `cachingDecorator` cho bất kỳ function nào và nó sẽ trả về caching wrapper. Điều đó thật tuyệt vời, bởi vì chúng ta có thể có nhiều functions có thể sử dụng một tính năng như vậy và tất cả những gì chúng ta cần làm là áp dụng `cachingDecorator` cho chúng.

Bằng cách tách bộ đệm khỏi mã function chính, chúng ta cũng giữ mã chính đơn giản hơn.

Bây giờ hãy đi vào chi tiết về cách thức hoạt động của nó.

Kết quả của `cachingDecorator(func)` là một "wrapper": `function(x)` để "wraps" cuộc gọi của `func(x)` thành caching logic:

![](decorator-makecaching-wrapper.png)

Như chúng ta có thể thấy, the wrapper trả về kết quả của `func(x)` "as is". Từ một outside code, the wrapped `slow` function vẫn làm như vậy. Nó chỉ có một khía cạnh bộ nhớ đệm được thêm vào hành vi của nó.

Tóm lại, có một số lợi ích của việc sử dụng một `cachingDecorator` riêng biệt thay vì thay đổi mã của chính `slow`:

- `cachingDecorator` có thể tái sử dụng. Chúng ta có thể áp dụng nó cho một function khác.
- The caching logic là riêng biệt, nó không làm tăng độ phức tạp của chính `slow` (nếu có).
- Chúng ta có thể kết hợp nhiều decorators nếu cần (các decorators khác sẽ theo sau).


## Sử dụng "func.call" cho ngữ cảnh

The caching decorator được đề cập ở trên không phù hợp để làm việc với các phương thức đối tượng.

Chẳng hạn, trong đoạn mã dưới đây `worker.slow()` dừng hoạt động sau khi trang trí (decoration):

```js
// we'll make worker.slow caching
let worker = {
  someMethod() {
    return 1;
  },

  slow(x) {
    // actually, there can be a scary CPU-heavy task here  
    alert("Called with " + x);
    return x * this.someMethod(); // (*)
  }
};

// same code as before
function cachingDecorator(func) {
  let cache = new Map();
  return function(x) {
    if (cache.has(x)) {
      return cache.get(x);
    }
    let result = func(x); // (**)
    cache.set(x, result);
    return result;
  };
}

alert( worker.slow(1) ); // the original method works

worker.slow = cachingDecorator(worker.slow); // now make it caching

alert( worker.slow(2) ); // Whoops! Error: Cannot read property 'someMethod' of undefined
```

Lỗi xảy ra trong dòng `(*)` cố truy cập `this.someMethod` và không thành công. Bạn có thấy tại sao không?

Lý do là wrapper gọi original function là `func(x)` trong dòng `(**)`. Và, khi được gọi như vậy, hàm sẽ nhận `this = undefined`.

Chúng ta sẽ quan sát một triệu chứng tương tự nếu chúng ta cố gắng chạy:

```js
let func = worker.slow;
func(2);
```

Vì vậy, wrapper chuyển cuộc gọi đến original method, nhưng không có ngữ cảnh `this`. Do đó có lỗi.

Hãy sửa nó.

Có một phương thức hàm tích hợp đặc biệt [func.call(context, ...args)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/call) cho phép gọi một hàm rõ ràng là có thiết lập `this`.

Cú pháp là:

```js
func.call(context, arg1, arg2, ...)
```

Nó chạy `func` cung cấp đối số đầu tiên là `this`, và tiếp theo là các đối số.

Nói một cách đơn giản, hai cuộc gọi này thực hiện gần như giống nhau:

```js
func(1, 2, 3);
func.call(obj, 1, 2, 3)
```

Cả hai đều gọi `func` với các đối số `1`, `2` và `3`. Sự khác biệt duy nhất là `func.call` cũng đặt `this` thành `obj`.

Ví dụ, trong đoạn mã dưới đây, chúng ta gọi `sayHi` trong ngữ cảnh của các đối tượng khác nhau: `sayHi.call(user)` chạy `sayHi` cung cấp `this=user`, và dòng tiếp theo đặt `this=admin`:

```js
function sayHi() {
  alert(this.name);
}

let user = { name: "John" };
let admin = { name: "Admin" };

// use call to pass different objects as "this"
sayHi.call( user ); // this = John
sayHi.call( admin ); // this = Admin
```

Và ở đây, chúng ta sử dụng `call` để gọi `say` với ngữ cảnh và cụm từ đã cho:

```js
function say(phrase) {
  alert(this.name + ': ' + phrase);
}

let user = { name: "John" };

// user becomes this, and "Hello" becomes the first argument
say.call( user, "Hello" ); // John: Hello
```

Trong trường hợp của chúng ta, chúng ta có thể sử dụng `call` trong wrapper để truyền ngữ cảnh cho hàm ban đầu:

```js
let worker = {
  someMethod() {
    return 1;
  },

  slow(x) {
    alert("Called with " + x);
    return x * this.someMethod(); // (*)
  }
};

function cachingDecorator(func) {
  let cache = new Map();
  return function(x) {
    if (cache.has(x)) {
      return cache.get(x);
    }
    let result = func.call(this, x); // "this" is passed correctly now
    cache.set(x, result);
    return result;
  };
}

worker.slow = cachingDecorator(worker.slow); // now make it caching

alert( worker.slow(2) ); // works
alert( worker.slow(2) ); // works, doesn't call the original (cached)
```

Bây giờ mọi thứ đều ổn.

Để làm cho tất cả rõ ràng, chúng ta hãy xem sâu hơn cách thức `this` được truyền qua:

1. Sau phần trang trí (decoration) `worker.slow` bây giờ là wrapper `function (x) { ... }`.
2. Vì vậy, khi `worker.slow(2)` được thực thi, wrapper lấy `2` làm đối số và `this=worker` (nó là đối tượng trước dấu chấm).
3. Bên trong wrapper, giả sử kết quả chưa được lưu vào bộ đệm, `func.call(this, x)` truyền `this` (`=worker`) hiện tại và đối số hiện tại (`=2`) cho original method.

## Going multi-argument with "func.apply"

Bây giờ chúng ta hãy làm cho `cachingDecorator` thậm chí còn phổ quát hơn. Cho đến bây giờ nó chỉ hoạt động với các hàm đối số đơn.

Bây giờ làm thế nào để lưu trữ phương thức `worker.slow` đa đối số?

```js
let worker = {
  slow(min, max) {
    return min + max; // scary CPU-hogger is assumed
  }
};

// should remember same-argument calls
worker.slow = cachingDecorator(worker.slow);
```

Chúng ta có hai nhiệm vụ để giải quyết ở đây.

Đầu tiên là cách sử dụng cả hai đối số `min` và `max` cho khóa trong `cache` map. Trước đây, đối với một đối số `x`, chúng ta chỉ có thể `cache.set(x, result)` để lưu kết quả và `cache.get(x)` để truy xuất nó. Nhưng bây giờ chúng ta cần nhớ kết quả cho một *kết hợp các đối số* `(min,max)`. `Map` bản địa chỉ lấy một giá trị duy nhất làm khóa.

Có nhiều giải pháp khả thi:

1. Triển khai một cấu trúc dữ liệu map-like (hoặc sử dụng của bên thứ ba) mới linh hoạt hơn và cho phép multi-keys.
2. Sử dụng các nested maps: `cache.set(min)` sẽ là một `Map` lưu trữ cặp `(max, result)`. Vì vậy, chúng ta có thể nhận được `result` là `cache.get(min).get(max)`.
3. Kết hợp hai giá trị thành một. Trong trường hợp cụ thể của chúng ta, chúng ta chỉ có thể sử dụng một chuỗi `"min,max"` làm khóa `Map`. Để linh hoạt, chúng ta có thể cho phép cung cấp *hashing function* cho decorator, biết cách tạo một giá trị từ nhiều giá trị.

Đối với nhiều ứng dụng thực tế, biến thể thứ 3 là đủ tốt, vì vậy chúng ta sẽ sử dụng nó.

Nhiệm vụ thứ hai cần giải quyết là làm thế nào để truyền nhiều đối số cho `func`. Hiện tại, the wrapper `function(x)` đang có một đối số duy nhất và `func.call(this, x)` truyền nó.

Ở đây, chúng ta có thể sử dụng một phương thức tích hợp khác [func.apply](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/apply).

Cú pháp là:

```js
func.apply(context, args)
```

Nó chạy `func` cài đặt `this=context` và sử dụng một array-like object `args` làm danh sách các đối số.


Chẳng hạn, hai cuộc gọi này gần như giống nhau:

```js
func(1, 2, 3);
func.apply(context, [1, 2, 3])
```

Cả hai đều chạy `func` cho nó các đối số `1,2,3`. Nhưng `apply` cũng đặt `this=context`.

Chẳng hạn, ở đây `say` được gọi với `this=user` và `messageData` như một danh sách các đối số:

```js
function say(time, phrase) {
  alert(`[${time}] ${this.name}: ${phrase}`);
}

let user = { name: "John" };

let messageData = ['10:00', 'Hello']; // become time and phrase

// user becomes this, messageData is passed as a list of arguments (time, phrase)
say.apply(user, messageData); // [10:00] John: Hello (this=user)
```

Sự khác biệt cú pháp duy nhất giữa `call` và `apply` là `call` mong đợi một danh sách các đối số, trong khi `apply` có một array-like object với chúng.

Chúng ta đã biết toán tử trải rộng (spread operator) `...` từ chương **rest-parameters-spread-operator** có thể truyền một mảng (hoặc bất kỳ iterable) dưới dạng danh sách các đối số. Vì vậy, nếu chúng ta sử dụng nó với `call`, chúng ta có thể đạt được gần giống như `apply`.

Hai cuộc gọi này gần như tương đương nhau:

```js
let args = [1, 2, 3];

func.call(context, ...args); // pass an array as list with spread operator
func.apply(context, args);   // is same as using apply
```

Nếu chúng ta xem xét kỹ hơn, có một sự khác biệt nhỏ giữa việc sử dụng `call` và `apply`.

- Toán tử trải rộng `...` cho phép chuyển *iterable* `args` làm danh sách cho `call`.
- `apply` chỉ chấp nhận *array-like* `args`.

Vì vậy, những cuộc gọi này bổ sung cho nhau. Nơi chúng ta mong đợi một iterable, `call` hoạt động, nơi chúng ta mong đợi một array-like, `apply` hoạt động.

Và nếu `args` vừa có thể iterable vừa array-like, giống như một mảng thực, thì về mặt kỹ thuật chúng ta có thể sử dụng bất kỳ trong số chúng, nhưng `apply` có thể sẽ nhanh hơn, bởi vì đó là một thao tác duy nhất. Hầu hết các JavaScript engines tối ưu hóa nội bộ tốt hơn một cặp `call + spread`.

Một trong những cách sử dụng quan trọng nhất của `apply` là chuyển cuộc gọi đến một function khác, như thế này:

```js
let wrapper = function() {
  return anotherFunction.apply(this, arguments);
};
```

Đó gọi là *chuyển tiếp cuộc gọi (call forwarding)*. The `wrapper` chuyển mọi thứ nó nhận được: bối cảnh `this` và các đối số cho `anotherFunction` và trả về kết quả của nó.

Khi một external code gọi như là `wrapper`, nó không thể phân biệt được cuộc gọi từ original function.

Bây giờ, Bây giờ hãy nướng tất cả thành `cachingDecorator` mạnh mẽ hơn:

```js
let worker = {
  slow(min, max) {
    alert(`Called with ${min},${max}`);
    return min + max;
  }
};

function cachingDecorator(func, hash) {
  let cache = new Map();
  return function() {
    let key = hash(arguments); // (*)
    if (cache.has(key)) {
      return cache.get(key);
    }

    let result = func.apply(this, arguments); // (**)

    cache.set(key, result);
    return result;
  };
}

function hash(args) {
  return args[0] + ',' + args[1];
}

worker.slow = cachingDecorator(worker.slow, hash);

alert( worker.slow(3, 5) ); // works
alert( "Again " + worker.slow(3, 5) ); // same (cached)
```

Bây giờ wrapper hoạt động với bất kỳ số lượng đối số.

Có hai thay đổi:

- Trong dòng `(*)`, nó gọi `hash` để tạo một khóa duy nhất từ `arguments`. Ở đây chúng ta sử dụng một hàm "nối" đơn giản để biến các đối số `(3, 5)` thành khóa `"3,5"`. Các trường hợp phức tạp hơn có thể yêu cầu các hàm băm khác.
- Sau đó `(**)` sử dụng `func.apply` để truyền cả bối cảnh và tất cả các đối số mà wrapper nhận được (bất kể có bao nhiêu) cho hàm ban đầu.


## Mượn một phương thức (Borrowing a method)

Bây giờ, hãy thực hiện thêm một cải tiến nhỏ trong hashing function:

```js
function hash(args) {
  return args[0] + ',' + args[1];
}
```

Đến bây giờ, nó chỉ hoạt động trên hai đối số. Sẽ tốt hơn nếu nó có thể dán bất kỳ số lượng `args` nào.

Giải pháp tự nhiên sẽ là sử dụng phương thức [arr.join](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/join):

```js
function hash(args) {
  return args.join();
}
```

...Thật không may, điều đó sẽ không làm việc. Bởi vì chúng ta đang gọi đối tượng `hash(arguments)` và `arguments` object vừa có thể iterable vừa array-like, nhưng không phải là một mảng thực sự.

Vì vậy, việc gọi `join` vào nó sẽ thất bại, như chúng ta có thể thấy dưới đây:

```js
function hash() {
  alert( arguments.join() ); // Error: arguments.join is not a function
}

hash(1, 2);
```

Tuy nhiên, có một cách dễ dàng để sử dụng array join:

```js
function hash() {
  alert( [].join.call(arguments) ); // 1,2
}

hash(1, 2);
```

Thủ thuật được gọi là *method borrowing*.

Chúng ta lấy (mượn) một phương thức nối từ một mảng thông thường `[].join`. Và sử dụng `[].join.call` để chạy nó trong ngữ cảnh của `arguments`.

Tại sao nó hoạt động?

Đó là bởi vì thuật toán bên trong của phương thức gốc `arr.join(glue)` rất đơn giản.

Lấy từ đặc tả gần như "nguyên trạng":

1. Đặt `glue` là đối số đầu tiên hoặc, nếu không có đối số, thì dấu phẩy `","`.
2. Đặt `result` là một chuỗi rỗng.
3. Nối `this[0]` vào `result`.
4. Nối `glue` và `this[1]`.
5. Nối `glue` và `this[2]`.
6. ...Làm như vậy cho đến khi các items `this.length` đều được dán.
7. Return `result`.

Vì vậy, về mặt kỹ thuật, nó cần `this` và joins `this[0]`, `this[1]` ...etc vào với nhau. Nó được viết một cách có chủ ý theo cách cho phép bất kỳ array-like `this` (không phải là trùng hợp ngẫu nhiên, nhiều phương thức tuân theo thực tiễn này). Đó là lý do tại sao nó cũng hoạt động với `this=arguments`.

## Tóm lược

*Decorator* là một wrapper xung quanh một function làm thay đổi hành vi của nó. Công việc chính vẫn được thực hiện bởi function.

Nói chung là an toàn để thay thế một function hoặc một phương thức bằng một decorated, ngoại trừ một điều nhỏ. Nếu original function có các thuộc tính trên nó, như `func.calledCount` hoặc bất cứ thứ gì, thì decorated sẽ không cung cấp chúng. Bởi vì đó là một wrapper. Vì vậy, người ta cần phải cẩn thận nếu sử dụng chúng. Một số decorators cung cấp thuộc tính (properties) riêng của chúng.

Decorators có thể được xem là "tính năng" hoặc "khía cạnh" có thể được thêm vào một function. Chúng ta có thể thêm một hoặc thêm nhiều. Và tất cả điều này không thay đổi mã của nó!

Để thực hiện `cachingDecorator`, chúng ta đã nghiên cứu các phương thức:

- [func.call(context, arg1, arg2...)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/call) -- gọi `func` với bối cảnh và đối số đã cho.
- [func.apply(context, args)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/apply) -- gọi `func` truyền `context` như `this` và array-like `args` vào một danh sách các đối số.

*Chuyển tiếp cuộc gọi (call forwarding)* chung thường được thực hiện với `apply`:

```js
let wrapper = function() {
  return original.apply(this, arguments);
}
```

Chúng ta cũng đã thấy một ví dụ về *mượn phương thức (method borrowing)* khi chúng ta lấy một phương thức từ một đối tượng và `call` nó trong ngữ cảnh của một đối tượng khác. Nó là khá phổ biến để thực hiện các phương thức mảng và áp dụng chúng cho các đối số. Sự thay thế là sử dụng rest parameters object là một mảng thực (The alternative is to use rest parameters object that is a real array).

Có rất nhiều decorators có trong tự nhiên. Kiểm tra xem bạn nhận biết chúng tốt như thế nào bằng cách giải quyết các nhiệm vụ của chương này.
