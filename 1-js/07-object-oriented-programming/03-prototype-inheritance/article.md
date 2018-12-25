# Kế thừa nguyên mẫu (Prototypal inheritance)

Trong lập trình, chúng ta thường muốn lấy một cái gì đó và mở rộng nó.

Chẳng hạn, chúng ta có một đối tượng `user` với các thuộc tính và phương thức của nó và muốn tạo `admin` và `guest` như các biến thể được sửa đổi một chút của nó. Chúng ta muốn sử dụng lại những gì chúng ta có trong `user`, không sao chép/thực hiện lại các phương thức của nó, chỉ cần xây dựng một đối tượng mới trên đầu (on top) của nó.

*Kế thừa nguyên mẫu (Prototypal inheritance)* là một tính năng ngôn ngữ giúp trong đó.

## [[Prototype]]

Trong JavaScript, các đối tượng có một thuộc tính ẩn đặc biệt `[[Prototype]]` (như được đặt tên trong đặc tả), đó là `null` hoặc tham chiếu một đối tượng khác. Đối tượng đó được gọi là "nguyên mẫu":

![prototype](object-prototype-empty.png)

Điều đó `[[Prototype]]` có nghĩa là "ma thuật". Khi chúng ta muốn đọc một thuộc tính từ `object` và nó bị thiếu, JavaScript sẽ tự động lấy nó từ prototype. Trong lập trình, điều đó được gọi là "prototypal inheritance". Nhiều tính năng ngôn ngữ thú vị và kỹ thuật lập trình được dựa trên nó.

Thuộc tính `[[Prototype]]` là nội bộ và ẩn, nhưng có nhiều cách để đặt nó.

Một trong số đó là sử dụng `__proto__`, như thế này:

```js
let animal = {
  eats: true
};
let rabbit = {
  jumps: true
};

rabbit.__proto__ = animal;
```

Xin lưu ý rằng `__proto__` là *không giống* như `[[Prototype]]`. Đó là một getter/setter cho nó. Chúng ta sẽ nói về những cách khác để thiết lập nó sau, nhưng bây giờ `__proto__` sẽ làm tốt thôi.

Nếu chúng ta tìm kiếm một thuộc tính trong `rabbit` và nó bị thiếu, JavaScript sẽ tự động lấy nó từ `animal`.

Ví dụ:

```js
let animal = {
  eats: true
};
let rabbit = {
  jumps: true
};

rabbit.__proto__ = animal; // (*)

// we can find both properties in rabbit now:
alert( rabbit.eats ); // true (**)
alert( rabbit.jumps ); // true
```

Ở đây, dòng `(*)` đặt `animal` là nguyên mẫu (prototype) của `rabbit`.

Sau đó, khi `alert` cố đọc thuộc tính `rabbit.eats` `(**)`, nó không ở `rabbit`, vì vậy JavaScript theo tham chiếu `[[Prototype]]` và tìm thấy nó trong `animal` (tìm từ dưới lên):

![](proto-animal-rabbit.png)

Ở đây chúng ta có thể nói rằng "`animal` là prototype của `rabbit`" hoặc "`rabbit` được thừa hưởng nguyên mẫu từ `animal`".

Vì vậy, nếu `animal` có nhiều đặc tính và phương thức hữu ích, thì chúng sẽ tự động có sẵn trong `rabbit`. Các tính chất như vậy được gọi là "thừa kế (inherited)".

Nếu chúng ta có một phương thức trong `animal`, nó có thể được gọi trên `rabbit`:

```js
let animal = {
  eats: true,
  walk() {
    alert("Animal walk");
  }
};

let rabbit = {
  jumps: true,
  __proto__: animal
};

// walk is taken from the prototype
rabbit.walk(); // Animal walk
```

Phương thức này được tự động lấy từ nguyên mẫu, như thế này:

![](proto-animal-rabbit-walk.png)

Chuỗi nguyên mẫu (prototype chain) có thể dài hơn:

```js
let animal = {
  eats: true,
  walk() {
    alert("Animal walk");
  }
};

let rabbit = {
  jumps: true,
  __proto__: animal
};

let longEar = {
  earLength: 10,
  __proto__: rabbit
}

// walk is taken from the prototype chain
longEar.walk(); // Animal walk
alert(longEar.jumps); // true (from rabbit)
```

![](proto-animal-rabbit-chain.png)

Thực tế chỉ có hai hạn chế:

1. Các tham chiếu không thể đi theo vòng tròn. JavaScript sẽ đưa ra lỗi nếu chúng ta cố gắng gán `__proto__` trong một vòng tròn.
2. Giá trị của `__proto__` có thể là một đối tượng hoặc `null`. Tất cả các giá trị khác (như nguyên thủy) được bỏ qua.

Ngoài ra nó có thể rõ ràng, nhưng vẫn: chỉ có thể có một `[[Prototype]]`. Một đối tượng có thể không được thừa kế từ hai đối tượng khác.

## Read/write rules

Các prototype chỉ được sử dụng để đọc thuộc tính.

Đối với các data properties (không phải getters/setters) các thao tác ghi/xóa làm việc trực tiếp với đối tượng.

Trong ví dụ dưới đây, chúng ta gán phương thức `walk` của riêng nó cho `rabbit`:

```js
let animal = {
  eats: true,
  walk() {
    /* this method won't be used by rabbit */  
  }
};

let rabbit = {
  __proto__: animal
}

rabbit.walk = function() {
  alert("Rabbit! Bounce-bounce!");
};

rabbit.walk(); // Rabbit! Bounce-bounce!
```

Từ giờ trở đi, `rabbit.walk()` gọi tìm phương thức ngay lập tức trong đối tượng và thực thi nó, mà không cần sử dụng prototype:

![](proto-animal-rabbit-walk-2.png)

Đối với getters/setters -- nếu chúng ta đọc/viết một thuộc tính, chúng được tra cứu trong prototype và được gọi.

Chẳng hạn, hãy xem thuộc tính `admin.fullName` trong mã dưới đây:

```js
let user = {
  name: "John",
  surname: "Smith",

  set fullName(value) {
    [this.name, this.surname] = value.split(" ");
  },

  get fullName() {
    return `${this.name} ${this.surname}`;
  }
};

let admin = {
  __proto__: user,
  isAdmin: true
};

alert(admin.fullName); // John Smith (*)

// setter triggers!
admin.fullName = "Alice Cooper"; // (**)
```

Ở đây trong dòng `(*)` thuộc tính `admin.fullName` có một getter trong nguyên mẫu `user`, vì vậy nó được gọi. Và trong dòng `(**)`, thuộc tính có một setter trong nguyên mẫu, vì vậy nó được gọi.

## Giá trị của "this"

Một câu hỏi thú vị có thể xuất hiện trong ví dụ trên: giá trị của `this` bên trong `set fullName(value)` là gì? Các thuộc tính `this.name` và `this.surname` được viết ở đâu: `user` hay `admin`?

Câu trả lời rất đơn giản: `this` hoàn toàn không bị ảnh hưởng bởi các nguyên mẫu.

**Bất kể phương thức được tìm thấy ở đâu: trong một đối tượng hoặc nguyên mẫu của nó. Trong một cuộc gọi phương thức, `this` luôn là đối tượng trước dấu chấm.**

Vì vậy, setter thực sự sử dụng `admin` là `this`, không phải `user`.

Đó thực sự là một điều cực kỳ quan trọng, bởi vì chúng ta có thể có một đối tượng lớn với nhiều phương thức và kế thừa từ nó. Sau đó, chúng ta có thể chạy các phương thức của nó trên các đối tượng được kế thừa và chúng sẽ sửa đổi trạng thái của các đối tượng này, chứ không phải đối tượng lớn.

Chẳng hạn, ở đây `animal` đại diện cho một "method storage" và `rabbit` sử dụng nó.

Cuộc gọi `rabbit.sleep()` đặt `this.isSleeping` trên đối tượng `rabbit`:

```js
// animal has methods
let animal = {
  walk() {
    if (!this.isSleeping) {
      alert(`I walk`);
    }
  },
  sleep() {
    this.isSleeping = true;
  }
};

let rabbit = {
  name: "White Rabbit",
  __proto__: animal
};

// modifies rabbit.isSleeping
rabbit.sleep();

alert(rabbit.isSleeping); // true
alert(animal.isSleeping); // undefined (no such property in the prototype)
```

Hình ảnh kết quả:

![](proto-animal-rabbit-walk-3.png)

Nếu chúng ta có các đối tượng khác như `bird`, `snake` v.v ... kế thừa từ `animal`, thì chúng cũng sẽ có quyền truy cập vào các phương thức của `animal`. Nhưng `this` trong mỗi phương thức sẽ là đối tượng tương ứng, được đánh giá tại thời điểm gọi (trước dấu chấm), chứ không phải `animal`. Vì vậy, khi chúng ta ghi dữ liệu vào `this`, nó được lưu trữ vào các đối tượng này.

Kết quả là các phương thức được chia sẻ, nhưng trạng thái đối tượng thì không.

## Tóm lược

- Trong JavaScript, tất cả các đối tượng đều có thuộc tính `[[Prototype]]` ẩn đó là một đối tượng khác hoặc `null`.
- Chúng ta có thể sử dụng `obj.__proto__` để truy cập nó (cũng có những cách khác sẽ sớm được đề cập tới).
- Đối tượng được tham chiếu bởi `[[Prototype]]` được gọi là "prototype".
- Nếu chúng ta muốn đọc một thuộc tính của `obj` hoặc gọi một phương thức và nó không tồn tại, thì JavaScript sẽ cố gắng tìm nó trong nguyên mẫu. Các thao tác ghi/xóa hoạt động trực tiếp trên đối tượng, chúng không sử dụng nguyên mẫu (trừ khi thuộc tính thực sự là một setter).
- Vì vậy, các phương thức luôn làm việc với đối tượng hiện tại ngay cả khi chúng được kế thừa.
