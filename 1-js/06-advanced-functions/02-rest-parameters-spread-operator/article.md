# Các tham số rest và toán tử spread

Nhiều built-in functions của JavaScript hỗ trợ số lượng đối số tùy ý.

Ví dụ:

- `Math.max(arg1, arg2, ..., argN)` -- trả về giá trị lớn nhất trong các đối số.
- `Object.assign(dest, src1, ..., srcN)` -- sao chép các thuộc tính từ `src1..N` chuyển vào `dest`.
- ...and so on.

Trong chương này chúng ta sẽ học cách làm tương tự. Và, quan trọng hơn, làm thế nào để cảm thấy thoải mái khi làm việc với các functions và arrays như vậy.

## Rest parameters `...`

Một hàm có thể được gọi với bất kỳ số lượng đối số nào, bất kể nó được định nghĩa như thế nào.

Giống như ở đây:

```js
function sum(a, b) {
  return a + b;
}

alert( sum(1, 2, 3, 4, 5) );
```

Sẽ không có lỗi vì các đối số "thừa". Nhưng tất nhiên trong kết quả chỉ có cái đầu tiên sẽ được tính.

Các tham số còn lại có thể được đề cập trong định nghĩa hàm với ba dấu chấm `...`. Chúng có nghĩa đen là "tập hợp các tham số còn lại thành một mảng".

Chẳng hạn, để tập hợp tất cả các đối số thành mảng `args`:

```js
function sumAll(...args) { // args is the name for the array
  let sum = 0;

  for (let arg of args) sum += arg;

  return sum;
}

alert( sumAll(1) ); // 1
alert( sumAll(1, 2) ); // 3
alert( sumAll(1, 2, 3) ); // 6
```

Chúng ta có thể chọn lấy các tham số đầu tiên dưới dạng các biến và chỉ thu thập phần còn lại.

Ở đây, hai đối số đầu tiên đi vào các biến và phần còn lại đi vào mảng `titles`:

```js
function showName(firstName, lastName, ...titles) {
  alert( firstName + ' ' + lastName ); // Julius Caesar

  // the rest go into titles array
  // i.e. titles = ["Consul", "Imperator"]
  alert( titles[0] ); // Consul
  alert( titles[1] ); // Imperator
  alert( titles.length ); // 2
}

showName("Julius", "Caesar", "Consul", "Imperator");
```

<br>

> ---

**📌 Các tham số rest phải ở cuối**

Các tham số còn lại (rest parameters) thu thập tất cả các đối số còn lại, do đó, vd sau đây không có ý nghĩa và gây ra lỗi:

```js
function f(arg1, ...rest, arg2) { // arg2 after ...rest ?!
  // error
}
```

The `...rest` phải luôn luôn là cuối cùng.

> ---

<br>

## Biến "đối số" (The "arguments" variable)

Ngoài ra còn có một array-like object đặc biệt có tên là `arguments` chứa tất cả các đối số theo chỉ mục của chúng.

Ví dụ:

```js
function showName() {
  alert( arguments.length );
  alert( arguments[0] );
  alert( arguments[1] );

  // it's iterable
  // for(let arg of arguments) alert(arg);
}

// shows: 2, Julius, Caesar
showName("Julius", "Caesar");

// shows: 1, Ilya, undefined (no second argument)
showName("Ilya");
```

Vào thời xưa, rest parameters không tồn tại trong ngôn ngữ và sử dụng `arguments` là cách duy nhất để có được tất cả các đối số của hàm, bất kể tổng số của chúng.

Và nó vẫn hoạt động, chúng ta có thể sử dụng nó ngày hôm nay.

Nhưng nhược điểm là mặc dù `arguments` vừa array-like vừa có thể iterable, nhưng nó không phải là một mảng. Nó không hỗ trợ các phương thức mảng, vì vậy chúng ta không thể gọi `arguments.map(...)` chẳng hạn.

Ngoài ra, nó luôn chứa tất cả các đối số. Chúng ta không thể lấy chúng một phần, giống như chúng ta đã làm với các rest parameters.

Vì vậy, khi chúng ta cần các tính năng này, thì các rest parameters được ưa thích.

<br>

> ---

**📌 Các arrow functions không có `"arguments"`**

Nếu chúng ta truy cập đối tượng `arguments` từ một arrow function, nó sẽ lấy chúng từ "normal" function bên ngoài.

Đây là một ví dụ:

```js
function f() {
  let showArg = () => alert(arguments[0]);
  showArg();
}

f(1); // 1
```

> ---

<br>

Như chúng ta nhớ, các arrow functions không có `this` riêng. Bây giờ chúng ta biết chúng cũng không có đối tượng đặc biệt `arguments`.

## Toán tử trải rộng (Spread operator)

Chúng ta vừa thấy làm thế nào để có được một mảng từ danh sách các tham số.

Nhưng đôi khi chúng ta cần làm chính xác điều ngược lại.

Chẳng hạn, có một hàm tích hợp [Math.max](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Math/max) trả về số lớn nhất từ một danh sách:

```js
alert( Math.max(3, 5, 1) ); // 5
```

Bây giờ hãy nói rằng chúng ta có một mảng `[3, 5, 1]`. Làm thế nào để chúng ta gọi `Math.max` với nó?

Truyền nó "như là" sẽ không hoạt động, bởi vì `Math.max` mong đợi một danh sách các đối số số, không phải là một mảng duy nhất:

```js
let arr = [3, 5, 1];

alert( Math.max(arr) ); // NaN
```

Và chắc chắn chúng ta không thể liệt kê các mục theo cách thủ công trong mã `Math.max(arr[0], arr[1], arr[2])`, bởi vì chúng ta có thể không chắc chắn có bao nhiêu. Khi kịch bản của chúng ta thực thi, có thể có rất nhiều, hoặc có thể không có. Và điều đó sẽ trở nên xấu xí.

*Spread operator* để giải cứu! Nó trông tương tự như các rest parameters, cũng sử dụng `...`, nhưng hoàn toàn ngược lại.

Khi `...arr` được sử dụng trong lệnh gọi hàm, nó "mở rộng" một iterable object `arr` vào danh sách các đối số.

Đối với `Math.max`:

```js
let arr = [3, 5, 1];

alert( Math.max(...arr) ); // 5 (spread turns array into a list of arguments)
```

Chúng ta cũng có thể vượt qua nhiều iterables theo cách này:

```js
let arr1 = [1, -2, 3, 4];
let arr2 = [8, 3, -8, 1];

alert( Math.max(...arr1, ...arr2) ); // 8
```

Chúng ta thậm chí có thể kết hợp spread operator với các giá trị bình thường:


```js
let arr1 = [1, -2, 3, 4];
let arr2 = [8, 3, -8, 1];

alert( Math.max(1, ...arr1, 2, ...arr2, 25) ); // 25
```

Ngoài ra, the spread operator có thể được sử dụng để hợp nhất các mảng:

```js
let arr = [3, 5, 1];
let arr2 = [8, 9, 15];

let merged = [0, ...arr, 2, ...arr2];

alert(merged); // 0,3,5,1,2,8,9,15 (0, then arr, then 2, then arr2)
```

Trong các ví dụ ở trên, chúng ta đã sử dụng một mảng để chứng minh the spread operator, nhưng bất kỳ iterable nào cũng sẽ làm được.

Chẳng hạn, ở đây chúng ta sử dụng spread operator để biến chuỗi thành mảng các ký tự:

```js
let str = "Hello";

alert( [...str] ); // H,e,l,l,o
```

The spread operator trong nội bộ sử dụng các trình vòng lặp (iterators) để thu thập các phần tử, giống như cách `for..of` thực hiện.

Vì vậy, đối với một chuỗi, `for..of` trả về các ký tự và `...str` trở thành `"H","e","l","l","o"`. Danh sách các ký tự được truyền cho bộ khởi tạo mảng (array initializer) `[...str]`.

Đối với tác vụ cụ thể này, chúng ta cũng có thể sử dụng `Array.from`, vì nó chuyển đổi một iterable (như một chuỗi) thành một mảng:

```js
let str = "Hello";

// Array.from converts an iterable into an array
alert( Array.from(str) ); // H,e,l,l,o
```

Kết quả giống như `[...str]`.

Nhưng có một sự khác biệt tinh tế giữa `Array.from(obj)` và `[...obj]`:

- `Array.from` hoạt động trên cả hai array-likes và iterables.
- The spread operator chỉ hoạt động trên các iterables.

Vì vậy, đối với nhiệm vụ biến một cái gì đó thành một mảng, `Array.from` có xu hướng phổ quát hơn.


## Tóm lược

Khi chúng ta thấy `"..."` trong mã, đó là rest parameters hoặc spread operator.

Có một cách dễ dàng để phân biệt giữa chúng:

- Khi `...` ở cuối các tham số hàm, đó là "rest parameters" và tập hợp phần còn lại của danh sách các đối số thành một mảng.
- Khi `...` xảy ra trong một cuộc gọi hàm hoặc tương tự, nó được gọi là "spread operator" và mở rộng một mảng thành một danh sách.

Sử dụng các mẫu:

- Các rest parameters được sử dụng để tạo các hàm chấp nhận bất kỳ số lượng đối số.
- The spread operator được sử dụng để truyền một mảng cho các hàm thường yêu cầu một danh sách gồm nhiều đối số.

Chúng cùng nhau giúp đi lại giữa một danh sách (a list) và một loạt các tham số (an array of parameters) một cách dễ dàng.

Tất cả các đối số của một lệnh gọi hàm cũng có sẵn trong "old-style" `arguments`: array-like iterable object.
No search results.
