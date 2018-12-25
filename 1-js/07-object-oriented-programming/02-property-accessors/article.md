
# Property getters and setters

Có hai kiểu thuộc tính.

Kiểu đầu tiên là *thuộc tính dữ liệu*. Chúng ta đã biết làm thế nào để làm việc với chúng. Trên thực tế, tất cả các thuộc tính mà chúng ta đang sử dụng cho đến nay là các thuộc tính dữ liệu.

Kiểu thuộc tính thứ hai là một cái gì đó mới. Đó là *accessor properties*. Chúng cơ bản là các hàm hoạt động trên việc getting và setting một giá trị, nhưng trông giống như các thuộc tính thông thường đối với một external code.

## Getters and setters

Accessor properties được biểu diễn bằng các phương thức "getter" và "setter". Trong một object literal, chúng được ký hiệu là `get` và `set`:

```js
let obj = {
  get propName() {
    // getter, the code executed on getting obj.propName
  },

  set propName(value) {
    // setter, the code executed on setting obj.propName = value
  }
};
```

The getter hoạt động khi `obj.propName` được đọc, the setter -- khi nó được gán.

Chẳng hạn, chúng ta có một đối tượng `user` với `name` và `surname`:

```js
let user = {
  name: "John",
  surname: "Smith"
};
```

Bây giờ chúng ta muốn thêm một thuộc tính "fullName", đó phải là "John Smith". Tất nhiên, chúng ta  không muốn sao copy-paste thông tin hiện có, vì vậy chúng ta có thể triển khai nó như một accessor (người truy cập):

```js
let user = {
  name: "John",
  surname: "Smith",

  get fullName() {
    return `${this.name} ${this.surname}`;
  }
};

alert(user.fullName); // John Smith
```

Từ bên ngoài, một accessor property trông giống như một thuộc tính thông thường. Đó là ý tưởng của các accessor properties. Chúng ta không *gọi* `user.fullName` là một hàm, chúng ta *đọc* nó bình thường là: the getter chạy phía sau hậu trường.

Cho đến bây giờ, `fullName` chỉ có một getter. Nếu chúng ta cố gắng gán `user.fullName=`, sẽ có một lỗi.

Hãy sửa nó bằng cách thêm một setter cho `user.fullName`:

```js
let user = {
  name: "John",
  surname: "Smith",

  get fullName() {
    return `${this.name} ${this.surname}`;
  },

  set fullName(value) {
    [this.name, this.surname] = value.split(" ");
  }
};

// set fullName is executed with the given value.
user.fullName = "Alice Cooper";

alert(user.name); // Alice
alert(user.surname); // Cooper
```

Bây giờ chúng ta có một "virtual" property. Nó có thể đọc và ghi được, nhưng thực tế không tồn tại.

<br>

> ---

**📌 Accessor properties chỉ có thể truy cập được với get/set**

Một thuộc tính có thể là một "data property" hoặc một "accessor property", nhưng không phải cả hai.

Khi một thuộc tính được định nghĩa bằng `get prop()` hoặc `set prop()`, đó là một accessor property. Vì vậy, phải có một getter để đọc nó, và phải là một setter nếu chúng ta muốn gán nó.

Đôi khi, bình thường chỉ có một setter hoặc chỉ một getter. Nhưng thuộc tính sẽ không thể đọc được hoặc ghi được trong trường hợp đó.

> ---

<br>

## Accessor descriptors

Các mô tả (descriptors) cho các accessor properties là khác nhau -- so với các thuộc tính dữ liệu.

Đối với các accessor properties, không có `value` và `writable`, mà thay vào đó là các hàm `get` và `set`.

Vì vậy, một accessor descriptor có thể có:

- **`get`** -- một hàm không có đối số, hoạt động khi một thuộc tính được đọc,
- **`set`** -- một hàm có một đối số, được gọi khi thuộc tính được đặt,
- **`enumerable`** -- giống như đối với các thuộc tính dữ liệu,
- **`configurable`** -- giống như đối với các thuộc tính dữ liệu.

Chẳng hạn, để tạo một accessor `fullName` với `defineProperty`, chúng ta có thể truyền một descriptor với `get` và `set`:

```js
let user = {
  name: "John",
  surname: "Smith"
};

Object.defineProperty(user, 'fullName', {
  get() {
    return `${this.name} ${this.surname}`;
  },

  set(value) {
    [this.name, this.surname] = value.split(" ");
  }
});

alert(user.fullName); // John Smith

for(let key in user) alert(key); // name, surname
```

Xin lưu ý một lần nữa rằng một thuộc tính có thể là một accessor hoặc một thuộc tính dữ liệu, không phải cả hai.

Nếu chúng ta cố gắng cung cấp cả `get` và `value` trong cùng một descriptor, sẽ có một lỗi:

```js
// Error: Invalid property descriptor.
Object.defineProperty({}, 'prop', {
  get() {
    return 1
  },

  value: 2
});
```

## Smarter getters/setters

Getters/setters có thể được sử dụng như các wrappers trên các giá trị thuộc tính "thực" để có thêm quyền kiểm soát chúng.

Chẳng hạn, nếu chúng ta muốn cấm các tên quá ngắn cho `user`, chúng ta có thể lưu trữ `name` trong một thuộc tính đặc biệt `_name`. Và lọc các phép gán trong setter:

```js
let user = {
  get name() {
    return this._name;
  },

  set name(value) {
    if (value.length < 4) {
      alert("Name is too short, need at least 4 characters");
      return;
    }
    this._name = value;
  }
};

user.name = "Pete";
alert(user.name); // Pete

user.name = ""; // Name is too short...
```

Về mặt kỹ thuật, mã bên ngoài vẫn có thể truy cập trực tiếp vào tên bằng cách sử dụng `user._name`. Nhưng có một thỏa thuận được biết đến rộng rãi rằng các thuộc tính bắt đầu bằng dấu gạch dưới `"_"` là nội bộ và không nên chạm vào từ bên ngoài đối tượng.


## Sử dụng để tương thích (Using for compatibility)

Một trong những ý tưởng tuyệt vời đằng sau getters và setters -- chúng cho phép kiểm soát một thuộc tính dữ liệu "bình thường" và điều chỉnh nó bất cứ lúc nào.

Chẳng hạn, chúng ta đã bắt đầu triển khai các user objects bằng cách sử dụng các thuộc tính dữ liệu `name` và `age`:

```js
function User(name, age) {
  this.name = name;
  this.age = age;
}

let john = new User("John", 25);

alert( john.age ); // 25
```

...Nhưng sớm hay muộn, mọi thứ có thể thay đổi. Thay vì `age`, chúng ta có thể quyết định lưu trữ `birthday`, vì nó chính xác và tiện lợi hơn:

```js
function User(name, birthday) {
  this.name = name;
  this.birthday = birthday;
}

let john = new User("John", new Date(1992, 6, 1));
```

Bây giờ phải làm gì với mã cũ vẫn sử dụng thuộc tính `age`?

Chúng ta có thể cố gắng tìm tất cả các địa điểm như vậy và sửa chúng, nhưng điều đó sẽ mất thời gian và khó thực hiện nếu mã đó được viết bởi người khác. Và bên cạnh đó, `age` là một điều tốt đẹp để có trong `user`, phải không? Ở một số nơi, đó chỉ là những gì chúng ta muốn.

Thêm một getter cho `age` giảm thiểu vấn đề:

```js
function User(name, birthday) {
  this.name = name;
  this.birthday = birthday;

  // age is calculated from the current date and birthday
  Object.defineProperty(this, "age", {
    get() {
      let todayYear = new Date().getFullYear();
      return todayYear - this.birthday.getFullYear();
    }
  });
}

let john = new User("John", new Date(1992, 6, 1));

alert( john.birthday ); // birthday is available
alert( john.age );      // ...as well as the age
```

Bây giờ mã cũ cũng hoạt động và chúng ta đã có một thuộc tính bổ sung tốt đẹp.
