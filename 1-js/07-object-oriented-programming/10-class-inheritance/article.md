
# Class inheritance, super

Các Classes có thể mở rộng (extend) lẫn nhau. Có một cú pháp hay, về mặt kỹ thuật dựa trên sự kế thừa nguyên mẫu (prototypal inheritance).

Để kế thừa từ một lớp khác, chúng ta nên chỉ định `"extends"` và lớp cha trước dấu ngoặc `{..}`.

Ở đây `Rabbit` kế thừa từ `Animal`:

```js
class Animal {

  constructor(name) {
    this.speed = 0;
    this.name = name;
  }

  run(speed) {
    this.speed += speed;
    alert(`${this.name} runs with speed ${this.speed}.`);
  }

  stop() {
    this.speed = 0;
    alert(`${this.name} stopped.`);
  }

}

// Inherit from Animal
class Rabbit extends Animal {
  hide() {
    alert(`${this.name} hides!`);
  }
}

let rabbit = new Rabbit("White Rabbit");

rabbit.run(5); // White Rabbit runs with speed 5.
rabbit.hide(); // White Rabbit hides!
```

Từ khóa `extends` thực sự thêm một tham chiếu `[[Prototype]]` từ `Rabbit.prototype` tới `Animal.prototype`, giống như bạn mong đợi, và như chúng ta đã thấy trước đây.

![](animal-rabbit-extends.png)

Vì vậy, bây giờ `rabbit` đã truy cập cả vào các phương thức riêng của nó và các phương thức của `Animal`.

<br>

> ---

**📌 Mọi biểu thức đều được cho phép sau khi `extends`**

Cú pháp class cho phép chỉ định không chỉ một lớp, mà bất kỳ biểu thức (expression) nào sau `extends`.

Ví dụ, một lệnh gọi hàm tạo class cha:

```js
function f(phrase) {
  return class {
    sayHi() { alert(phrase) }
  }
}

class User extends f("Hello") {}

new User().sayHi(); // Hello
```

Ở đây `class User` kế thừa từ kết quả của `f("Hello")`.

Điều đó có thể hữu ích cho các mẫu lập trình nâng cao khi chúng ta sử dụng các hàm để tạo các classes phụ thuộc thuộc vào nhiều điều kiện và có thể kế thừa từ chúng.

> ---

<br>

## Overriding a method

Bây giờ chúng ta hãy tiến lên và ghi đè một phương thức. Đến bây giờ, `Rabbit` thừa hưởng phương thức `stop` đặt `this.speed = 0` từ `Animal`.

Nếu chúng ta chỉ định `stop` của riêng mình trong `Rabbit`, thì nó sẽ được sử dụng thay thế:

```js
class Rabbit extends Animal {
  stop() {
    // ...this will be used for rabbit.stop()
  }
}
```

...Nhưng thông thường, chúng ta không muốn thay thế hoàn toàn một phương thức cha (parent method), mà là xây dựng trên đầu của nó (build on top of it), điều chỉnh hoặc mở rộng chức năng của nó. Chúng ta làm một cái gì đó trong phương thức của chúng ta, nhưng gọi phương thức cha trước/sau nó hoặc trong process.

Các classes cung cấp từ khóa `"super"` cho điều đó.

- `super.method(...)` để gọi phương thức cha.
- `super(...)` để gọi một parent constructor (chỉ bên trong constructor của chúng ta).

Ví dụ, hãy để rabbit của chúng ta autohide khi stopped:

```js
class Animal {

  constructor(name) {
    this.speed = 0;
    this.name = name;
  }

  run(speed) {
    this.speed += speed;
    alert(`${this.name} runs with speed ${this.speed}.`);
  }

  stop() {
    this.speed = 0;
    alert(`${this.name} stopped.`);
  }

}

class Rabbit extends Animal {
  hide() {
    alert(`${this.name} hides!`);
  }

  stop() {
    super.stop(); // call parent stop
    this.hide(); // and then hide
  }
}

let rabbit = new Rabbit("White Rabbit");

rabbit.run(5); // White Rabbit runs with speed 5.
rabbit.stop(); // White Rabbit stopped. White rabbit hides!
```

Bây giờ `Rabbit` có phương thức `stop` gọi phương thức cha là `super.stop()` trong process.

<br>

> ---

**📌 Các arrow functions không có `super`**

Như đã đề cập trong chương **arrow-functions**, arrow functions không có `super`.

Nếu được truy cập, nó được lấy từ outer function. Ví dụ:

```js
class Rabbit extends Animal {
  stop() {
    setTimeout(() => super.stop(), 1000); // call parent stop after 1sec
  }
}
```

`super` trong hàm mũi tên giống như trong  `stop()`, vì vậy nó hoạt động như dự định. Nếu chúng ta chỉ định "regular" function ở đây, sẽ có lỗi:

```js
// Unexpected super
setTimeout(function() { super.stop() }, 1000);
```

> ---

<br>

## Overriding constructor

Với các constructors, nó có một chút khó khăn.

Cho đến bây giờ, `Rabbit` không có `constructor` của riêng nó.

Theo [đặc tả](https://tc39.github.io/ecma262/#sec-runtime-semantics-classdefinitionevaluation), nếu một class mở rộng một class khác và không có `constructor`, thì `constructor` sau sẽ được tạo:

```js
class Rabbit extends Animal {
  // generated for extending classes without own constructors
  constructor(...args) {
    super(...args);
  }
}
```

Như chúng ta có thể thấy, về cơ bản nó gọi `constructor` cha truyền cho nó tất cả các đối số. Điều đó xảy ra nếu chúng ta không viết một constructor của riêng mình.

Bây giờ, hãy thêm một custom constructor vào `Rabbit`. Nó sẽ chỉ định `earLength` cộng thêm với `name`:

```js
class Animal {
  constructor(name) {
    this.speed = 0;
    this.name = name;
  }
  // ...
}

class Rabbit extends Animal {

  constructor(name, earLength) {
    this .speed = 0;
    this.name = name;
    this.earLength = earLength;
  }

  // ...
}

// Doesn't work!
let rabbit = new Rabbit("White Rabbit", 10); // Error: this is not defined.
```

Rất tiếc! Chúng ta đã có một lỗi. Bây giờ chúng ta không thể tạo ra rabbits. Có chuyện gì sai vậy?

Câu trả lời ngắn gọn là: các constructors trong các lớp kế thừa phải gọi `super(...)` và (!) làm điều đó trước khi sử dụng `this`.

...Nhưng tại sao? Chuyện gì đang xảy ra ở đây? Thật vậy, yêu cầu có vẻ lạ.

Tất nhiên, có một lời giải thích. Chúng ta hãy đi vào chi tiết, vì vậy bạn thực sự hiểu những gì đang xảy ra.

Trong JavaScript, có một sự khác biệt giữa "constructor function của một class kế thừa" và tất cả các class khác. Trong một class kế thừa, constructor function tương ứng được gắn nhãn với một thuộc tính nội bộ đặc biệt `[[ConstructorKind]]:"derived"`.

Sự khác biệt là:

- Khi một normal constructor chạy, nó tạo ra một empty object là `this` và tiếp tục với nó.
- Nhưng khi một derived (dẫn xuất) constructor chạy, nó không làm điều đó. Nó hy vọng các parent constructor sẽ làm công việc này.

Vì vậy, nếu chúng ta tạo một constructor của riêng mình, thì chúng ta phải gọi `super`, bởi vì nếu không thì đối tượng có tham chiếu `this` sẽ không được tạo. Và chúng ta sẽ nhận được một lỗi.

Để `Rabbit` hoạt động, chúng ta cần gọi `super()` trước khi sử dụng `this`, như ở đây:

```js
class Animal {

  constructor(name) {
    this.speed = 0;
    this.name = name;
  }

  // ...
}

class Rabbit extends Animal {

  constructor(name, earLength) {
    super(name);
    this.earLength = earLength;
  }

  // ...
}

// now fine
let rabbit = new Rabbit("White Rabbit", 10);
alert(rabbit.name); // White Rabbit
alert(rabbit.earLength); // 10
```

## Super: internals, [[HomeObject]]

Chúng ta hãy tìm hiểu sâu hơn một chút dưới mui xe của `super`. Nhân tiện, chúng ta sẽ thấy một số điều thú vị.

Đầu tiên phải nói, từ tất cả những gì chúng ta đã học cho đến bây giờ, không thể biết được làm thế nào `super` có thể hoạt động.

Vâng, thực sự, chúng ta hãy tự hỏi, làm thế nào nó có thể hoạt động về mặt kỹ thuật? Khi một phương thức đối tượng chạy, nó nhận được đối tượng hiện tại là `this`. Nếu chúng ta gọi `super.method()` thì làm thế nào để lấy `method`? Đương nhiên, chúng ta cần lấy `method` từ prototype của đối tượng hiện tại. Về mặt kỹ thuật, chúng ta (hoặc một JavaScript engine) có thể làm điều đó như thế nào?

Có lẽ chúng ta có thể lấy phương thức từ `[[Prototype]]` của `this`, như `this.__proto__.method`? Thật không may, điều đó không làm việc.

Hãy thử làm nó. Không có các classes, sử dụng các plain objects vì mục đích đơn giản.

Ở đây, `rabbit.eat()` nên gọi phương thức `animal.eat()` của parent object:

```js
let animal = {
  name: "Animal",
  eat() {
    alert(`${this.name} eats.`);
  }
};

let rabbit = {
  __proto__: animal,
  name: "Rabbit",
  eat() {
    // that's how super.eat() could presumably work
    this.__proto__.eat.call(this); // (*)
  }
};

rabbit.eat(); // Rabbit eats.
```

Tại dòng `(*)`, chúng ta lấy `eat` từ prototype (`animal`) và gọi nó trong ngữ cảnh của đối tượng hiện tại. Xin lưu ý rằng `.call(this)` rất quan trọng ở đây, bởi vì một đơn giản `this.__proto__.eat()` này sẽ thực thi parent `eat` trong ngữ cảnh của prototype, không phải đối tượng hiện tại.

Và trong đoạn mã trên, nó thực sự hoạt động như dự định: chúng ta có `alert` chính xác.

Bây giờ hãy thêm một đối tượng vào chuỗi (chain). Chúng ta sẽ thấy mọi thứ vỡ (break) như thế nào:

```js
let animal = {
  name: "Animal",
  eat() {
    alert(`${this.name} eats.`);
  }
};

let rabbit = {
  __proto__: animal,
  eat() {
    // ...bounce around rabbit-style and call parent (animal) method
    this.__proto__.eat.call(this); // (*)
  }
};

let longEar = {
  __proto__: rabbit,
  eat() {
    // ...do something with long ears and call parent (rabbit) method
    this.__proto__.eat.call(this); // (**)
  }
};

longEar.eat(); // Error: Maximum call stack size exceeded
```

Mã không hoạt động nữa! Chúng ta có thể thấy lỗi khi cố gắng gọi `longEar.eat()`.

Nó có thể không rõ ràng, nhưng nếu chúng ta theo dõi cuộc gọi `longEar.eat()`, thì chúng ta có thể thấy tại sao. Trong cả hai dòng `(*)` và `(**)`, giá trị của `this` là đối tượng hiện tại (`longEar`). Điều đó rất cần thiết: tất cả các object methods đều lấy đối tượng hiện tại là `this`, không phải là prototype hay thứ gì đó.

Vì vậy, trong cả hai dòng `(*)` và `(**)` giá trị của `this.__proto__` hoàn toàn giống nhau: `rabbit`. Cả hai đều gọi `rabbit.eat` mà không đi lên chuỗi (going up the chain) trong vòng lặp vô tận (endless loop).

Đây là bức tranh về những gì xảy ra:

![](this-super-loop.png)

1. Bên trong `longEar.eat()`, dòng `(**)` gọi `rabbit.eat` cung cấp cho nó `this=longEar`.

    ```js
    // inside longEar.eat() we have this = longEar
    this.__proto__.eat.call(this) // (**)
    // becomes
    longEar.__proto__.eat.call(this)
    // that is
    rabbit.eat.call(this);
    ```
    
2. Sau đó, trong dòng `(*)` của `rabbit.eat`, chúng ta muốn chuyển cuộc gọi thậm chí cao hơn trong chuỗi, nhưng `this=longEar`, vì vậy `this.__proto__.eat` lại là `rabbit.eat`!

    ```js
    // inside rabbit.eat() we also have this = longEar
    this.__proto__.eat.call(this) // (*)
    // becomes
    longEar.__proto__.eat.call(this)
    // or (again)
    rabbit.eat.call(this);
    ```

3. ...Vì vậy, `rabbit.eat` tự gọi mình trong vòng lặp vô tận, bởi vì nó không thể tăng thêm nữa.

Vấn đề không thể được giải quyết bằng cách sử dụng `this` một mình.

### `[[HomeObject]]`

Để cung cấp giải pháp, JavaScript thêm một thuộc tính nội bộ đặc biệt cho các hàm: `[[HomeObject]]`.

** Khi một hàm được chỉ định là một phương thức class hoặc phương thức đối tượng, thuộc tính `[[HomeObject]]` của nó trở thành đối tượng đó.**

Điều này thực sự vi phạm ý tưởng về các "unbound" functions, bởi vì các phương thức ghi nhớ các đối tượng của chúng. Và `[[HomeObject]]` không thể thay đổi, vì vậy ràng buộc này là mãi mãi. Vì vậy, đó là một thay đổi rất quan trọng trong ngôn ngữ.

Nhưng sự thay đổi này là an toàn. `[[HomeObject]]` chỉ được sử dụng để gọi các phương thức cha trong `super`, để giải quyết prototype. Vì vậy, nó không phá vỡ tính tương thích.

Hãy xem cách nó hoạt động cho `super` -- một lần nữa, bằng cách sử dụng các đối tượng đơn giản:

```js
let animal = {
  name: "Animal",
  eat() {         // [[HomeObject]] == animal
    alert(`${this.name} eats.`);
  }
};

let rabbit = {
  __proto__: animal,
  name: "Rabbit",
  eat() {         // [[HomeObject]] == rabbit
    super.eat();
  }
};

let longEar = {
  __proto__: rabbit,
  name: "Long Ear",
  eat() {         // [[HomeObject]] == longEar
    super.eat();
  }
};

longEar.eat();  // Long Ear eats.
```

Mọi phương thức đều nhớ đối tượng của nó trong thuộc tính `[[HomeObject]]` bên trong. Sau đó, `super` sử dụng nó để giải quyết parent prototype.

`[[HomeObject]]` được định nghĩa cho các phương thức được định nghĩa cả trong các classes và trong các đối tượng đơn giản (plain objects). Nhưng đối với các đối tượng, các phương thức phải được chỉ định chính xác theo cách đã cho: as `method()`, not as `"method: function()"`.

Trong ví dụ dưới đây, một cú pháp phi phương thức (non-method syntax) được sử dụng để so sánh. Thuộc tính `[[HomeObject]]` không được đặt và kế thừa không hoạt động:

```js
let animal = {
  eat: function() { // should be the short syntax: eat() {...}
    // ...
  }
};

let rabbit = {
  __proto__: animal,
  eat: function() {
    super.eat();
  }
};

rabbit.eat();  // Error calling super (because there's no [[HomeObject]])
```

## Static methods and inheritance

Cú pháp `class` cũng hỗ trợ kế thừa cho các thuộc tính tĩnh.

Ví dụ:

```js
class Animal {

  constructor(name, speed) {
    this.speed = speed;
    this.name = name;
  }

  run(speed = 0) {
    this.speed += speed;
    alert(`${this.name} runs with speed ${this.speed}.`);
  }

  static compare(animalA, animalB) {
    return animalA.speed - animalB.speed;
  }

}

// Inherit from Animal
class Rabbit extends Animal {
  hide() {
    alert(`${this.name} hides!`);
  }
}

let rabbits = [
  new Rabbit("White Rabbit", 10),
  new Rabbit("Black Rabbit", 5)
];

rabbits.sort(Rabbit.compare);

rabbits[0].run(); // Black Rabbit runs with speed 5.
```

Bây giờ chúng ta có thể gọi `Rabbit.compare` giả sử rằng `Animal.compare` được kế thừa sẽ được gọi.

Làm thế nào nó hoạt động? Một lần nữa, sử dụng nguyên mẫu (prototypes). Như bạn có thể đã đoán, phần mở rộng cũng cung cấp cho `Rabbit` các tham chiếu `[[Prototype]]` cho `Animal`.

![](animal-rabbit-static.png)

Vì vậy, `Rabbit` function bây giờ kế thừa từ `Animal` function. Và `Animal` function bình thường có `[[Prototype]]` tham chiếu `Function.prototype`, vì nó không `extend` bất cứ thứ gì.

Ở đây, hãy kiểm tra xem:

```js
class Animal {}
class Rabbit extends Animal {}

// for static propertites and methods
alert(Rabbit.__proto__ === Animal); // true

// and the next step is Function.prototype
alert(Animal.__proto__ === Function.prototype); // true

// that's in addition to the "normal" prototype chain for object methods
alert(Rabbit.prototype.__proto__ === Animal.prototype);
```

Theo cách này, `Rabbit` có quyền truy cập vào tất cả các phương thức tĩnh của `Animal`.

### Không có kế thừa tĩnh trong phần dựng sẵn (No static inheritance in built-ins)

Xin lưu ý rằng các built-in classes không có tham chiếu static `[[Prototype]]` như vậy. Chẳng hạn, `Object` có `Object.defineProperty`, `Object.keys` và v.v., nhưng `Array`, `Date` v.v. không kế thừa chúng.

Đây là hình ảnh cấu trúc (structure) cho `Date` và `Object`:

![](object-date-inheritance.png)

Lưu ý, không có liên kết giữa `Date` và `Object`. Cả `Object` và `Date` đều tồn tại độc lập. `Date.prototype` kế thừa từ `Object.prototype`, nhưng đó là tất cả.

Sự khác biệt như vậy tồn tại vì lý do lịch sử: không có suy nghĩ về cú pháp lớp và kế thừa các phương thức tĩnh vào buổi bình minh của ngôn ngữ JavaScript.

## Natives are extendable

Built-in classes như Array, Map và các classes khác cũng có thể mở rộng.

Chẳng hạn, ở đây `PowerArray` kế thừa từ `Array` gốc:

```js
// add one more method to it (can do more)
class PowerArray extends Array {
  isEmpty() {
    return this.length === 0;
  }
}

let arr = new PowerArray(1, 2, 5, 10, 50);
alert(arr.isEmpty()); // false

let filteredArr = arr.filter(item => item >= 10);
alert(filteredArr); // 10, 50
alert(filteredArr.isEmpty()); // false
```

Xin lưu ý một điều rất thú vị. Các phương thức dựng sẵn như `filter`, `map` và các phương thức khác -- trả về các đối tượng mới có chính xác kiểu được kế thừa. Chúng dựa vào thuộc tính `constructor` để làm như vậy.

Trong ví dụ trên,

```js
arr.constructor === PowerArray
```

Vì vậy, khi `arr.filter()` được gọi, bên trong nó tạo ra mảng kết quả mới chính xác như `new PowerArray`. Và chúng ta có thể tiếp tục sử dụng các phương thức của nó trong chuỗi tiếp theo.

Thậm chí hơn nữa, chúng ta có thể tùy chỉnh hành vi đó. The static getter `Symbol.species`, nếu tồn tại, trả về constructor để sử dụng trong các trường hợp như vậy.

Ví dụ, ở đây do các `Symbol.species` built-in methods như `map`, `filter` sẽ trả về các "normal" arrays:

```js
class PowerArray extends Array {
  isEmpty() {
    return this.length === 0;
  }

  // built-in methods will use this as the constructor
  static get [Symbol.species]() {
    return Array;
  }
}

let arr = new PowerArray(1, 2, 5, 10, 50);
alert(arr.isEmpty()); // false

// filter creates new array using arr.constructor[Symbol.species] as constructor
let filteredArr = arr.filter(item => item >= 10);

// filteredArr is not PowerArray, but Array
alert(filteredArr.isEmpty()); // Error: filteredArr.isEmpty is not a function
```

Chúng ta có thể sử dụng nó trong các khóa nâng cao hơn (advanced keys) để loại bỏ extended functionality khỏi các resulting values nếu không cần thiết. Hoặc, có thể, để mở rộng hơn nữa.
