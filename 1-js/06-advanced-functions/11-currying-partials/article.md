
# Currying và partials

Cho đến bây giờ chúng ta mới chỉ nói về sự ràng buộc (binding) `this`. Hãy tiến thêm một bước nữa.

Chúng ta có thể liên kết (bind) không chỉ `this`, mà cả các đối số. Điều đó hiếm khi được thực hiện, nhưng đôi khi có thể có ích.

Cú pháp đầy đủ của `bind`:

```js
let bound = func.bind(context, arg1, arg2, ...);
```

Nó cho phép liên kết ngữ cảnh dưới dạng `this` và bắt đầu các đối số của hàm.

Chẳng hạn, chúng ta có hàm nhân `mul(a, b)`:

```js
function mul(a, b) {
  return a * b;
}
```

Chúng ta hãy sử dụng `bind` để tạo một hàm `double` trên cơ sở của nó:

```js
let double = mul.bind(null, 2);

alert( double(3) ); // = mul(2, 3) = 6
alert( double(4) ); // = mul(2, 4) = 8
alert( double(5) ); // = mul(2, 5) = 10
```

Lệnh gọi `mul.bind(null, 2)` tạo ra một hàm mới `double` chuyển các cuộc gọi đến `mul`, fixing `null` làm bối cảnh và `2` làm đối số đầu tiên. Đối số tiếp theo được thông qua "as is".

Đó gọi là [partial function application](https://en.wikipedia.org/wiki/Partial_application) -- chúng ta tạo một function mới bằng cách fixing một số tham số của cái hiện có.

Xin lưu ý rằng ở đây chúng ta thực sự không sử dụng `this` ở đây. Nhưng `bind` yêu cầu nó, vì vậy chúng ta phải đặt một cái gì đó như `null`.

Hàm `triple` trong mã bên dưới tăng gấp ba giá trị:

```js
let triple = mul.bind(null, 3);

alert( triple(3) ); // = mul(3, 3) = 9
alert( triple(4) ); // = mul(3, 4) = 12
alert( triple(5) ); // = mul(3, 5) = 15
```

Tại sao chúng ta thường tạo ra một partial function?

Ở đây lợi ích của chúng ta là chúng ta đã tạo ra một hàm độc lập với tên dễ đọc (`double`, `triple`). Chúng ta có thể sử dụng nó và không viết đối số đầu tiên bao giờ, vì nó được fixed với `bind`.

Trong các trường hợp khác, partial application rất hữu ích khi chúng ta có một function rất chung chung và muốn một biến thể ít phổ biến hơn cho thuận tiện.

Chẳng hạn, chúng ta có một hàm `send(from, to, text)`. Sau đó, bên trong một đối tượng `user`, chúng ta có thể muốn sử dụng một biến thể một phần của nó: `sendTo(to, text)` gửi từ người dùng hiện tại.

## Going partial without context

Điều gì xảy ra nếu chúng ta muốn fix một số đối số, nhưng không ràng buộc `this`?

Bản gốc `bind` không cho phép điều đó. Chúng ta không thể bỏ qua bối cảnh và chuyển sang các đối số.

May mắn thay, một hàm `partial` chỉ ràng buộc các đối số có thể được thực hiện.

Như thế này:

```js
function partial(func, ...argsBound) {
  return function(...args) { // (*)
    return func.call(this, ...argsBound, ...args);
  }
}

// Usage:
let user = {
  firstName: "John",
  say(time, phrase) {
    alert(`[${time}] ${this.firstName}: ${phrase}!`);
  }
};

// add a partial method that says something now by fixing the first argument
user.sayNow = partial(user.say, new Date().getHours() + ':' + new Date().getMinutes());

user.sayNow("Hello");
// Something like:
// [10:00] John: Hello!
```

Kết quả của `partial(func[, arg1, arg2...])` cuộc gọi là một wrapper `(*)` gọi `func` với:
- Tương tự `this` như nó nhận được (đối với `user.sayNow` gọi nó là `user`)
- Sau đó cung cấp cho nó `...argsBound` -- các đối số từ lệnh gọi `partial` (`"10:00"`)
- Sau đó cung cấp cho nó `...args` -- các đối số được cung cấp cho wrapper (`"Hello"`)

Thật dễ dàng để làm điều đó với toán tử lây lan (spread operator), phải không?

Ngoài ra, có một triển khai [\_.partial](https://lodash.com/docs#partial) từ thư viện lodash.

## Currying

Đôi khi mọi người trộn lẫn partial function application được đề cập ở trên với một thứ khác có tên là "currying". Đó là một kỹ thuật thú vị khác để làm việc với các functions mà chúng ta chỉ cần đề cập ở đây.

[Currying](https://en.wikipedia.org/wiki/Currying) đang dịch một hàm từ có thể gọi là `f(a, b, c)` thành có thể gọi là `f(a)(b)(c)`.

Hãy tạo hàm `curry` thực hiện chức năng currying cho các hàm nhị phân. Nói cách khác, nó dịch `f(a, b)` thành `f(a)(b)`:

```js
function curry(func) {
  return function(a) {
    return function(b) {
      return func(a, b);
    };
  };
}

// usage
function sum(a, b) {
  return a + b;
}

let carriedSum = curry(sum);

alert( carriedSum(1)(2) ); // 3
```

Như bạn có thể thấy, việc thực hiện là một loạt các wrappers.

- Kết quả của `curry(func)` là một wrapper `function(a)`.
- Khi được gọi là `sum(1)`, đối số được lưu trong Lexical Environment và một wrapper mới trả về `function(b)`.
- Sau đó `sum(1)(2)` cuối cùng gọi `function(b)` cung cấp `2`, và nó chuyển cuộc gọi đến đa đối số gốc `sum`.

Các triển khai nâng cao hơn của currying như [\_.curry](https://lodash.com/docs#curry) từ thư viện lodash làm một cái gì đó tinh vi hơn. Chúng trả về một wrapper cho phép một hàm được gọi bình thường khi tất cả các đối số được cung cấp *hoặc* trả về một phần khác.

```js
function curry(f) {
  return function(...args) {
    // if args.length == f.length (as many arguments as f has),
    //   then pass the call to f
    // otherwise return a partial function that fixes args as first arguments
  };
}
```

## Currying? What for?

Currying nâng cao cho phép cả hai giữ function có thể gọi được bình thường và có được các phần dễ dàng. Để hiểu những lợi ích chúng ta chắc chắn cần một ví dụ thực tế xứng đáng.

Chẳng hạn, chúng ta có logging function `log(date, importance, message)` định dạng và xuất thông tin. Trong các dự án thực, các functions như vậy cũng có nhiều tính năng hữu ích khác như gửi nó qua mạng hoặc lọc:

```js
function log(date, importance, message) {
  alert(`[${date.getHours()}:${date.getMinutes()}] [${importance}] ${message}`);
}
```

Let's curry it!

```js
log = _.curry(log);
```

Sau đó, `log` vẫn hoạt động theo cách thông thường:

```js
log(new Date(), "DEBUG", "some debug");
```

...Nhưng cũng có thể được gọi ở dạng curried:

```js
log(new Date())("DEBUG")("some debug"); // log(a)(b)(c)
```

Chúng ta hãy có một function tiện lợi cho today's logs:

```js
// todayLog will be the partial of log with fixed first argument
let todayLog = log(new Date());

// use it
todayLog("INFO", "message"); // [HH:mm] INFO message
```

Và bây giờ là một function tiện lợi cho các today's debug messages:

```js
let todayDebug = todayLog("DEBUG");

todayDebug("message"); // [HH:mm] DEBUG message
```

Vì thế:
1. Chúng ta đã không mất bất cứ thứ gì sau khi currying: `log` vẫn có thể gọi được bình thường.
2. Chúng ta đã có thể tạo ra các partial functions thuận tiện trong nhiều trường hợp.

## Thực hiện cà ri nâng cao (Advanced curry implementation)

Trong trường hợp bạn quan tâm, đây là cách triển khai cà ri "nâng cao" mà chúng ta có thể sử dụng ở trên.

```js
function curry(func) {

  return function curried(...args) {
    if (args.length >= func.length) {
      return func.apply(this, args);
    } else {
      return function(...args2) {
        return curried.apply(this, args.concat(args2));
      }
    }
  };

}

function sum(a, b, c) {
  return a + b + c;
}

let curriedSum = curry(sum);

// still callable normally
alert( curriedSum(1, 2, 3) ); // 6

// get the partial with curried(1) and call it with 2 other arguments
alert( curriedSum(1)(2,3) ); // 6

// full curried form
alert( curriedSum(1)(2)(3) ); // 6
```

`curry` mới có thể trông phức tạp, nhưng nó thực sự khá dễ hiểu.

Kết quả của `curry(func)` là wrapper `curried` trông như thế này:

```js
// func is the function to transform
function curried(...args) {
  if (args.length >= func.length) { // (1)
    return func.apply(this, args);
  } else {
    return function pass(...args2) { // (2)
      return curried.apply(this, args.concat(args2));
    }
  }
};
```

Khi chúng ta chạy nó, có hai nhánh:

1. Gọi ngay: nếu đếm `args` được truyền giống như original function có trong định nghĩa của nó (`func.length`) hoặc dài hơn, thì sau đó chỉ cần chuyển cuộc gọi đến nó.
2. Nhận một phần: nếu không, `func` chưa được gọi. Thay vào đó, một wrapper khác `pass` được trả về, nó sẽ re-apply `curried` cung cấp các đối số trước đó cùng với các đối số mới. Sau đó, trong một cuộc gọi mới, một lần nữa, chúng ta sẽ nhận được một phần mới (nếu không đủ đối số) hoặc, cuối cùng, là kết quả.

Chẳng hạn, hãy xem điều gì xảy ra trong trường hợp `sum(a, b, c)`. Ba đối số, vì vậy `sum.length = 3`.

Đối với cuộc gọi `curried(1)(2)(3)`:

1. Cuộc gọi đầu tiên `curried(1)` nhớ `1` trong Lexical Environment của nó và trả về một wrapper `pass`.
2. The wrapper `pass` được gọi với `(2)`: nó cần các đối số trước đó (`1`), nối chúng với những gì nó nhận được `(2)` và gọi `curried(1, 2)` với chúng.

    Khi số lượng đối số vẫn còn ít hơn 3, `curry` tiếp tục trả về `pass`.
    
3. The wrapper `pass` được gọi lại với `(3)`, cho cuộc gọi tiếp theo `pass(3)` lấy các đối số trước đó (`1`, `2`) và thêm `3` cho chúng, tạo ra cuộc gọi `curried(1, 2, 3)` -- cuối cùng sau khi có `3` đối số, chúng được trao cho original function.

Nếu điều đó vẫn chưa rõ ràng, chỉ cần theo dõi chuỗi cuộc gọi trong tâm trí của bạn hoặc trên giấy.

<br>

> ---

**📌 Fixed-length functions only**

Việc currying đòi hỏi hàm phải có một số lượng đối số cố định đã biết.

> ---

<br>
<br>

> ---

**📌 A little more than currying**

Theo định nghĩa, currying nên chuyển đổi `sum(a, b, c)` thành `sum(a)(b)(c)`.

Nhưng hầu hết các triển khai thực hiện currying trong JavaScript đều được nâng cao, như được mô tả: chúng cũng giữ function có thể gọi được trong biến thể đa đối số.

> ---

<br>

## Tóm lược

- Khi chúng ta fix một số đối số của hàm hiện có, hàm kết quả (ít phổ quát hơn) được gọi là *một phần*. Chúng ta có thể sử dụng `bind` để lấy một phần, nhưng cũng có những cách khác.

    Partials thuận tiện khi chúng ta không muốn lặp đi lặp lại cùng một lập luận. Giống như nếu chúng ta có hàm `send(from, to)` và `from` phải luôn giống nhau cho nhiệm vụ của chúng ta, chúng ta có thể lấy một phần và tiếp tục với nó.

- *Currying* là một biến đổi làm cho `f(a,b,c)` có thể gọi là `f(a)(b)(c)`. Việc triển khai JavaScript thường giữ cả hàm có thể gọi bình thường và trả về một phần nếu số lượng đối số là không đủ.

    Currying là tuyệt vời khi chúng ta muốn partials dễ dàng. Như chúng ta đã thấy trong ví dụ ghi nhật ký: hàm phổ quát `log(date, importance, message)` sau khi curry cung cấp cho chúng ta các phần khi được gọi với một đối số như `log(date)` hoặc hai đối số `log(date, importance)`.  
