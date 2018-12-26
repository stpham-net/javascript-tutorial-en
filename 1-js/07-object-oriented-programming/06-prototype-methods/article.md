
# Methods for prototypes

Trong chương này, chúng ta đề cập đến các phương thức bổ sung để làm việc với một prototype.

Ngoài ra còn có các cách khác để get/set một prototype, bên cạnh những cách mà chúng ta đã biết:

- [Object.create(proto[, descriptors])](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/create) -- tạo một empty object với `proto` là `[[Prototype]]` và các property descriptors tùy chọn.
- [Object.getPrototypeOf(obj)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/getPrototypeOf) -- trả về `[[Prototype]]` của `obj`.
- [Object.setPrototypeOf(obj, proto)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/setPrototypeOf) -- đặt `[[Prototype]]` của `obj` thành `proto`.

Ví dụ:

```js
let animal = {
  eats: true
};

// create a new object with animal as a prototype
let rabbit = Object.create(animal);

alert(rabbit.eats); // true
alert(Object.getPrototypeOf(rabbit) === animal); // get the prototype of rabbit

Object.setPrototypeOf(rabbit, {}); // change the prototype of rabbit to {}
```

`Object.create` có một đối số thứ hai tùy chọn: property descriptors. Chúng ta có thể cung cấp các thuộc tính bổ sung cho đối tượng mới ở đó, như thế này:

```js
let animal = {
  eats: true
};

let rabbit = Object.create(animal, {
  jumps: {
    value: true
  }
});

alert(rabbit.jumps); // true
```

Các descriptors có cùng định dạng như được mô tả trong chương **property-descriptors**.

Chúng ta có thể sử dụng `Object.create` để thực hiện một object cloning mạnh hơn so với sao chép các thuộc tính trong `for..in`:

```js
// fully identical shallow clone of obj
let clone = Object.create(Object.getPrototypeOf(obj), Object.getOwnPropertyDescriptors(obj));
```

Cuộc gọi này tạo ra một bản sao chính xác của `obj`, bao gồm tất cả các thuộc tính: có thể đếm được (enumerable) và không thể đếm được (non-enumerable), data properties và setters/getters -- tất cả mọi thứ và với đúng `[[Prototype]]`.

## Lịch sử tóm tắt

Nếu chúng ta đếm tất cả các cách để quản lý `[[Prototype]]`, thì có rất nhiều! Nhiều cách để làm như vậy!

Tại sao vậy?

Đó là vì lý do lịch sử.

- Thuộc tính `"prototype"` của một constructor function hoạt động từ thời rất xa xưa.
- Cuối năm 2012: `Object.create` xuất hiện trong tiêu chuẩn. Nó cho phép tạo các đối tượng với nguyên mẫu đã cho, nhưng không cho phép get/set nó. Vì vậy, các trình duyệt đã triển khai non-standard `__proto__` accessor cho phép get/set một prototype bất cứ lúc nào.
- Cuối năm 2015: `Object.setPrototypeOf` và `Object.getPrototypeOf` đã được thêm vào tiêu chuẩn. `__proto__` đã được triển khai trên thực tế ở mọi nơi, do đó, nó đã đi đến Phụ lục B của tiêu chuẩn, là tùy chọn cho các môi trường không có trình duyệt.

Cho đến bây giờ chúng ta có tất cả những cách này theo ý của chúng ta.

Về mặt kỹ thuật, chúng ta có thể get/set `[[Prototype]]` bất cứ lúc nào. Nhưng thông thường chúng ta chỉ đặt nó một lần vào thời điểm tạo đối tượng và sau đó không sửa đổi: `rabbit` thừa hưởng từ `animal`, và điều đó sẽ không thay đổi. Và các JavaScript engines được tối ưu hóa cao cho điều đó. Thay đổi nguyên mẫu "đang hoạt động" bằng `Object.setPrototypeOf` hoặc `obj.__proto__=` là một hoạt động rất chậm. Nhưng nó có thể.

## Đối tượng "rất đơn giản" ("Very plain" objects)

Như chúng ta biết, các đối tượng có thể được sử dụng như các mảng kết hợp để lưu trữ các cặp key/value.

...Nhưng nếu chúng ta cố lưu trữ các khóa *do người dùng cung cấp* trong đó (ví dụ: từ điển do người dùng nhập), chúng ta có thể thấy một trục trặc thú vị: tất cả các khóa đều hoạt động tốt ngoại trừ `"__proto__"`.

Kiểm tra ví dụ này:

```js
let obj = {};

let key = prompt("What's the key?", "__proto__");
obj[key] = "some value";

alert(obj[key]); // [object Object], not "some value"!
```

Ở đây nếu người dùng gõ vào `__proto__`, việc gán sẽ bị bỏ qua!

Điều đó không làm chúng ta ngạc nhiên. Thuộc tính `__proto__` là đặc biệt: nó phải là một đối tượng hoặc `null`, một chuỗi không thể trở thành một prototype.

Nhưng chúng ta không có ý định thực hiện hành vi đó, phải không? Chúng ta muốn lưu trữ các cặp key/value và khóa có tên `"__proto__"` không được lưu đúng cách. Vì vậy, đó là một bug. Ở đây hậu quả không khủng khiếp. Nhưng trong các trường hợp khác, nguyên mẫu thực sự có thể bị thay đổi, do đó việc thực thi có thể sai theo những cách hoàn toàn bất ngờ.

Điều tồi tệ nhất -- thường là các nhà phát triển không nghĩ về khả năng đó. Điều đó làm cho các bugs như vậy khó nhận thấy và thậm chí biến chúng thành các lỗ hổng, đặc biệt là khi JavaScript được sử dụng ở phía máy chủ.

Điều đó chỉ xảy ra với `__proto__`. Tất cả các thuộc tính khác là "có thể gán" bình thường.

Làm thế nào để tránh vấn đề?

Đầu tiên, chúng ta có thể chuyển sang sử dụng `Map`, sau đó mọi thứ đều ổn.

Nhưng `Object` cũng có thể phục vụ chúng ta tốt ở đây, bởi vì những người sáng tạo ngôn ngữ đã suy nghĩ về vấn đề đó từ lâu.

`__proto__` không phải là một thuộc tính của một đối tượng, mà là một thuộc tính accessor của `Object.prototype`:

![](object-prototype-2.png)

Vì vậy, nếu `obj.__proto__` được đọc hoặc gán, thì getter/setter tương ứng được gọi từ nguyên mẫu của nó, và nó gets/sets `[[Prototype]]`.

Như đã nói lúc đầu: `__proto__` là một cách để truy cập `[[Prototype]]`, nó không phải là chính `[[Prototype]]`.

Bây giờ, nếu chúng ta muốn sử dụng một đối tượng như một mảng kết hợp, chúng ta có thể thực hiện nó với một mẹo nhỏ:

```js
let obj = Object.create(null);

let key = prompt("What's the key?", "__proto__");
obj[key] = "some value";

alert(obj[key]); // "some value"
```

`Object.create(null)` tạo một đối tượng trống mà không có nguyên mẫu (`[[Prototype]]` là `null`):

![](object-prototype-null.png)

Vì vậy, không có getter/setter được kế thừa cho `__proto__`. Bây giờ nó được xử lý như một thuộc tính dữ liệu thông thường, vì vậy ví dụ trên hoạt động đúng.

Chúng ta có thể gọi đối tượng như vậy là "rất đơn giản" hoặc "đối tượng từ điển thuần túy (pure dictionary objects)", bởi vì chúng thậm chí còn đơn giản hơn đối tượng đơn giản thông thường `{...}`.

Một nhược điểm là các đối tượng như vậy thiếu bất kỳ phương thức built-in object nào, ví dụ `toString`:

```js
let obj = Object.create(null);

alert(obj); // Error (no toString)
```

...Nhưng điều đó thường tốt cho các mảng kết hợp.

Xin lưu ý rằng hầu hết các phương thức liên quan đến đối tượng là `Object.something(...)`, như `Object.keys(obj)` -- chúng không có trong prototype, vì vậy chúng sẽ tiếp tục hoạt động trên các đối tượng như vậy:

```js
let chineseDictionary = Object.create(null);
chineseDictionary.hello = "ni hao";
chineseDictionary.bye = "zai jian";

alert(Object.keys(chineseDictionary)); // hello,bye
```

## Getting all properties

Có nhiều cách để lấy keys/values từ một đối tượng.

Chúng ta đã biết những cách này:

- [Object.keys(obj)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/keys) / [Object.values(obj)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/values) / [Object.entries(obj)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/entries) -- returns an array of enumerable own string property names/values/key-value pairs. Các phương thức này chỉ liệt kê các thuộc tính *enumerable* và các thuộc tính có *chuỗi là khóa (strings as keys)*.

Nếu chúng ta muốn các thuộc tính symbolic:

- [Object.getOwnPropertySymbols(obj)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/getOwnPropertySymbols) -- returns an array of all own symbolic property names.

Nếu chúng ta muốn các thuộc tính non-enumerable:

- [Object.getOwnPropertyNames(obj)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/getOwnPropertyNames) -- returns an array of all own string property names.

Nếu chúng ta muốn *tất cả* các thuộc tính:

- [Reflect.ownKeys(obj)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Reflect/ownKeys) -- returns an array of all own property names.

Các phương thức này hơi khác một chút về các thuộc tính mà chúng trả về, nhưng tất cả chúng đều hoạt động trên chính đối tượng. Các thuộc tính từ prototype không được liệt kê.

Vòng lặp `for..in` là khác: nó cũng lặp lại các thuộc tính được kế thừa.

Ví dụ:

```js
let animal = {
  eats: true
};

let rabbit = {
  jumps: true,
  __proto__: animal
};

// only own keys
alert(Object.keys(rabbit)); // jumps

// inherited keys too
for(let prop in rabbit) alert(prop); // jumps, then eats
```

Nếu chúng ta muốn phân biệt các thuộc tính được kế thừa, có một phương thức tích hợp [obj.hasOwnProperty(key)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/hasOwnProperty): nó trả về `true` nếu `obj` có thuộc tính riêng (không được kế thừa) có tên là `key`.

Vì vậy, chúng ta có thể lọc ra các thuộc tính được kế thừa (hoặc làm một cái gì đó khác với chúng):

```js
let animal = {
  eats: true
};

let rabbit = {
  jumps: true,
  __proto__: animal
};

for(let prop in rabbit) {
  let isOwn = rabbit.hasOwnProperty(prop);
  alert(`${prop}: ${isOwn}`); // jumps:true, then eats:false
}
```

Ở đây chúng ta có chuỗi thừa kế (inheritance chain) sau: `rabbit`, rồi `animal`, rồi `Object.prototype` (vì `animal` là một literal object `{...}`, so it's by default), và sau đó là `null` trên nó:

![](rabbit-animal-object.png)

Lưu ý, có một điều buồn cười. Phương thức `rabbit.hasOwnProperty` đến từ đâu? Nhìn vào chuỗi chúng ta có thể thấy rằng phương thức được cung cấp bởi `Object.prototype.hasOwnProperty`. Nói cách khác, nó được kế thừa.

...Nhưng tại sao `hasOwnProperty` không xuất hiện trong vòng lặp `for..in`, nếu nó liệt kê tất cả các thuộc tính được kế thừa?  Câu trả lời rất đơn giản: nó không phải là enumerable. Cũng giống như tất cả các thuộc tính khác của `Object.prototype`. Đó là lý do tại sao chúng không được liệt kê.

## Tóm lược

Dưới đây là danh sách ngắn gọn các phương thức mà chúng ta đã thảo luận trong chương này -- dưới dạng tóm tắt:

- [Object.create(proto[, descriptors])](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/create) -- creates an empty object with given `proto` as `[[Prototype]]` (can be `null`) and optional property descriptors.
- [Object.getPrototypeOf(obj)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object.getPrototypeOf) -- returns the `[[Prototype]]` of `obj` (same as `__proto__` getter).
- [Object.setPrototypeOf(obj, proto)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object.setPrototypeOf) -- sets the `[[Prototype]]` of `obj` to `proto` (same as `__proto__` setter).
- [Object.keys(obj)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/keys) / [Object.values(obj)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/values) / [Object.entries(obj)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/entries) -- returns an array of enumerable own string property names/values/key-value pairs.
- [Object.getOwnPropertySymbols(obj)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/getOwnPropertySymbols) -- returns an array of all own symbolic property names.
- [Object.getOwnPropertyNames(obj)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/getOwnPropertyNames) -- returns an array of all own string property names.
- [Reflect.ownKeys(obj)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Reflect/ownKeys) -- returns an array of all own property names.
- [obj.hasOwnProperty(key)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/hasOwnProperty): it returns `true` if `obj` has its own (not inherited) property named `key`.

Chúng ta cũng đã làm rõ rằng `__proto__` là một getter/setter cho `[[Prototype]]` và nằm trong `Object.prototype`, giống như các phương thức khác.

Chúng ta có thể tạo một đối tượng mà không cần nguyên mẫu bằng `Object.create(null)`. Các đối tượng như vậy được sử dụng như "từ điển thuần túy (pure dictionaries)", chúng không có vấn đề gì với `"__proto__"` làm khóa.

Tất cả các phương thức trả về các thuộc tính đối tượng (như `Object.keys` và các phương thức khác) -- trả về các thuộc tính "riêng" ("own" properties). Nếu chúng ta muốn cả những thuộc tính được thừa kế, thì chúng ta có thể sử dụng `for..in`.
