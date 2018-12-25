# F.prototype

Trong JavaScript hiện đại, chúng ta có thể thiết lập một nguyên mẫu bằng cách sử dụng `__proto__`, như được mô tả trong bài viết trước. Nhưng nó không như thế mọi lúc.

JavaScript đã có sự kế thừa nguyên mẫu ngay từ đầu. Đó là một trong những tính năng cốt lõi của ngôn ngữ.

Nhưng trong thời xưa, có một cách khác (và duy nhất) để đặt nó: sử dụng thuộc tính `"prototype"` của constructor function. Và vẫn còn nhiều kịch bản sử dụng nó.

## The "prototype" property

Như chúng ta đã biết, `new F()` tạo ra một đối tượng mới.

Khi một đối tượng mới được tạo bằng `new F()`, `[[Prototype]]` của đối tượng được đặt thành `F.prototype`.

Nói cách khác, nếu `F` có thuộc tính `prototype` với giá trị của kiểu đối tượng, thì toán tử `new` sử dụng nó để đặt `[[Prototype]]` cho đối tượng mới.

Xin lưu ý rằng `F.prototype` ở đây có nghĩa là một thuộc tính thông thường có tên `"prototype"` trên `F`. Nghe có vẻ giống với thuật ngữ "prototype", nhưng ở đây chúng ta thực sự có ý là một thuộc tính thông thường với tên này.

Đây là ví dụ:

```js
let animal = {
  eats: true
};

function Rabbit(name) {
  this.name = name;
}

Rabbit.prototype = animal;

let rabbit = new Rabbit("White Rabbit"); //  rabbit.__proto__ == animal

alert( rabbit.eats ); // true
```

Đặt `Rabbit.prototype = animal` theo nghĩa đen là: "Khi một `new Rabbit` được tạo, gán `[[Prototype]]` cho `animal`".

Đây là hình ảnh kết quả:

![](proto-constructor-animal-rabbit.png)

Trên hình, `"prototype"` là một mũi tên nằm ngang, nó là một thuộc tính thông thường và `[[Prototype]]` là dọc, có nghĩa là sự kế thừa của `rabbit` từ `animal`.

## Default F.prototype, constructor property

Mọi hàm đều có thuộc tính `"prototype"` ngay cả khi chúng ta không cung cấp nó.

The default `"prototype"` là một đối tượng có thuộc tính `constructor` duy nhất quay lại chính hàm đó.

Như thế này:

```js
function Rabbit() {}

/* default prototype
Rabbit.prototype = { constructor: Rabbit };
*/
```

![](function-prototype-constructor.png)

Chúng ta có thể kiểm tra nó:

```js
function Rabbit() {}
// by default:
// Rabbit.prototype = { constructor: Rabbit }

alert( Rabbit.prototype.constructor == Rabbit ); // true
```

Một cách tự nhiên, nếu chúng ta không làm gì, thuộc tính `constructor` có sẵn cho tất cả các con thỏ thông qua `[[Prototype]]`:

```js
function Rabbit() {}
// by default:
// Rabbit.prototype = { constructor: Rabbit }

let rabbit = new Rabbit(); // inherits from {constructor: Rabbit}

alert(rabbit.constructor == Rabbit); // true (from prototype)
```

![](rabbit-prototype-constructor.png)

Chúng ta có thể sử dụng thuộc tính `constructor` để tạo một đối tượng mới sử dụng cùng constructor như đối tượng hiện có.

Giống như ở đây:

```js
function Rabbit(name) {
  this.name = name;
  alert(name);
}

let rabbit = new Rabbit("White Rabbit");

let rabbit2 = new rabbit.constructor("Black Rabbit");
```

Điều đó thật tiện lợi khi chúng ta có một đối tượng, không biết constructor  nào đã được sử dụng cho nó (ví dụ: nó đến từ thư viện của bên thứ 3) và chúng ta cần tạo một đối tượng khác cùng loại.

Nhưng có lẽ điều quan trọng nhất về `"constructor"` là...

**...Bản thân JavaScript không đảm bảo giá trị `"constructor"` đúng.**

Vâng, nó tồn tại trong `"prototype"` mặc định cho các hàm, nhưng đó là tất cả. Điều gì xảy ra với nó sau này -- hoàn toàn thuộc về chúng ta.

Cụ thể, nếu chúng ta thay thế toàn bộ default prototype, thì sẽ không có `"constructor"` trong đó.

Ví dụ:

```js
function Rabbit() {}
Rabbit.prototype = {
  jumps: true
};

let rabbit = new Rabbit();
alert(rabbit.constructor === Rabbit); // false
```

Vì vậy, để giữ đúng `"constructor"`, chúng ta có thể chọn thêm/xóa các thuộc tính vào `"prototype"` mặc định thay vì ghi đè lên toàn bộ:

```js
function Rabbit() {}

// Not overwrite Rabbit.prototype totally
// just add to it
Rabbit.prototype.jumps = true
// the default Rabbit.prototype.constructor is preserved
```

Hoặc, thay vào đó, tạo lại thuộc tính `constructor` bằng tay:

```js
Rabbit.prototype = {
  jumps: true,
  constructor: Rabbit
};

// bây giờ constructor cũng đúng, bởi vì chúng ta đã thêm nó
```

## Tóm lược

Trong chương này, chúng ta đã mô tả ngắn gọn cách thiết lập `[[Prototype]]` cho các đối tượng được tạo thông qua constructor function. Sau này chúng ta sẽ thấy các mẫu lập trình nâng cao hơn (advanced programming patterns) dựa vào nó.

Mọi thứ khá đơn giản, chỉ cần một vài ghi chú để làm cho mọi thứ rõ ràng:

- Thuộc tính `F.prototype` không giống với `[[Prototype]]`. Điều duy nhất `F.prototype` làm: nó thiết lập `[[Prototype]]` của các đối tượng mới khi `new F()` được gọi.
- Giá trị của `F.prototype` phải là một đối tượng hoặc null: các giá trị khác sẽ không hoạt động.
- Thuộc tính `"prototype"` chỉ có hiệu ứng đặc biệt như vậy khi được đặt cho một constructor function và được gọi với `new`.

Trên các đối tượng thông thường, `prototype` không có gì đặc biệt:

```js
let user = {
  name: "John",
  prototype: "Bla-bla" // no magic at all
};
```

Theo mặc định, tất cả các hàm đều có `F.prototype = { constructor: F }`, vì vậy chúng ta có thể lấy constructor của một đối tượng bằng cách truy cập thuộc tính `"constructor"` của nó.
