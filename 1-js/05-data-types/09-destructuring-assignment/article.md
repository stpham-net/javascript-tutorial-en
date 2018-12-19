# Destructuring assignment

Hai cấu trúc dữ liệu được sử dụng nhiều nhất trong JavaScript là `Object` và `Array`.

Các đối tượng cho phép chúng ta đóng gói nhiều mẩu thông tin vào một thực thể duy nhất và các mảng cho phép chúng ta lưu trữ các bộ sưu tập theo thứ tự. Vì vậy, chúng ta có thể tạo một đối tượng hoặc một mảng và xử lý nó như một thực thể duy nhất hoặc có thể chuyển nó tới một lệnh gọi hàm.

*Destructuring assignment* là một cú pháp đặc biệt cho phép chúng ta "giải nén" các mảng hoặc đối tượng thành một loạt các biến, vì đôi khi chúng thuận tiện hơn. Phá hủy cấu trúc (destructuring) cũng hoạt động rất tốt với các hàm phức tạp có nhiều tham số, giá trị mặc định và chúng ta sẽ sớm thấy chúng được xử lý như thế nào.

## Array destructuring

Một ví dụ về cách mảng bị phá hủy (destructured) thành các biến:

```js
      // we have an array with the name and surname
      let arr = ["Ilya", "Kantor"]

      // destructuring assignment
      let [firstName, surname] = arr;

      alert(firstName); // Ilya
      alert(surname);  // Kantor
```

Bây giờ chúng ta có thể làm việc với các biến thay vì các thành viên mảng.

Nó trông tuyệt vời khi được kết hợp với `split` hoặc các phương thức trả về mảng khác:

```js
      let [firstName, surname] = "Ilya Kantor".split(' ');
```

<br>

> ---

**📌 "Phá hủy (Destructuring)" không có nghĩa là "phá hoại (destructive)".**

Nó được gọi là "destructuring assignment", bởi vì nó "phá hủy" bằng cách sao chép các items vào các biến. Nhưng bản thân mảng không bị sửa đổi.

Đó chỉ là một cách viết ngắn hơn:

```js
      // let [firstName, surname] = arr;
      let firstName = arr[0];
      let surname = arr[1];
```

> ---

<br>
<br>

> ---

**📌 Bỏ qua các phần tử đầu tiên**

Các phần tử không mong muốn của mảng cũng có thể được loại bỏ thông qua dấu phẩy thừa:

```js
      // first and second elements are not needed
      let [, , title] = ["Julius", "Caesar", "Consul", "of the Roman Republic"];

      alert( title ); // Consul
```

Trong đoạn mã trên, mặc dù các phần tử thứ nhất và thứ hai của mảng bị bỏ qua, phần tử thứ ba được gán cho `title` và phần còn lại cũng bị bỏ qua.

> ---

<br>
<br>

> ---

**📌 Hoạt động với bất kỳ iterable nào ở phía bên phải**

...Trên thực tế, chúng ta có thể sử dụng nó với bất kỳ iterable, không chỉ các mảng:

```js
      let [a, b, c] = "abc"; // ["a", "b", "c"]
      let [one, two, three] = new Set([1, 2, 3]);
```

> ---

<br>
<br>

> ---

**📌 Gán vào bất cứ thứ gì ở phía bên trái**

Chúng ta có thể sử dụng bất kỳ "chuyển nhượng (assignables)" ở bên trái.

Ví dụ, một thuộc tính đối tượng:

```js
      let user = {};
      [user.name, user.surname] = "Ilya Kantor".split(' ');

      alert(user.name); // Ilya
```

> ---

<br>
<br>

> ---

**📌 Looping with .entries()**

Trong chương trước, chúng ta đã thấy phương thức [Object.entries(obj)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/entries).

Chúng ta có thể sử dụng nó với việc destructuring để lặp qua keys-and-values của một object:

```js
      let user = {
        name: "John",
        age: 30
      };

      // loop over keys-and-values
      for (let [key, value] of Object.entries(user)) {
        alert(`${key}:${value}`); // name:John, then age:30
      }
```

...Và tương tự cho một map:

```js
      let user = new Map();
      user.set("name", "John");
      user.set("age", "30");

      for (let [key, value] of user.entries()) {
        alert(`${key}:${value}`); // name:John, then age:30
      }
```

> ---

<br>

### Phần còn lại (the rest) '...'

Nếu chúng ta muốn không chỉ nhận được các giá trị đầu tiên, mà còn thu thập tất cả các giá trị tiếp theo -- chúng ta có thể thêm một tham số nữa là "phần còn lại (the rest)" bằng ba dấu chấm `"..."`:

```js
      let [name1, name2, ...rest] = ["Julius", "Caesar", "Consul", "of the Roman Republic"];

      alert(name1); // Julius
      alert(name2); // Caesar

      alert(rest[0]); // Consul
      alert(rest[1]); // of the Roman Republic
      alert(rest.length); // 2
```

Giá trị của `rest` là mảng của các phần tử mảng còn lại. Chúng ta có thể sử dụng bất kỳ tên biến nào khác thay cho `rest`, chỉ cần đảm bảo rằng nó có ba dấu chấm trước nó và nằm cuối cùng trong destructuring assignment.

### Default values

Nếu có ít giá trị trong mảng hơn các biến trong phép gán, sẽ không có lỗi. Giá trị vắng mặt được coi là undefined:

```js
      let [firstName, surname] = [];

      alert(firstName); // undefined
```

Nếu chúng ta muốn một giá trị "mặc định" để thay thế giá trị còn thiếu, chúng ta có thể cung cấp nó bằng cách sử dụng `=`:

```js
      // default values
      let [name = "Guest", surname = "Anonymous"] = ["Julius"];

      alert(name);    // Julius (from array)
      alert(surname); // Anonymous (default used)
```

Giá trị mặc định có thể là các biểu thức phức tạp hơn hoặc thậm chí các lệnh gọi hàm. Chúng chỉ được đánh giá nếu giá trị không được cung cấp.

Chẳng hạn, ở đây chúng ta sử dụng hàm `prompt` cho hai giá trị mặc định. Nhưng nó sẽ chỉ chạy cho giá trị còn thiếu:

```js
      // runs only prompt for surname
      let [name = prompt('name?'), surname = prompt('surname?')] = ["Julius"];

      alert(name);    // Julius (from array)
      alert(surname); // whatever prompt gets
```



## Object destructuring

The destructuring assignment cũng hoạt động với các đối tượng.

Cú pháp cơ bản là:

```js
      let {var1, var2} = {var1:…, var2…}
```

Chúng ta có một đối tượng hiện có ở phía bên phải, mà chúng ta muốn chia thành các biến. Phía bên trái chứa một "mẫu (pattern)" cho các thuộc tính tương ứng. Trong trường hợp đơn giản, đó là danh sách các tên biến trong `{...}`.

Ví dụ:

```js
      let options = {
        title: "Menu",
        width: 100,
        height: 200
      };

      let {title, width, height} = options;

      alert(title);  // Menu
      alert(width);  // 100
      alert(height); // 200
```

Các thuộc tính `options.title`, `options.width` và `options.height` được gán cho các biến tương ứng. Thứ tự không quan trọng. Điều này cũng hoạt động:

```js
      // changed the order of properties in let {...}
      let {height, width, title} = { title: "Menu", height: 200, width: 100 }
```

Mẫu ở phía bên trái có thể phức tạp hơn và chỉ định ánh xạ giữa các thuộc tính và biến.

Ví dụ, nếu chúng ta muốn gán một thuộc tính cho một biến bằng một tên khác, `options.width` để truyền vào biến có tên `w`, thì chúng ta có thể đặt nó bằng dấu hai chấm:

```js
      let options = {
        title: "Menu",
        width: 100,
        height: 200
      };

      // { sourceProperty: targetVariable }
      let {width: w, height: h, title} = options;

      // width -> w
      // height -> h
      // title -> title

      alert(title);  // Menu
      alert(w);      // 100
      alert(h);      // 200
```

Dấu hai chấm hiển thị "what : goes where". Trong ví dụ trên thuộc tính `width` truyền tới `w`, thuộc tính `height` truyền tới `h` và `title` được gán cho cùng tên.

Đối với các thuộc tính có khả năng bị thiếu, chúng ta có thể đặt các giá trị mặc định bằng cách sử dụng `"="`, như thế này:

```js
      let options = {
        title: "Menu"
      };

      let {width = 100, height = 200, title} = options;

      alert(title);  // Menu
      alert(width);  // 100
      alert(height); // 200
```

Giống như với các mảng hoặc tham số hàm, các giá trị mặc định có thể là bất kỳ biểu thức hoặc thậm chí các lệnh gọi hàm. Họ sẽ được đánh giá nếu giá trị không được cung cấp.

Mã dưới đây yêu cầu chiều rộng, nhưng không phải tiêu đề.

```js
      let options = {
        title: "Menu"
      };

      let {width = prompt("width?"), title = prompt("title?")} = options;

      alert(title);  // Menu
      alert(width);  // (whatever you the result of prompt is)
```

Chúng ta cũng có thể kết hợp cả dấu hai chấm và đẳng thức:

```js
      let options = {
        title: "Menu"
      };

      let {width: w = 100, height: h = 200, title} = options;

      alert(title);  // Menu
      alert(w);      // 100
      alert(h);      // 200
```

### Toán tử còn lại (The rest operator)

Điều gì xảy ra nếu đối tượng có nhiều thuộc tính hơn chúng ta có các biến? Chúng ta có thể lấy một số và sau đó chỉ định "phần còn lại" ở đâu đó không?

Thông số kỹ thuật cho việc sử dụng toán tử còn lại (ba dấu chấm) ở đây gần như nằm trong tiêu chuẩn, nhưng hầu hết các trình duyệt chưa hỗ trợ.

Nó trông như thế này:

```js
      let options = {
        title: "Menu",
        height: 200,
        width: 100
      };

      let {title, ...rest} = options;

      // now title="Menu", rest={height: 200, width: 100}
      alert(rest.height);  // 200
      alert(rest.width);   // 100
```

<br>

> ---

**📌 Gotcha without `let`**

Trong các ví dụ trên, các biến đã được khai báo ngay trước khi gán: `let {…} = {…}`. Tất nhiên, chúng ta cũng có thể sử dụng các biến hiện có. Nhưng có một nhược điểm.

Điều này sẽ không hoạt động:

```js
      let title, width, height;

      // error in this line
      {title, width, height} = {title: "Menu", width: 200, height: 100};
```

Vấn đề là JavaScript xử lý `{...}` trong luồng mã chính (không phải bên trong một biểu thức khác) dưới dạng một khối mã. Các khối mã như vậy có thể được sử dụng để nhóm các câu lệnh, như thế này:

```js
      {
        // a code block
        let message = "Hello";
        // ...
        alert( message );
      }
```

Để hiển thị cho JavaScript biết rằng đó không phải là một khối mã, chúng ta có thể gói toàn bộ phép gán trong ngoặc `(...)`:

```js
      let title, width, height;

      // okay now
      ({title, width, height} = {title: "Menu", width: 200, height: 100});

      alert( title ); // Menu
```

> ---

<br>

## Nested destructuring

Nếu một đối tượng hoặc một mảng chứa các đối tượng và mảng khác, chúng ta có thể sử dụng các mẫu bên trái phức tạp hơn để trích xuất các phần sâu hơn.

Trong đoạn mã dưới đây `options` có một đối tượng khác trong thuộc tính `size` và một mảng trong thuộc tính `items`. Mẫu ở bên trái của phép gán có cùng cấu trúc:

```js
      let options = {
        size: {
          width: 100,
          height: 200
        },
        items: ["Cake", "Donut"],
        extra: true    // something extra that we will not destruct
      };

      // destructuring assignment on multiple lines for clarity
      let {
        size: { // put size here
          width,
          height
        },
        items: [item1, item2], // assign items here
        title = "Menu" // not present in the object (default value is used)
      } = options;

      alert(title);  // Menu
      alert(width);  // 100
      alert(height); // 200
      alert(item1);  // Cake
      alert(item2);  // Donut
```

Toàn bộ đối tượng `options` ngoại trừ `extra` không được đề cập, được gán cho các biến tương ứng.

![](destructuring-complex.png)

Cuối cùng, chúng ta có `width`, `height`, `item1`, `item2` và `title` từ giá trị mặc định.

Điều đó thường xảy ra với các destructuring assignments. Chúng ta có một đối tượng phức tạp với nhiều thuộc tính và chỉ muốn trích xuất những gì chúng ta cần.

Ngay cả ở đây nó xảy ra:

```js
      // take size as a whole into a variable, ignore the rest
      let { size } = options;
```

## Smart function parameters

Đôi khi một hàm có thể có nhiều tham số, hầu hết trong số đó là tùy chọn. Điều đó đặc biệt đúng với giao diện người dùng. Hãy tưởng tượng một function tạo ra một menu. Nó có thể có chiều rộng, chiều cao, tiêu đề, items list, v.v.

Đây là một cách tồi để viết function như vậy:

```js
      function showMenu(title = "Untitled", width = 200, height = 100, items = []) {
        // ...
      }
```

Trong cuộc sống thực, vấn đề là làm thế nào để nhớ thứ tự lập luận. Thông thường các IDE cố gắng giúp chúng ta, đặc biệt nếu mã được ghi chép tốt, nhưng vẫn ... Một vấn đề khác là làm thế nào để gọi một hàm khi hầu hết các tham số đều được mặc định.

Như thế này?

```js
      showMenu("My Menu", undefined, undefined, ["Item1", "Item2"])
```

Thật là xấu xí. Và trở nên không thể đọc được khi chúng ta xử lý nhiều tham số hơn.

Destructuring đến để giải cứu!

Chúng ta có thể truyền tham số dưới dạng một đối tượng và hàm ngay lập tức phá hủy chúng thành các biến:

```js
      // we pass object to function
      let options = {
        title: "My menu",
        items: ["Item1", "Item2"]
      };

      // ...and it immediately expands it to variables
      function showMenu({title = "Untitled", width = 200, height = 100, items = []}) {
        // title, items – taken from options,
        // width, height – defaults used
        alert( `${title} ${width} ${height}` ); // My Menu 200 100
        alert( items ); // Item1, Item2
      }

      showMenu(options);
```

Chúng ta cũng có thể sử dụng destructuring phức tạp hơn với các đối tượng lồng nhau (nested objects) và dấu hai chấm ánh xạ (colon mappings):

```js
      let options = {
        title: "My menu",
        items: ["Item1", "Item2"]
      };

      function showMenu({
        title = "Untitled",
        width: w = 100,  // width goes to w
        height: h = 200, // height goes to h
        items: [item1, item2] // items first element goes to item1, second to item2
      }) {
        alert( `${title} ${w} ${h}` ); // My Menu 100 200
        alert( item1 ); // Item1
        alert( item2 ); // Item2
      }

      showMenu(options);
```

Cú pháp tương tự như đối với một destructuring assignment:

```js
      function({
        incomingProperty: parameterName = defaultValue
        ...
      })
```

Xin lưu ý rằng việc destructuring như vậy giả sử rằng `showMenu()` không có đối số. Nếu chúng ta muốn tất cả các giá trị theo mặc định, thì chúng ta nên chỉ định một đối tượng trống:

```js
      showMenu({});

      showMenu(); // this would give an error
```

Chúng ta có thể khắc phục điều này bằng cách tạo `{}` giá trị mặc định cho toàn bộ destructuring:


```js
      // simplified parameters a bit for clarity
      function showMenu({ title = "Menu", width = 100, height = 200 } = {}) {
        alert( `${title} ${width} ${height}` );
      }

      showMenu(); // Menu 100 200
```

Trong đoạn mã trên, toàn bộ đối tượng là `{}` theo mặc định, do đó luôn có thứ gì đó để destructurize.

## Tóm lược

- Destructuring assignment cho phép ánh xạ ngay lập tức một đối tượng hoặc mảng lên nhiều biến.
- Cú pháp đối tượng:

    ```js
    let {prop : varName = default, ...} = object
    ```

    Điều này có nghĩa là thuộc tính `prop` nên truyền vào biến `varName` và, nếu không có thuộc tính đó tồn tại, thì nên sử dụng giá trị `default`.

- Cú pháp mảng:

    ```js
    let [item1 = default, item2, ...rest] = array
    ```

    Mục đầu tiên truyền đến `item1`; cái thứ hai truyền vào `item2`, tất cả phần còn lại tạo thành mảng `rest`.

- Đối với các trường hợp phức tạp hơn, bên trái phải có cấu trúc tương tự như bên phải.
