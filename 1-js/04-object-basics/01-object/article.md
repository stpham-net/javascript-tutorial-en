
# Các đối tượng (Objects)

Như chúng ta đã biết từ chương **kiểu (types)**, có bảy kiểu dữ liệu trong JavaScript. Sáu trong số chúng được gọi là "nguyên thủy", bởi vì các giá trị của chúng chỉ chứa một thứ duy nhất (có thể là một chuỗi hoặc một số hoặc bất cứ thứ gì).

Ngược lại, các đối tượng được sử dụng để lưu trữ các bộ sưu tập có khóa của các dữ liệu khác nhau và các thực thể phức tạp hơn. Trong JavaScript, các đối tượng thâm nhập vào hầu hết mọi khía cạnh của ngôn ngữ. Vì vậy, chúng ta phải hiểu chúng trước khi đi sâu vào bất cứ nơi nào khác.

Một đối tượng có thể được tạo bằng dấu ngoặc hình `{…}` với một danh sách tùy chọn *thuộc tính (properties)*. Một thuộc tính là một cặp "key: value", trong đó `key` là một chuỗi (còn được gọi là "tên thuộc tính (property name)") và `value` có thể là bất cứ thứ gì.

Chúng ta có thể tưởng tượng một đối tượng như một cái tủ với các tập tin đã ký. Mỗi phần dữ liệu được lưu trữ trong tệp của nó bằng khóa. Thật dễ dàng để tìm một tệp theo tên của nó hoặc add/remove một tệp.

![](object.png)

Một đối tượng trống ("tủ trống") có thể được tạo bằng một trong hai cú pháp:

```js
      let user = new Object(); // "object constructor" syntax
      let user = {};  // "object literal" syntax
```

![](object-user-blank.png)

Thông thường, các dấu ngoặc `{...}` được sử dụng. Tuyên bố đó được gọi là *object literal*.

## Literals và properties

Chúng ta có thể ngay lập tức đặt một số thuộc tính vào các cặp `{...}` dưới dạng "key: value":

```js
      let user = {     // an object
        name: "John",  // by key "name" store value "John"
        age: 30        // by key "age" store value 30
      };
```

Một thuộc tính có một khóa (còn được gọi là "tên" hoặc "định danh") trước dấu hai chấm `":"` và một giá trị ở bên phải của nó.

Trong đối tượng `user`, có hai thuộc tính:

1. Thuộc tính đầu tiên có tên `"name"` và giá trị `"John"`.
2. Cái thứ hai có tên `"age"` và giá trị `30`.

Kết quả là đối tượng `user` có thể được tưởng tượng như một cái tủ với hai tệp được ký có nhãn "name" và "age".

![user object](object-user.png)

Chúng ta có thể thêm, xóa và đọc các tập tin từ nó bất cứ lúc nào.

Các giá trị thuộc tính có thể truy cập bằng cách sử dụng ký hiệu chấm:

```js
      // get fields of the object:
      alert( user.name ); // John
      alert( user.age ); // 30
```

Giá trị có thể là bất kỳ loại nào. Hãy thêm một boolean:

```js
      user.isAdmin = true;
```

![user object 2](object-user-isadmin.png)

Để xóa một thuộc tính, chúng ta có thể sử dụng toán tử `delete`:

```js
      delete user.age;
```

![user object 3](object-user-delete.png)

Chúng ta cũng có thể sử dụng tên thuộc tính đa từ, nhưng sau đó chúng phải được trích dẫn (quoted):

```js
      let user = {
        name: "John",
        age: 30,
        "likes birds": true  // multiword property name must be quoted
      };
```

![](object-user-props.png)


Thuộc tính cuối cùng trong danh sách có thể kết thúc bằng dấu phẩy:

```js
      let user = {
        name: "John",
        age: 30,
      }
```
Đó được gọi là dấu phẩy "trailing (kế tiếp)" hoặc "hanging (treo)". Làm cho nó dễ dàng hơn để thêm/xóa/di chuyển xung quanh các thuộc tính, bởi vì tất cả các dòng trở nên giống nhau.

## Dấu ngoặc vuông (Square brackets)

Đối với các thuộc tính đa từ, truy cập dấu chấm không hoạt động:

```js
      // this would give a syntax error
      user.likes birds = true
```

Đó là bởi vì dấu chấm yêu cầu khóa phải là định danh biến hợp lệ. Đó là: không có khoảng trống và các hạn chế khác.

Có một "ký hiệu ngoặc vuông" thay thế hoạt động với bất kỳ chuỗi nào:


```js
      let user = {};

      // set
      user["likes birds"] = true;

      // get
      alert(user["likes birds"]); // true

      // delete
      delete user["likes birds"];
```

Bây giờ mọi thứ đều ổn. Xin lưu ý rằng chuỗi bên trong ngoặc là trích dẫn chính xác (bất kỳ kiểu nào của trích dẫn nào cũng vậy).

Dấu ngoặc vuông cũng cung cấp một cách để có được tên thuộc tính là kết quả của bất kỳ biểu thức nào -- trái ngược với một chuỗi ký tự -- như từ một biến như sau:

```js
      let key = "likes birds";

      // same as user["likes birds"] = true;
      user[key] = true;
```

Ở đây, biến `key` có thể được tính tại run-time hoặc phụ thuộc vào đầu vào của người dùng. Và sau đó chúng ta sử dụng nó để truy cập vào thuộc tính. Điều đó cho chúng ta rất nhiều tính linh hoạt. Ký hiệu chấm không thể được sử dụng theo cách tương tự.

Ví dụ:

```js
      let user = {
        name: "John",
        age: 30
      };

      let key = prompt("What do you want to know about the user?", "name");

      // access by variable
      alert( user[key] ); // John (if enter "name")
```

### Thuộc tính được tính (Computed properties)

Chúng ta có thể sử dụng dấu ngoặc vuông trong một object literal. Đó gọi là *computed properties*.

Ví dụ:

```js
      let fruit = prompt("Which fruit to buy?", "apple");

      let bag = {
        [fruit]: 5, // the name of the property is taken from the variable fruit
      };

      alert( bag.apple ); // 5 if fruit="apple"
```

Ý nghĩa của một computed property rất đơn giản: `[fruit]` có nghĩa là tên thuộc tính nên được lấy từ `fruit`.

Vì vậy, nếu một khách nhập vào `"apple"`, `bag` sẽ trở thành `{apple: 5}`.

Về cơ bản, nó hoạt động giống như:

```js
      let fruit = prompt("Which fruit to buy?", "apple");
      let bag = {};

      // take property name from the fruit variable
      bag[fruit] = 5;
```

...Nhưng trông đẹp hơn.

Chúng ta có thể sử dụng các biểu thức phức tạp hơn trong dấu ngoặc vuông:

```js
      let fruit = 'apple';
      let bag = {
        [fruit + 'Computers']: 5 // bag.appleComputers = 5
      };
```

Dấu ngoặc vuông mạnh hơn nhiều so với ký hiệu dấu chấm. Chúng cho phép bất kỳ tên thuộc tính và các biến. Nhưng chúng cũng cồng kềnh hơn để viết.

Vì vậy, hầu hết thời gian, khi tên thuộc tính được biết và đơn giản, dấu chấm được sử dụng. Và nếu chúng ta cần một cái gì đó phức tạp hơn, thì chúng ta chuyển sang dấu ngoặc vuông.

<br>

> ---

**📌 Các từ dành riêng được phép làm tên thuộc tính**

Một biến không thể có tên bằng một trong những từ dành riêng cho ngôn ngữ như "for", "let", "return", v.v.

Nhưng đối với một thuộc tính đối tượng, không có hạn chế đó. Tên nào cũng được:

```js
      let obj = {
        for: 1,
        let: 2,
        return: 3
      };

      alert( obj.for + obj.let + obj.return );  // 6
```

Về cơ bản, bất kỳ tên nào cũng được cho phép, nhưng có một tên đặc biệt: `"__proto __"` được đối xử đặc biệt vì lý do lịch sử. Chẳng hạn, chúng ta không thể đặt nó thành một giá trị phi đối tượng:

```js
      let obj = {};
      obj.__proto__ = 5;
      alert(obj.__proto__); // [object Object], didn't work as intended
```

Như chúng ta thấy từ mã, việc gán cho một `5` nguyên thủy bị bỏ qua.

Điều đó có thể trở thành một nguồn của các lỗi và thậm chí là lỗ hổng nếu chúng ta dự định lưu trữ các cặp khóa-giá trị tùy ý trong một đối tượng và cho phép khách truy cập chỉ định các khóa.

Trong trường hợp đó, khách truy cập có thể chọn "__proto__" làm khóa và logic gán sẽ bị hủy (như được hiển thị ở trên).

Có một cách để làm cho các đối tượng coi `__proto__` như một thuộc tính thông thường, chúng ta sẽ đề cập sau, nhưng trước tiên chúng ta cần biết thêm về các đối tượng.

Ngoài ra còn có một cấu trúc dữ liệu khác **Map**, mà chúng ta sẽ tìm hiểu trong chương **map-set-weakmap-weakset**, hỗ trợ các khóa tùy ý.

> ---

<br>

## Viết nhanh giá trị thuộc tính (Property value shorthand)

Trong mã thực, chúng ta thường sử dụng các biến hiện có làm giá trị cho tên thuộc tính.

Ví dụ:

```js
      function makeUser(name, age) {
        return {
          name: name,
          age: age
          // ...other properties
        };
      }

      let user = makeUser("John", 30);
      alert(user.name); // John
```

Trong ví dụ trên, các thuộc tính có cùng tên với các biến. Trường hợp sử dụng để tạo một thuộc tính từ một biến rất phổ biến, có một *property value shorthand* đặc biệt để làm cho nó ngắn hơn.

Thay vì `name:name`, chúng ta chỉ cần viết `name`, như thế này:

```js
      function makeUser(name, age) {
        return {
          name, // same as name: name
          age   // same as age: age
          // ...
        };
      }
```

Chúng ta có thể sử dụng cả thuộc tính bình thường và tốc ký trong cùng một đối tượng:

```js
      let user = {
        name,  // same as name:name
        age: 30
      };
```

## Kiểm tra sự tồn tại

Một tính năng đáng chú ý của object là có thể truy cập bất kỳ thuộc tính nào. Sẽ không có lỗi nếu thuộc tính không tồn tại! Truy cập một thuộc tính không tồn tại chỉ trả về `undefined`. Nó cung cấp một cách rất phổ biến để kiểm tra xem thuộc tính có tồn tại hay không -- để lấy nó và so sánh với không xác định:

```js
      let user = {};

      alert( user.noSuchProperty === undefined ); // true means "no such property"
```

Ngoài ra còn tồn tại một toán tử đặc biệt `"in"` để kiểm tra sự tồn tại của một thuộc tính.

Cú pháp là:

```js
      "key" in object
```

Ví dụ:

```js
      let user = { name: "John", age: 30 };

      alert( "age" in user ); // true, user.age exists
      alert( "blabla" in user ); // false, user.blabla doesn't exist
```

Xin lưu ý rằng ở phía bên trái của `in` phải có *tên thuộc tính*. Đó thường là một chuỗi trích dẫn.

Nếu chúng ta bỏ qua dấu ngoặc kép, điều đó có nghĩa là một biến chứa tên sẽ được kiểm tra. Ví dụ:

```js
      let user = { age: 30 };
	  
      let key = "age";
      alert( key in user ); // true, takes the name from key and checks for such property
```

<br>

> ---

**📌 Sử dụng "in" cho các thuộc tính lưu trữ `undefined`**

Thông thường, so sánh nghiêm ngặt `"=== undefined"` kiểm tra hoạt động tốt. Nhưng có một trường hợp đặc biệt khi nó thất bại, nhưng `"in"` hoạt động chính xác.

Đó là khi một thuộc tính đối tượng tồn tại, nhưng lưu trữ `undefined`:

```js
      let obj = {
        test: undefined
      };

      alert( obj.test ); // it's undefined, so - no such property?

      alert( "test" in obj ); // true, the property does exist!
```


Trong đoạn mã trên, thuộc tính `obj.test` về mặt kỹ thuật tồn tại. Vì vậy, toán tử `in` hoạt động đúng.

Các tình huống như thế này rất hiếm khi xảy ra, vì `undefined` thường không được chỉ định. Chúng tôi chủ yếu sử dụng `null` cho các giá trị "unknown" hoặc "empty". Vì vậy, toán tử `in` là một vị khách kỳ lạ trong mã.

> ---

<br>

## Vòng lặp "for..in"

Để đi qua tất cả các khóa của một đối tượng, tồn tại một dạng vòng lặp đặc biệt: `for..in`. Đây là một điều hoàn toàn khác với cấu trúc `for (;;)` mà chúng ta đã nghiên cứu trước đây.

Cú pháp:

```js
      for(key in object) {
        // executes the body for each key among object properties
      }
```

Chẳng hạn, hãy xuất tất cả các thuộc tính của `user`:

```js
      let user = {
        name: "John",
        age: 30,
        isAdmin: true
      };

      for(let key in user) {
        // keys
        alert( key );  // name, age, isAdmin
        // values for the keys
        alert( user[key] ); // John, 30, true
      }
```

Lưu ý rằng tất cả các cấu trúc "for" cho phép chúng ta khai báo biến vòng lặp bên trong vòng lặp, như `let key` ở đây.

Ngoài ra, chúng ta có thể sử dụng một tên biến khác ở đây thay vì `key`. Chẳng hạn, `"for(let prop in obj)"` cũng được sử dụng rộng rãi.


### Sắp xếp như một đối tượng (Ordered like an object)

Các đối tượng được sắp xếp? Nói cách khác, nếu chúng ta lặp qua một đối tượng, chúng ta có nhận được tất cả các thuộc tính theo cùng thứ tự chúng đã được thêm không? Chúng ta có thể dựa vào điều này không?

Câu trả lời ngắn gọn là: "được sắp xếp theo kiểu đặc biệt": các thuộc tính số nguyên được sắp xếp, các thuộc tính khác xuất hiện theo thứ tự tạo. Các chi tiết theo sau.

Ví dụ: hãy xem xét một đối tượng có mã điện thoại:

```js
      let codes = {
        "49": "Germany",
        "41": "Switzerland",
        "44": "Great Britain",
        // ..,
        "1": "USA"
      };

      for(let code in codes) {
        alert(code); // 1, 41, 44, 49
      }
```

Đối tượng có thể được sử dụng để đề xuất một danh sách các tùy chọn cho người dùng. Nếu chúng ta tạo một trang chủ yếu cho khán giả Đức thì có lẽ chúng ta muốn `49` là người đầu tiên.

Nhưng nếu chúng ta chạy mã, chúng ta sẽ thấy một bức tranh hoàn toàn khác:

- Hoa Kỳ (1) trước
- sau đó Thụy Sĩ (41) và như vậy.

Các mã điện thoại đi theo thứ tự tăng dần, bởi vì chúng là số nguyên. Vì vậy, chúng ta thấy `1, 41, 44, 49`.

<br>

> ---

**📌 Thuộc tính số nguyên? Cái gì vậy?**

Thuật ngữ "thuộc tính số nguyên" ở đây có nghĩa là một chuỗi có thể được chuyển đổi thành và từ một số nguyên mà không thay đổi.

Vì vậy, "49" là tên thuộc tính số nguyên, bởi vì khi nó được chuyển đổi thành số nguyên và trở lại, nó vẫn giống nhau. Nhưng "+49" và "1.2" thì không:

```js
      // Math.trunc is a built-in function that removes the decimal part
      alert( String(Math.trunc(Number("49"))) ); // "49", same, integer property
      alert( String(Math.trunc(Number("+49"))) ); // "49", not same "+49" ⇒ not integer property
      alert( String(Math.trunc(Number("1.2"))) ); // "1", not same "1.2" ⇒ not integer property
```

> ---

<br>

...Mặt khác, nếu các khóa không phải là số nguyên, thì chúng được liệt kê theo thứ tự tạo, chẳng hạn:

```js
      let user = {
        name: "John",
        surname: "Smith"
      };
      user.age = 25; // add one more

      // non-integer properties are listed in the creation order
      for (let prop in user) {
        alert( prop ); // name, surname, age
      }
```

Vì vậy, để khắc phục sự cố với mã điện thoại, chúng ta có thể "gian lận" bằng cách làm cho mã không phải là số nguyên. Thêm dấu cộng `"+"` trước mỗi mã là đủ.

Như thế này:

```js
      let codes = {
        "+49": "Germany",
        "+41": "Switzerland",
        "+44": "Great Britain",
        // ..,
        "+1": "USA"
      };

      for(let code in codes) {
        alert( +code ); // 49, 41, 44, 1
      }
```

Bây giờ nó hoạt động như dự định.

## Sao chép bằng tham chiếu (Copying by reference)

Một trong những khác biệt cơ bản của các đối tượng so với nguyên thủy là chúng được lưu trữ và sao chép "bằng cách tham chiếu".

Các giá trị nguyên thủy: chuỗi, số, booleans -- được gán/sao chép "dưới dạng toàn bộ giá trị".

Ví dụ:

```js
      let message = "Hello!";
      let phrase = message;
```

Kết quả là chúng ta có hai biến độc lập, mỗi biến đang lưu trữ chuỗi `"Hello!"`.

![](variable-copy-value.png)

Đối tượng không như thế.

**Một biến lưu trữ không phải chính đối tượng, mà là "địa chỉ trong bộ nhớ", nói cách khác là "một tham chiếu" đến nó.**

Đây là hình ảnh cho đối tượng:

```js
      let user = {
        name: "John"
      };
```

![](variable-contains-reference.png)

Ở đây, đối tượng được lưu trữ ở đâu đó trong bộ nhớ. Và biến `user` có "tham chiếu" đến nó.

**Khi một biến đối tượng được sao chép -- tham chiếu được sao chép, đối tượng không được sao chép.**

Nếu chúng ta tưởng tượng một đối tượng như một cái tủ, thì một biến là chìa khóa cho nó. Sao chép một biến sao chép khóa, nhưng không phải chính tủ.

Ví dụ:

```js
      let user = { name: "John" };

      let admin = user; // copy the reference
```

Bây giờ chúng ta có hai biến, mỗi biến có tham chiếu đến cùng một đối tượng:

![](variable-copy-reference.png)

Chúng ta có thể sử dụng bất kỳ biến nào để truy cập vào cái tủ và sửa đổi nội dung của nó:

```js
      let user = { name: 'John' };

      let admin = user;

      admin.name = 'Pete'; // changed by the "admin" reference

      alert(user.name); // 'Pete', changes are seen from the "user" reference
```

Ví dụ trên chứng tỏ rằng chỉ có một đối tượng. Như thể chúng ta có một cái tủ có hai chìa khóa và sử dụng một trong số chúng (`admin`) để vào trong đó. Sau đó, nếu sau này chúng ta sử dụng khóa khác (`user`), chúng ta sẽ thấy các thay đổi.

### So sánh theo tham chiếu (Comparison by reference)

Các toán tử bằng nhau `==` và bằng nhau nghiêm ngặt `===` cho các đối tượng hoạt động giống hệt nhau.

**Hai đối tượng chỉ bằng nhau nếu chúng là cùng một đối tượng.**

Chẳng hạn, hai biến tham chiếu cùng một đối tượng, chúng bằng nhau:

```js
      let a = {};
      let b = a; // copy the reference

      alert( a == b ); // true, both variables reference the same object
      alert( a === b ); // true
```

Và ở đây hai đối tượng độc lập không bằng nhau, mặc dù cả hai đều trống rỗng:

```js
      let a = {};
      let b = {}; // two independent objects

      alert( a == b ); // false
```

Để so sánh như `obj1 > obj2` hoặc để so sánh `obj == 5`, các đối tượng được chuyển đổi thành nguyên thủy. Chúng ta sẽ nghiên cứu cách chuyển đổi đối tượng hoạt động như nào sớm thôi, nhưng nói thật, việc so sánh như vậy là rất hiếm và thường là kết quả của một nhầm lẫn khi code.

### Đối tượng Const (Const object)

Một đối tượng được khai báo là `const` *có thể* bị thay đổi.

Ví dụ:

```js
      const user = {
        name: "John"
      };

      user.age = 25; // (*)

      alert(user.age); // 25
```

Có vẻ như dòng `(*)` sẽ gây ra lỗi, nhưng không, hoàn toàn không có vấn đề gì. Đó là bởi vì `const` sửa giá trị của chính `user`. Và ở đây `user` lưu trữ tham chiếu đến cùng một đối tượng mọi lúc. Dòng `(*)` đi vào *bên trong* đối tượng, nó không gán với `user`.

Ví dụ, `const` sẽ báo lỗi nếu chúng ta cố gắng đặt `user` thành một thứ khác:

```js
      const user = {
        name: "John"
      };

      // Error (can't reassign user)
      user = {
        name: "Pete"
      };
```

...Nhưng nếu chúng ta muốn tạo các thuộc tính đối tượng không đổi thì sao? Như vậy, `user.age = 25` sẽ báo lỗi. Điều đó cũng có thể. Chúng ta sẽ đề cập đến nó trong chương **property-descriptors**.

## Nhân bản và hợp nhất, Object.assign (Cloning and merging, Object.assign)

Vì vậy, sao chép một biến đối tượng sẽ tạo thêm một tham chiếu đến cùng một đối tượng.

Nhưng nếu chúng ta cần sao chép một đối tượng thì sao? Tạo một bản sao độc lập, một nhân bản?

Điều đó cũng có thể thực hiện được, nhưng khó khăn hơn một chút, vì không có phương thức tích hợp sẵn cho JavaScript. Trên thực tế, điều đó hiếm khi cần thiết. Sao chép bằng cách tham khảo là tốt hầu hết thời gian.

Nhưng nếu chúng ta thực sự muốn điều đó, thì chúng ta cần tạo một đối tượng mới và sao chép cấu trúc của đối tượng hiện có bằng cách lặp lại các thuộc tính của nó và sao chép chúng ở cấp độ nguyên thủy.

Như thế này:

```js
      let user = {
        name: "John",
        age: 30
      };

      let clone = {}; // the new empty object

      // let's copy all user properties into it
      for (let key in user) {
        clone[key] = user[key];
      }

      // now clone is a fully independent clone
      clone.name = "Pete"; // changed the data in it

      alert( user.name ); // still John in the original object
```

Ngoài ra, chúng ta có thể sử dụng phương thức [Object.assign](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/assign) cho điều đó.

Cú pháp là:

```js
      Object.assign(dest[, src1, src2, src3...])
```

- Các đối số `dest` và  `src1, ..., srcN` (có thể nhiều như cần thiết) là các đối tượng.
- Nó sao chép các thuộc tính của tất cả các đối tượng `src1, ..., srcN` thành `dest`. Nói cách khác, các thuộc tính của tất cả các đối số bắt đầu từ thứ 2 được sao chép vào thứ 1. Sau đó, nó trả về `dest`.

Chẳng hạn, chúng ta có thể sử dụng nó để hợp nhất một số đối tượng thành một:

```js
      let user = { name: "John" };

      let permissions1 = { canView: true };
      let permissions2 = { canEdit: true };

      // copies all properties from permissions1 and permissions2 into user
      Object.assign(user, permissions1, permissions2);

      // now user = { name: "John", canView: true, canEdit: true }
```

Nếu đối tượng nhận (`user`) đã có cùng thuộc tính được đặt tên, nó sẽ bị ghi đè:

```js
      let user = { name: "John" };

      // overwrite name, add isAdmin
      Object.assign(user, { name: "Pete", isAdmin: true });

      // now user = { name: "Pete", isAdmin: true }
```

Chúng ta cũng có thể sử dụng `Object.assign` để thay thế vòng lặp để nhân bản đơn giản:

```js
      let user = {
        name: "John",
        age: 30
      };

      let clone = Object.assign({}, user);
```

Nó sao chép tất cả các thuộc tính của `user` vào đối tượng trống và trả về nó. Trên thực tế, giống như vòng lặp, nhưng ngắn hơn.

Cho đến bây giờ chúng ta giả định rằng tất cả các thuộc tính của `user` là nguyên thủy. Nhưng các thuộc tính có thể được tham chiếu đến các đối tượng khác. Làm gì với chúng đây?

Như thế này:

```js
      let user = {
        name: "John",
        sizes: {
          height: 182,
          width: 50
        }
      };

      alert( user.sizes.height ); // 182
```

Bây giờ không đủ để sao chép `clone.sizes = user.sizes`, vì`user.sizes` là một đối tượng, nó sẽ được sao chép bằng tham chiếu. Vì vậy, `clone` và `user` sẽ có cùng kích thước:

Như thế này:

```js
      let user = {
        name: "John",
        sizes: {
          height: 182,
          width: 50
        }
      };

      let clone = Object.assign({}, user);

      alert( user.sizes === clone.sizes ); // true, same object

      // user and clone share sizes
      user.sizes.width++;       // change a property from one place
      alert(clone.sizes.width); // 51, see the result from the other one
```

Để khắc phục điều đó, chúng ta nên sử dụng vòng lặp nhân bản kiểm tra từng giá trị của `user[key]` và, nếu đó là một đối tượng, thì cũng sao chép cấu trúc của nó. Điều đó được gọi là "nhân bản sâu".

Có một thuật toán tiêu chuẩn để nhân bản sâu xử lý các trường hợp trên và các trường hợp phức tạp hơn, được gọi là [Thuật toán nhân bản có cấu trúc](http://w3c.github.io/html/infrastructure.html#safe-passing-of-structured-data). Để không phát minh lại bánh xe, chúng ta có thể sử dụng triển khai thực hiện nó từ thư viện JavaScript [lodash](https://lodash.com), phương thức này được gọi là [_.cloneDeep(obj)](https://lodash.com/docs#cloneDeep).

## Tóm lược

Đối tượng là kết hợp các mảng với một số tính năng đặc biệt.

Chúng lưu trữ các thuộc tính (các cặp key-value), trong đó:
- Khóa thuộc tính phải là chuỗi hoặc symbols (thường là chuỗi).
- Giá trị có thể là bất kỳ loại nào.

Để truy cập một thuộc tính, chúng ta có thể sử dụng:
- Ký hiệu dấu chấm: `obj.property`.
- Ký hiệu ngoặc vuông `obj["property"]`. Dấu ngoặc vuông cho phép lấy khóa từ một biến, như `obj[varWithKey]`.

Toán tử bổ sung:
- Để xóa một thuộc tính: `delete obj.prop`.
- Để kiểm tra xem một thuộc tính có khóa đã cho có tồn tại không: `"key" in obj`.
- Để lặp lại một đối tượng: vòng lặp `for(let key in obj)`.

Các đối tượng được chỉ định và sao chép bằng cách tham chiếu. Nói cách khác, một biến lưu trữ không phải là "giá trị đối tượng", mà là "tham chiếu" (địa chỉ trong bộ nhớ) cho giá trị. Vì vậy, sao chép một biến như vậy hoặc chuyển nó dưới dạng đối số hàm sao chép tham chiếu đó, không phải đối tượng. Tất cả các hoạt động thông qua các tham chiếu được sao chép (như thêm/xóa các thuộc tính) được thực hiện trên cùng một đối tượng.

Để tạo một "bản sao thực" (một nhân bản), chúng ta có thể sử dụng `Object.assign` hoặc [_.cloneDeep(obj)](https://lodash.com/docs#cloneDeep).

Những gì chúng ta đã nghiên cứu trong chương này được gọi là "đối tượng đơn giản", hoặc chỉ là 'Đối tượng'.

Có nhiều loại đối tượng khác trong JavaScript:

- `Array` để lưu trữ các bộ sưu tập dữ liệu theo thứ tự,
- `Date` để lưu trữ thông tin về ngày và giờ,
- `Error` để lưu trữ thông tin về lỗi.
- ...Và như vậy.

Chúng có những tính năng đặc biệt mà chúng ta sẽ nghiên cứu sau. Đôi khi mọi người nói một cái gì đó như "kiểu Array" hoặc "kiểu Date", nhưng chính thức chúng không phải là kiểu của riêng chúng, mà thuộc về một kiểu dữ liệu "object" duy nhất. Và chúng mở rộng nó theo nhiều cách khác nhau.

Các đối tượng trong JavaScript rất mạnh. Ở đây chúng ta vừa vạch ra bề mặt của một chủ đề thực sự rất lớn. Chúng ta sẽ làm việc chặt chẽ với các đối tượng và tìm hiểu thêm về chúng trong các phần tiếp theo của hướng dẫn.
