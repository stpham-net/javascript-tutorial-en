# JSON methods, toJSON

Giả sử chúng ta có một đối tượng phức tạp và chúng ta muốn chuyển đổi nó thành một chuỗi, để gửi nó qua mạng hoặc chỉ để xuất nó cho mục đích logging.

Đương nhiên, một chuỗi như vậy nên bao gồm tất cả các thuộc tính quan trọng.

Chúng ta có thể thực hiện chuyển đổi như thế này:

```js
      let user = {
        name: "John",
        age: 30,

        toString() {
          return `{name: "${this.name}", age: ${this.age}}`;
        }
      };

      alert(user); // {name: "John", age: 30}
```

...Nhưng trong quá trình phát triển, các thuộc tính mới được thêm vào, các thuộc tính cũ được đổi tên và loại bỏ. Cập nhật `toString` như vậy mỗi lần có thể trở thành một nỗi đau. Chúng ta có thể thử lặp lại các thuộc tính trong nó, nhưng nếu đối tượng phức tạp và có các đối tượng lồng nhau trong các thuộc tính thì sao? Chúng ta cũng cần phải thực hiện chuyển đổi cho chúng. Và, nếu chúng ta gửi đối tượng qua mạng, thì chúng ta cũng cần cung cấp mã để "đọc" đối tượng của mình ở phía bên nhận.

May mắn thay, không cần phải viết mã để xử lý tất cả điều này. Nhiệm vụ đã được giải quyết rồi.

## JSON.stringify

[JSON](http://en.wikipedia.org/wiki/JSON) (JavaScript Object Notation) là định dạng chung để biểu thị các giá trị và đối tượng. Nó được mô tả như trong tiêu chuẩn [RFC 4627](http://tools.ietf.org/html/rfc4627). Ban đầu nó được tạo ra cho JavaScript, nhưng nhiều ngôn ngữ khác cũng có thư viện để xử lý nó.  Vì vậy, thật dễ dàng để sử dụng JSON để trao đổi dữ liệu khi máy khách sử dụng JavaScript và máy chủ được viết trên Ruby/PHP/Java/Whatever.

JavaScript cung cấp các phương thức:

- `JSON.stringify` để chuyển đổi các đối tượng thành JSON.
- `JSON.parse` để chuyển đổi JSON trở lại thành một đối tượng.

Chẳng hạn, ở đây chúng ta `JSON.stringify` một sinh viên:

```js
      let student = {
        name: 'John',
        age: 30,
        isAdmin: false,
        courses: ['html', 'css', 'js'],
        wife: null
      };

      let json = JSON.stringify(student);

      alert(typeof json); // we've got a string!

      alert(json);
      /* JSON-encoded object:
      {
        "name": "John",
        "age": 30,
        "isAdmin": sai,
        "courses": ["html", "css", "js"],
        "wife": null
      }
      */
```

Phương thức `JSON.stringify(student)` lấy đối tượng và chuyển đổi nó thành một chuỗi.

Kết quả chuỗi `json` được gọi là một đối tượng *JSON-encoded* hoặc *serialized* hoặc *stringified* hoặc *marshalled*. Chúng ta đã sẵn sàng để gửi nó qua dây hoặc đưa vào một kho lưu trữ dữ liệu đơn giản.

Xin lưu ý rằng một JSON-encoded object có một số khác biệt quan trọng so với object literal:

- Chuỗi sử dụng dấu ngoặc kép. Không có dấu ngoặc đơn hoặc backticks trong JSON. Vì vậy, `'John'` trở thành `"John"`.
- Object property names cũng được trích dẫn kép (double-quoted). Đó là điều bắt buộc. Vì vậy, `age:30` trở thành `"age":30`.

`JSON.stringify` cũng có thể được áp dụng cho các kiểu nguyên thủy.

Các kiểu JSON nguyên bản được hỗ trợ là:

- Objects `{ ... }`
- Arrays `[ ... ]`
- Primitives:
    - strings,
    - numbers,
    - boolean values `true/false`,
    - `null`.

Ví dụ:

```js
      // a number in JSON is just a number
      alert( JSON.stringify(1) ) // 1

      // a string in JSON is still a string, but double-quoted
      alert( JSON.stringify('test') ) // "test"

      alert( JSON.stringify(true) ); // true

      alert( JSON.stringify([1, 2, 3]) ); // [1,2,3]
```

JSON là đặc tả ngôn ngữ chéo chỉ có dữ liệu (data-only cross-language specification), do đó một số thuộc tính đối tượng dành riêng cho JavaScript bị bỏ qua bởi `JSON.stringify`.

Cụ thể là:

- Thuộc tính hàm (phương thức) (Function properties (methods)).
- Symbolic properties.
- Thuộc tính lưu trữ `undefined`.

```js
      let user = {
        sayHi() { // ignored
          alert("Hello");
        },
        [Symbol("id")]: 123, // ignored
        something: undefined // ignored
      };

      alert( JSON.stringify(user) ); // {} (empty object)
```

Thường thì không sao. Nếu đó không phải là những gì chúng ta muốn, rồi chúng ta sẽ sớm thấy cách tùy chỉnh quá trình.

Điều tuyệt vời là các đối tượng lồng nhau được hỗ trợ và chuyển đổi tự động.

Ví dụ:

```js
      let meetup = {
        title: "Conference",
        room: {
          number: 23,
          participants: ["john", "ann"]
        }
      };

      alert( JSON.stringify(meetup) );
      /* The whole structure is stringified:
      {
        "title":"Conference",
        "room":{"number":23,"participants":["john","ann"]},
      }
      */
```

Giới hạn quan trọng: không được có tham chiếu lòng vòng (circular references).

Ví dụ:

```js
      let room = {
        number: 23
      };

      let meetup = {
        title: "Conference",
        participants: ["john", "ann"]
      };

      meetup.place = room;       // meetup references room
      room.occupiedBy = meetup; // room references meetup

      JSON.stringify(meetup); // Error: Converting circular structure to JSON
```

Ở đây, việc chuyển đổi không thành công, vì tham chiếu lòng vòng: `room.occupiedBy` tham chiếu `meetup` và `meetup.place` tham chiếu `room`:

![](json-meetup.png)


## Loại trừ và chuyển đổi: thay thế (Excluding and transforming: replacer)

Cú pháp đầy đủ của `JSON.stringify` là:

```js
      let json = JSON.stringify(value[, replacer, space])
```

**value**

Một giá trị để mã hóa.

**replacer**

Mảng các thuộc tính để mã hóa hoặc một hàm ánh xạ (mapping function) `function(key, value)`.

**space**

Số lượng space sử dụng để định dạng

Hầu hết thời gian, `JSON.stringify` chỉ được sử dụng với đối số đầu tiên. Nhưng nếu chúng ta cần tinh chỉnh quá trình chuyển đổi, như lọc ra các tham chiếu lòng vòng, chúng ta có thể sử dụng đối số thứ hai của `JSON.stringify`.

Nếu chúng ta truyền một mảng các thuộc tính cho nó, chỉ các thuộc tính này sẽ được mã hóa.

Ví dụ:

```js
      let room = {
        number: 23
      };

      let meetup = {
        title: "Conference",
        participants: [{name: "John"}, {name: "Alice"}],
        place: room // meetup references room
      };

      room.occupiedBy = meetup; // room references meetup

      alert( JSON.stringify(meetup, ['title', 'participants']) );
      // {"title":"Conference","participants":[{},{}]}
```

Ở đây chúng ta có lẽ là quá nghiêm ngặt. Danh sách thuộc tính được áp dụng cho toàn bộ cấu trúc đối tượng. Vì vậy, participants trống, vì `name` không có trong danh sách.

Chúng ta hãy bao gồm mọi thuộc tính ngoại trừ `room.occupiedBy` đã gây ra tham chiếu lòng vòng:

```js
      let room = {
        number: 23
      };

      let meetup = {
        title: "Conference",
        participants: [{name: "John"}, {name: "Alice"}],
        place: room // meetup references room
      };

      room.occupiedBy = meetup; // room references meetup

      alert( JSON.stringify(meetup, ['title', 'participants', 'place', 'name', 'number']) );
      /*
      {
        "title":"Conference",
        "participants":[{"name":"John"},{"name":"Alice"}],
        "place":{"number":23}
      }
      */
```

Bây giờ mọi thứ ngoại trừ `occupiedBy` được nối tiếp. Nhưng danh sách các thuộc tính khá dài.

May mắn thay, chúng ta có thể sử dụng một hàm thay vì một mảng như `replacer`.

Hàm sẽ được gọi cho mọi cặp `(key, value)` và sẽ trả về giá trị "được thay thế", sẽ được sử dụng thay cho giá trị ban đầu.

Trong trường hợp của chúng ta, chúng ta có thể trả về `value` "cho" tất cả mọi thứ ngoại trừ `occupiedBy`. Để bỏ qua `occupiedBy`, đoạn mã dưới đây trả về `undefined`:

```js
      let room = {
        number: 23
      };

      let meetup = {
        title: "Conference",
        participants: [{name: "John"}, {name: "Alice"}],
        place: room // meetup references room
      };

      room.occupiedBy = meetup; // room references meetup

      alert( JSON.stringify(meetup, function replacer(key, value) {
        alert(`${key}: ${value}`); // to see what replacer gets
        return (key == 'occupiedBy') ? undefined : value;
      }));

      /* key:value pairs that come to replacer:
      :             [object Object]
      title:        Conference
      participants: [object Object],[object Object]
      0:            [object Object]
      name:         John
      1:            [object Object]
      name:         Alice
      place:        [object Object]
      number:       23
      */
```

Xin lưu ý rằng hàm `replacer` nhận mọi cặp key/value bao gồm các đối tượng lồng nhau và các array items. Nó được áp dụng đệ quy. Giá trị của `this` bên trong `replacer` là đối tượng chứa thuộc tính hiện tại.

Cuộc gọi đầu tiên là đặc biệt. Nó được tạo bằng cách sử dụng một "wrapper object" đặc biệt: `{"": meetup}`. Nói cách khác, cặp `(key, value)` đầu tiên có một khóa trống và toàn bộ giá trị là đối tượng đích. Đó là lý do tại sao dòng đầu tiên là `":[object Object]"` trong ví dụ trên.

Ý tưởng là cung cấp càng nhiều năng lực cho `replacer` càng tốt: nó có cơ hội phân tích và thay thế/bỏ qua toàn bộ đối tượng nếu cần thiết.

## Formatting: spacer

Đối số thứ ba của `JSON.stringify(value, replacer, spaces)` là số lượng khoảng trắng được sử dụng cho định dạng đẹp.

Trước đây, tất cả các đối tượng được xâu chuỗi (stringified) không có thụt lề (indents) và khoảng trắng thừa. Điều đó tốt nếu chúng ta muốn gửi một đối tượng qua mạng. Đối số `spacer` được sử dụng riêng cho một đầu ra đẹp.

Ở đây `spacer = 2` bảo JavaScript hiển thị các đối tượng lồng nhau trên nhiều dòng, với thụt 2 khoảng trắng bên trong một đối tượng:

```js
      let user = {
        name: "John",
        age: 25,
        roles: {
          isAdmin: false,
          isEditor: true
        }
      };

      alert(JSON.stringify(user, null, 2));
      /* two-space indents:
      {
        "name": "John",
        "age": 25,
        "roles": {
          "isAdmin": false,
          "isEditor": true
        }
      }
      */

      /* for JSON.stringify(user, null, 4) the result would be more indented:
      {
          "name": "John",
          "age": 25,
          "roles": {
              "isAdmin": false,
              "isEditor": true
          }
      }
      */
```

Tham số `space` chỉ được sử dụng cho mục đích logging và đầu ra đẹp.

## Custom "toJSON"

Giống như `toString` để chuyển đổi chuỗi, một đối tượng có thể cung cấp phương thức `toJSON` cho chuyển đổi to-JSON. `JSON.stringify` tự động gọi nó nếu có sẵn.

Ví dụ:

```js
      let room = {
        number: 23
      };

      let meetup = {
        title: "Conference",
        date: new Date(Date.UTC(2017, 0, 1)),
        room
      };

      alert( JSON.stringify(meetup) );
      /*
        {
          "title":"Conference",
          "date":"2017-01-01T00:00:00.000Z",  // (1)
          "room": {"number":23}               // (2)
        }
      */
```

Ở đây chúng ta có thể thấy rằng `date` `(1)` đã trở thành một chuỗi. Đó là bởi vì tất cả các ngày đều có built-in `toJSON` method trả về kiểu chuỗi như vậy.

Bây giờ, hãy thêm một `toJSON` tùy chỉnh cho đối tượng của chúng ta `room`:

```js
      let room = {
        number: 23,
        toJSON() {
          return this.number;
        }
      };

      let meetup = {
        title: "Conference",
        room
      };

      alert( JSON.stringify(room) ); // 23

      alert( JSON.stringify(meetup) );
      /*
        {
          "title":"Conference",
          "room": 23
        }
      */
```

Như chúng ta có thể thấy, `toJSON` được sử dụng cả cho cuộc gọi trực tiếp `JSON.stringify(room)` và cho đối tượng lồng nhau.

## JSON.parse

Để giải mã JSON-string, chúng ta cần một phương thức khác có tên [JSON.parse](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/JSON/parse).

Cú pháp:

```js
      let value = JSON.parse(str[, reviver]);
```

**str**

JSON-string để phân tích cú pháp.

**reviver**

Tùy chọn function(key,value) sẽ được gọi cho mỗi cặp `(key, value)` và có thể biến đổi giá trị.

Ví dụ:

```js
      // stringified array
      let numbers = "[0, 1, 2, 3]";

      numbers = JSON.parse(numbers);

      alert( numbers[1] ); // 1
```

Hoặc cho các đối tượng lồng nhau:

```js
      let user = '{ "name": "John", "age": 35, "isAdmin": false, "friends": [0,1,2,3] }';

      user = JSON.parse(user);

      alert( user.friends[1] ); // 1
```

JSON có thể phức tạp đến mức cần thiết, các đối tượng và mảng có thể bao gồm các đối tượng và mảng khác. Nhưng họ phải tuân theo định dạng.

Dưới đây là những lỗi điển hình trong JSON viết tay (đôi khi chúng ta phải viết nó cho mục đích gỡ lỗi):

```js
      let json = `{
        name: "John",                     // mistake: property name without quotes
        "surname": 'Smith',               // mistake: single quotes in value (must be double)
        'isAdmin': false                  // mistake: single quotes in key (must be double)
        "birthday": new Date(2000, 2, 3), // mistake: no "new" is allowed, only bare values
        "friends": [0,1,2,3]              // here all fine
      }`;
```

Bên cạnh đó, JSON không hỗ trợ comments. Thêm một comment vào JSON làm cho nó không hợp lệ.

Có một định dạng khác có tên [JSON5](http://json5.org/), cho phép unquoted keys, comments, v.v. Nhưng đây là một thư viện độc lập, không có trong đặc điểm kỹ thuật của ngôn ngữ.

JSON thông thường nghiêm ngặt không phải vì các nhà phát triển của nó lười biếng, mà vì như thế sẽ cho phép thực hiện thuật toán phân tích cú pháp dễ dàng, đáng tin cậy và rất nhanh.

## Using reviver

Hãy tưởng tượng, chúng ta có một đối tượng `meetup` được xâu chuỗi (stringified) từ máy chủ.

Nó trông như thế này:

```js
      // title: (meetup title), date: (meetup date)
      let str = '{"title":"Conference","date":"2017-11-30T12:00:00.000Z"}';
```

...Và bây giờ chúng ta cần  *deserialize* nó, để quay lại thành đối tượng JavaScript.

Hãy làm điều đó bằng cách gọi `JSON.parse`:

```js
      let str = '{"title":"Conference","date":"2017-11-30T12:00:00.000Z"}';

      let meetup = JSON.parse(str);

      alert( meetup.date.getDate() ); // Error!
```

Rất tiếc! Một lỗi!

Giá trị của `meetup.date` là một chuỗi, không phải là một đối tượng `Date`. Làm thế nào để `JSON.parse` có thể biết rằng nó nên chuyển đổi chuỗi đó thành một `Date`?

Chúng ta hãy chuyển đến `JSON.parse` một hàm hồi sinh (reviving function) để trả về tất cả các giá trị "nguyên trạng", nhưng `date` sẽ trở thành một `Date`:

```js
      let str = '{"title":"Conference","date":"2017-11-30T12:00:00.000Z"}';

      let meetup = JSON.parse(str, function(key, value) {
        if (key == 'date') return new Date(value);
        return value;
      });

      alert( meetup.date.getDate() ); // now works!
```

Nhân tiện, nó cũng hoạt động cho các đối tượng lồng nhau:

```js
      let schedule = `{
        "meetups": [
          {"title":"Conference","date":"2017-11-30T12:00:00.000Z"},
          {"title":"Birthday","date":"2017-04-18T12:00:00.000Z"}
        ]
      }`;

      schedule = JSON.parse(schedule, function(key, value) {
        if (key == 'date') return new Date(value);
        return value;
      });

      alert( schedule.meetups[1].date.getDate() ); // works!
```

## Tóm lược

- JSON là một định dạng dữ liệu có các thư viện và tiêu chuẩn độc lập riêng cho hầu hết các ngôn ngữ lập trình.
- JSON hỗ trợ các đối tượng đơn giản, mảng, chuỗi, số, booleans và `null`.
- JavaScript cung cấp các phương thức [JSON.stringify](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify) để chuyển tiếp (serialize) thành JSON và [JSON.parse](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/JSON/parse) để đọc từ JSON.
- Cả hai phương thức đều hỗ trợ transformer functions để đọc/ghi thông minh.
- Nếu một đối tượng có `toJSON`, thì nó được gọi bởi `JSON.stringify`.
