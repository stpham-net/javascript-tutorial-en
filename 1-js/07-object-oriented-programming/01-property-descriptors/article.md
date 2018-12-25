
# Thuộc tính flags và các mô tả (descriptors)

Như chúng ta biết, các đối tượng có thể lưu trữ các thuộc tính.

Cho đến bây giờ, một thuộc tính là một cặp "key-value" đơn giản đối với chúng ta. Nhưng một thuộc tính đối tượng thực sự là một điều phức tạp và điều chỉnh hơn (tunable thing).

## Property flags

Các thuộc tính đối tượng, ngoài **`value`**, có ba thuộc tính đặc biệt (còn gọi là "flags"):

- **`writable`** -- nếu `true`, có thể thay đổi, nếu không nó chỉ đọc.
- **`enumerable`** -- nếu `true`, sau đó được liệt kê trong các vòng lặp, nếu không thì không được liệt kê.
- **`configurable`** -- nếu `true`, thuộc tính có thể bị xóa và các thuộc tính này có thể được sửa đổi, nếu không thì không.

Chúng ta chưa nhìn thấy chúng, vì nhìn chung chúng không xuất hiện. Khi chúng ta tạo một thuộc tính "theo cách thông thường", tất cả chúng đều là `true`. Nhưng chúng ta cũng có thể thay đổi chúng bất cứ lúc nào.

Trước tiên, hãy xem làm thế nào để có được những flags.

Phương thức [Object.getOwnPropertyDescriptor](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/getOwnPropertyDescriptor) cho phép truy vấn *đầy đủ* thông tin về một thuộc tính.

Cú pháp là:

```js
let descriptor = Object.getOwnPropertyDescriptor(obj, propertyName);
```

**`obj`**

Các đối tượng để có được thông tin từ.

**`propertyName`**

Tên của thuộc tính.

Giá trị được trả về là một đối tượng được gọi là "mô tả thuộc tính (property descriptor)": nó chứa giá trị và tất cả các cờ (flags).

Ví dụ:

```js
let user = {
  name: "John"
};

let descriptor = Object.getOwnPropertyDescriptor(user, 'name');

alert( JSON.stringify(descriptor, null, 2 ) );
/* property descriptor:
{
  "value": "John",
  "writable": true,
  "enumerable": true,
  "configurable": true
}
*/
```

Để thay đổi các cờ, chúng ta có thể sử dụng [Object.defineProperty](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/defineProperty).

Cú pháp là:

```js
Object.defineProperty(obj, propertyName, descriptor)
```

**`obj`, `propertyName`**

Các đối tượng và thuộc tính để làm việc trên.

**`descriptor`**

Property descriptor để áp dụng.

Nếu thuộc tính tồn tại, `defineProperty` cập nhật các cờ của nó. Còn không, nó tạo ra thuộc tính với các giá trị và cờ đã cho; trong trường hợp đó, nếu một cờ không được cung cấp, nó được giả sử là `false`.

Chẳng hạn, ở đây một thuộc tính `name` được tạo ra với tất cả các falsy flags:

```js
let user = {};

Object.defineProperty(user, "name", {
  value: "John"
});

let descriptor = Object.getOwnPropertyDescriptor(user, 'name');

alert( JSON.stringify(descriptor, null, 2 ) );
/*
{
  "value": "John",
  "writable": false,
  "enumerable": false,
  "configurable": false
}
*/
```

So sánh nó với "normally created" `user.name` ở trên: bây giờ tất cả các cờ đều là falsy. Nếu đó không phải là những gì chúng ta muốn thì tốt hơn chúng ta nên đặt chúng thành `true` trong `descriptor`.

Bây giờ hãy xem hiệu ứng của các cờ bằng ví dụ.

## Read-only

Hãy làm cho `user.name` chỉ đọc bằng cách thay đổi cờ `writable`:

```js
let user = {
  name: "John"
};

Object.defineProperty(user, "name", {
  writable: false
});

user.name = "Pete"; // Error: Cannot assign to read only property 'name'...
```

Bây giờ không ai có thể thay đổi tên người dùng của chúng ta, trừ khi họ áp dụng `defineProperty` của riêng họ để ghi đè tên của chúng ta.

Đây là hoạt động tương tự, nhưng đối với trường hợp khi một thuộc tính không tồn tại:

```js
let user = { };

Object.defineProperty(user, "name", {
  value: "Pete",
  // for new properties need to explicitly list what's true
  enumerable: true,
  configurable: true
});

alert(user.name); // Pete
user.name = "Alice"; // Error
```

## Không liệt kê (Non-enumerable)

Bây giờ, hãy thêm một `toString` tùy chỉnh vào `user`.

Thông thường, một built-in `toString` cho các đối tượng là không thể liệt kê được, nó không hiển thị trong `for..in`. Nhưng nếu chúng ta thêm `toString` của riêng mình, thì theo mặc định, nó sẽ hiển thị trong `for..in`, như thế này:

```js
let user = {
  name: "John",
  toString() {
    return this.name;
  }
};

// By default, both our properties are listed:
for (let key in user) alert(key); // name, toString
```

Nếu chúng ta không thích nó, thì chúng ta có thể đặt `enumerable:false`. Sau đó, nó sẽ không xuất hiện trong vòng lặp `for..in`, giống như built-in one:

```js
let user = {
  name: "John",
  toString() {
    return this.name;
  }
};

Object.defineProperty(user, "toString", {
  enumerable: false
});

// Now our toString disappears:
for (let key in user) alert(key); // name
```

Các thuộc tính không thể liệt kê được cũng được loại trừ khỏi `Object.keys`:

```js
alert(Object.keys(user)); // name
```

## Non-configurable

The non-configurable flag (`configurable:false`) đôi khi được đặt sẵn cho các built-in objects và properties.

Một thuộc tính non-configurable không thể bị xóa hoặc thay đổi với `defineProperty`.

Chẳng hạn, `Math.PI` là chỉ-đọc (read-only), không-liệt kê (non-enumerable) và không-cấu hình (non-configurable):

```js
let descriptor = Object.getOwnPropertyDescriptor(Math, 'PI');

alert( JSON.stringify(descriptor, null, 2 ) );
/*
{
  "value": 3.141592653589793,
  "writable": false,
  "enumerable": false,
  "configurable": false
}
*/
```

Vì vậy, một lập trình viên không thể thay đổi giá trị của `Math.PI` hoặc ghi đè lên nó.

```js
Math.PI = 3; // Error

// delete Math.PI won't work either
```

Làm cho một thuộc tính non-configurable là một con đường một chiều. Chúng ta không thể thay đổi nó trở lại, bởi vì `defineProperty` không hoạt động trên các thuộc tính non-configurable.

Ở đây chúng ta đang tạo ra `user.name` một hằng số "được niêm phong mãi mãi":

```js
let user = { };

Object.defineProperty(user, "name", {
  value: "John",
  writable: false,
  configurable: false
});

// won't be able to change user.name or its flags
// all this won't work:
//   user.name = "Pete"
//   delete user.name
//   defineProperty(user, "name", ...)
Object.defineProperty(user, "name", {writable: true}); // Error
```

<br>

> ---

**📌 Lỗi chỉ xuất hiện trong use strict**

Trong non-strict mode, không có lỗi xảy ra khi ghi vào các thuộc tính read-only và như vậy. Nhưng hoạt động vẫn không thành công. Các hành động vi phạm cờ chỉ âm thầm bị bỏ qua trong non-strict.

> ---

<br>

## Object.defineProperties

Có một phương thức [Object.defineProperties(obj, descriptors)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/defineProperties) cho phép xác định nhiều thuộc tính cùng một lúc.

Cú pháp là:

```js
Object.defineProperties(obj, {
  prop1: descriptor1,
  prop2: descriptor2
  // ...
});
```

Ví dụ:

```js
Object.defineProperties(user, {
  name: { value: "John", writable: false },
  surname: { value: "Smith", writable: false },
  // ...
});
```

Vì vậy, chúng ta có thể thiết lập nhiều thuộc tính cùng một lúc.

## Object.getOwnPropertyDescriptors

Để có được tất cả các mô tả thuộc tính (property descriptors) cùng một lúc, chúng ta có thể sử dụng phương thức [Object.getOwnPropertyDescriptors(obj)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/getOwnPropertyDescriptors).

Cùng với `Object.defineProperties`, nó có thể được sử dụng như một cách "nhận biết cờ (flags-aware)" để nhân bản một đối tượng:

```js
let clone = Object.defineProperties({}, Object.getOwnPropertyDescriptors(obj));
```

Thông thường khi chúng ta clone một đối tượng, chúng ta sử dụng một phép gán để sao chép các thuộc tính, như thế này:

```js
for (let key in user) {
  clone[key] = user[key]
}
```

...Nhưng điều đó không sao chép cờ. Vì vậy, nếu chúng ta muốn một bản sao "tốt hơn" thì `Object.defineProperties` được ưu tiên.

Một điểm khác biệt nữa là `for..in` bỏ qua các symbolic properties, nhưng `Object.getOwnPropertyDescriptors` trả về *tất cả* các property descriptors bao gồm cả các symbolic properties.

## Sealing (niêm phong) một object globally

Property descriptors làm việc ở cấp độ của các thuộc tính cá nhân.

Ngoài ra còn có các phương thức giới hạn quyền truy cập vào *toàn bộ* đối tượng:

**[Object.preventExtensions(obj)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/preventExtensions)**

Cấm thêm thuộc tính cho đối tượng.

**[Object.seal(obj)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/seal)**

Cấm thêm/xóa các thuộc tính, thiết lập cho tất cả các thuộc tính hiện có `configurable: false`.

**[Object.freeze(obj)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/freeze)**

Cấm thêm/xóa/thay đổi thuộc tính, thiết lập cho tất cả các thuộc tính hiện có `configurable: false, writable: false`.

Và cũng có những bài kiểm tra cho chúng:

**[Object.isExtensible(obj)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/isExtensible)**

Trả về `false` nếu việc thêm thuộc tính bị cấm, nếu không thì `true`.

**[Object.isSealed(obj)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/isSealed)**

Trả về `true` nếu việc thêm/xóa các thuộc tính bị cấm, và tất cả các thuộc tính hiện có đều có `configurable: false`.

**[Object.isFrozen(obj)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/isFrozen)**

Trả về `true` nếu việc thêm/xóa/thay đổi thuộc tính bị cấm và tất cả các thuộc tính hiện tại là `configurable: false, writable: false`.

Những phương thức này hiếm khi được sử dụng trong thực tế.
