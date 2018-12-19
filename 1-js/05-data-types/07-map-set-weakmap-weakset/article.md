
# Map, Set, WeakMap and WeakSet

Bây giờ chúng ta đã tìm hiểu về các cấu trúc dữ liệu phức tạp sau:

- Đối tượng để lưu trữ các bộ sưu tập có khóa.
- Mảng để lưu trữ các bộ sưu tập theo thứ tự.

Nhưng điều đó là không đủ cho cuộc sống thực. Đó là lý do tại sao `Map` và `Set` cũng tồn tại.

## Map

[Map](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map) là một tập hợp các items dữ liệu có khóa, giống như một `Object`. Nhưng sự khác biệt chính là `Map` cho phép các kiểu khóa.

Các phương thức chính là:

- `new Map()` -- tạo map.
- `map.set(key, value)` -- lưu trữ giá trị theo khóa.
- `map.get(key)` -- trả về giá trị theo khóa, `undefined` nếu` key` không tồn tại trong map.
- `map.has(key)` -- trả về `true` nếu `key` tồn tại, `false` nếu không.
- `map.delete(key)` -- xóa giá trị bằng khóa.
- `map.clear()` -- xóa map.
- `map.size` -- trả về số phần tử hiện tại.

Ví dụ:

```js
      let map = new Map();

      map.set('1', 'str1');   // a string key
      map.set(1, 'num1');     // a numeric key
      map.set(true, 'bool1'); // a boolean key

      // remember the regular Object? it would convert keys to string
      // Map keeps the type, so these two are different:
      alert( map.get(1)   ); // 'num1'
      alert( map.get('1') ); // 'str1'

      alert( map.size ); // 3
```

Như chúng ta có thể thấy, không giống như các đối tượng, các khóa không được chuyển đổi thành chuỗi. Bất kỳ kiểu của khóa là có thể.

**Map cũng có thể sử dụng các đối tượng làm khóa.**

Ví dụ:

```js
      let john = { name: "John" };

      // for every user, let's store their visits count
      let visitsCountMap = new Map();

      // john is the key for the map
      visitsCountMap.set(john, 123);

      alert( visitsCountMap.get(john) ); // 123
```

Sử dụng các đối tượng làm khóa là một trong những tính năng `Map` đáng chú ý và quan trọng nhất. Đối với các string keys, `Object` có thể ổn, nhưng sẽ khó có thể thay thế `Map` bằng một `Object` thông thường như trong ví dụ trên.

Vào thời xa xưa, trước khi `Map` tồn tại, mọi người đã thêm các định danh độc nhất (unique identifiers) vào các đối tượng cho điều đó:

```js
      // we add the id field
      let john = { name: "John", id: 1 };

      let visitsCounts = {};

      // now store the value by id
      visitsCounts[john.id] = 123;

      alert( visitsCounts[john.id] ); // 123
```

...Nhưng 'Map` thanh lịch hơn nhiều.

<br>

> ---

**📌 Cách `Map` so sánh các khóa**

Để kiểm tra các giá trị tương đương, `Map` sử dụng thuật toán [SameValueZero](https://tc39.github.io/ecma262/#sec-samevaluezero). Nó gần giống như đẳng thức nghiêm ngặt `===`, nhưng sự khác biệt là `NaN` được coi là bằng với `NaN`. Vì vậy, `NaN` cũng có thể được sử dụng làm khóa.

Thuật toán này không thể thay đổi hoặc tùy chỉnh.

> ---

<br>
<br>

> ---

**📌 Xâu chuỗi (Chaining)**

Mỗi lệnh gọi `map.set` trả về map, vì vậy chúng ta có thể "xâu chuỗi (chain)" các cuộc gọi:

```js
      map.set('1', 'str1')
        .set(1, 'num1')
        .set(true, 'bool1');
```

> ---

<br>

## Map from Object

Khi một `Map` được tạo, chúng ta có thể truyền một mảng (hoặc một iterable khác) với các cặp key-value, như thế này:

```js
      // array of [key, value] pairs
      let map = new Map([
        ['1',  'str1'],
        [1,    'num1'],
        [true, 'bool1']
      ]);
```

Có một phương thức tích hợp [Object.entries(obj)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/entries) trả về một mảng của các cặp key/value cho một đối tượng chính xác theo định dạng đó.

Vì vậy, chúng ta có thể khởi tạo map từ một object như thế này:

```js
      let map = new Map(Object.entries({
        name: "John",
        age: 30
      }));
```

Ở đây, `Object.entries` trả về mảng các cặp key/value: `[ ["name","John"], ["age", 30] ]`. Đó là những gì `Map` cần.

## Lặp lại trên Map (Iteration over Map)

Để lặp qua một `map`, có 3 phương thức:

- `map.keys()` -- trả về một iterable cho các khóa,
- `map.values()` -- trả về một iterable cho các giá trị,
- `map.entries()` -- trả về một lần lặp cho các mục `[key, value]`, nó được sử dụng theo mặc định trong `for..of`.

Ví dụ:

```js
      let recipeMap = new Map([
        ['cucumber', 500],
        ['tomatoes', 350],
        ['onion',    50]
      ]);

      // iterate over keys (vegetables)
      for (let vegetable of recipeMap.keys()) {
        alert(vegetable); // cucumber, tomatoes, onion
      }

      // iterate over values (amounts)
      for (let amount of recipeMap.values()) {
        alert(amount); // 500, 350, 50
      }

      // iterate over [key, value] entries
      for (let entry of recipeMap) { // the same as of recipeMap.entries()
        alert(entry); // cucumber,500 (and so on)
      }
```

<br>

> ---

**📌 Thứ tự chèn được sử dụng**

The iteration đi theo thứ tự giống như các giá trị được chèn vào. `Map` duy trì trật tự này, không giống như một `Object` thông thường.

> ---

<br>

Bên cạnh đó, `Map` có một built-in `forEach` method, tương tự như `Array`:

```js
      recipeMap.forEach( (value, key, map) => {
        alert(`${key}: ${value}`); // cucumber: 500 etc
      });
```

## Set

Một `Set` là một tập hợp các giá trị, trong đó mỗi giá trị chỉ có thể xuất hiện một lần.

Phương thức chính của nó là:

- `new Set(iterable)` -- tạo set, tùy ý từ một mảng các giá trị (bất kỳ iterable nào cũng sẽ làm được).
- `set.add(value)` -- thêm một giá trị, trả về chính set đó.
- `set.delete(value)` -- xóa giá trị, trả về `true` nếu `value` tồn tại tại thời điểm cuộc gọi, nếu không thì `false`.
- `set.has(value)` -- trả về `true` nếu giá trị tồn tại trong set, ngược lại là `false`.
- `set.clear()` -- xóa mọi thứ khỏi the set.
- `set.size` -- là số phần tử được tính.

Ví dụ: chúng ta có khách truy cập đến và chúng ta muốn ghi nhớ mọi người. Nhưng các chuyến thăm lặp đi lặp lại không nên dẫn đến trùng lặp. Một khách truy cập phải được "tính" chỉ một lần.

`Set` chỉ là điều đúng đắn cho điều đó:

```js
      let set = new Set();

      let john = { name: "John" };
      let pete = { name: "Pete" };
      let mary = { name: "Mary" };

      // visits, some users come multiple times
      set.add(john);
      set.add(pete);
      set.add(mary);
      set.add(john);
      set.add(mary);

      // set keeps only unique values
      alert( set.size ); // 3

      for (let user of set) {
        alert(user.name); // John (then Pete and Mary)
      }
```

Thay thế cho `Set` có thể là một mảng người dùng, và mã để kiểm tra trùng lặp trên mỗi lần chèn bằng cách sử dụng [arr.find](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/find). Nhưng hiệu suất sẽ tồi tệ hơn nhiều, bởi vì phương pháp này đi qua toàn bộ mảng kiểm tra mọi phần tử. `Set` được tối ưu hóa tốt hơn nhiều trong nội bộ để kiểm tra tính duy nhất.

## Lặp lại trên Set (Iteration over Set)

Chúng ta có thể lặp qua một set với `for..of` hoặc sử dụng `forEach`:

```js
      let set = new Set(["oranges", "apples", "bananas"]);

      for (let value of set) alert(value);

      // the same with forEach:
      set.forEach((value, valueAgain, set) => {
        alert(value);
      });
```

Lưu ý những điều buồn cười. Hàm `forEach` trong `Set` có 3 đối số: một giá trị, sau đó *lại một giá trị*, và sau đó là đối tượng đích. Thật vậy, cùng một giá trị xuất hiện trong các đối số hai lần.

Đó là khả năng tương thích với `Map` trong đó `forEach` có ba đối số. Có vẻ hơi lạ, chắc chắn. Nhưng có thể giúp thay thế `Map` bằng `Set` trong một số trường hợp nhất định và ngược lại.

Các phương thức tương tự `Map` có cho các trình vòng lặp (iterators) cũng được hỗ trợ:

- `set.keys()` -- trả về một iterable object cho các giá trị,
- `set.values()` -- giống như `set.keys`, để tương thích với `Map`,
- `set.entries()` -- trả về một iterable object cho các mục `[value, value]`, tồn tại để tương thích với `Map`.

## WeakMap và Weakset

`Weakset` là một loại `Set` đặc biệt không ngăn JavaScript xóa các mục của nó khỏi bộ nhớ. `WeakMap` là điều tương tự với `Map`.

Như chúng ta đã biết từ chương **garbage-collection**, JavaScript engine lưu trữ một giá trị trong bộ nhớ trong khi có thể truy cập (và có khả năng có thể được sử dụng).

Ví dụ:

```js
      let john = { name: "John" };

      // the object can be accessed, john is the reference to it

      // overwrite the reference
      john = null;

      // the object will be removed from memory
```

Thông thường, các thuộc tính của một đối tượng hoặc các thành phần của một mảng hoặc cấu trúc dữ liệu khác được coi là có thể truy cập và được giữ trong bộ nhớ trong khi cấu trúc dữ liệu đó nằm trong bộ nhớ.

Chẳng hạn, nếu chúng ta đặt một đối tượng vào một mảng, thì trong khi mảng còn sống, đối tượng cũng sẽ sống, ngay cả khi không có tham chiếu nào khác đến nó.

Như thế này:

```js
      let john = { name: "John" };

      let array = [ john ];

      john = null; // overwrite the reference

      // john is stored inside the array, so it won't be garbage-collected
      // we can get it as array[0]
```

Hoặc, nếu chúng ta sử dụng một đối tượng làm khóa trong một `Map` thông thường, thì trong khi `Map` tồn tại, thì đối tượng đó cũng tồn tại. Nó chiếm bộ nhớ và có thể không được garbage collected.

Ví dụ:

```js
      let john = { name: "John" };

      let map = new Map();
      map.set(john, "...");

      john = null; // overwrite the reference

      // john is stored inside the map,
      // we can get it by using map.keys()
```

`WeakMap/WeakSet` về cơ bản là khác nhau về khía cạnh này. Chúng không ngăn chặn garbage-collection của các đối tượng chính.

Hãy giải thích nó bắt đầu bằng `WeakMap`.

Sự khác biệt đầu tiên so với `Map` là các khóa `WeakMap` phải là các đối tượng, không phải là các giá trị nguyên thủy:

```js
      let weakMap = new WeakMap();

      let obj = {};

      weakMap.set(obj, "ok"); // works fine (object key)

      // can't use a string as the key
      weakMap.set("test", "Whoops"); // Error, because "test" is not an object
```

Bây giờ, nếu chúng ta sử dụng một đối tượng làm khóa trong nó và không có tham chiếu nào khác đến đối tượng đó -- nó sẽ tự động bị xóa khỏi bộ nhớ (và khỏi map).

```js
      let john = { name: "John" };

      let weakMap = new WeakMap();
      weakMap.set(john, "...");

      john = null; // overwrite the reference

      // john is removed from memory!
```

So sánh nó với ví dụ `Map` thông thường ở trên. Bây giờ nếu `john` chỉ tồn tại như là khóa của `WeakMap` -- thì nó sẽ tự động bị xóa.

`WeakMap` không hỗ trợ phép lặp (iteration) và phương thức `keys()`, `values()`, `entries()`, vì vậy không có cách nào để lấy tất cả các khóa hoặc giá trị từ nó.

`WeakMap` chỉ có các phương thức sau:

- `weakMap.get(key)`
- `weakMap.set(key, value)`
- `weakMap.delete(key, value)`
- `weakMap.has(key)`

Tại sao lại hạn chế như vậy? Đó là vì lý do kỹ thuật. Nếu một đối tượng đã mất tất cả các tham chiếu khác (như `john` trong đoạn mã trên), thì nó sẽ được garbage-collected tự động. Nhưng về mặt kỹ thuật, nó không được chỉ định chính xác *khi việc dọn dẹp xảy ra*.

The JavaScript engine quyết định điều đó. Nó có thể chọn thực hiện dọn dẹp bộ nhớ ngay lập tức hoặc chờ và thực hiện việc dọn dẹp sau khi có nhiều thao tác xóa. Vì vậy, về mặt kỹ thuật, số lượng phần tử hiện tại của `WeakMap` không được biết đến. The engine có thể đã làm sạch nó hoặc không, hoặc đã làm một phần. Vì lý do đó, các phương thức truy cập toàn bộ `WeakMap` không được hỗ trợ.

Bây giờ chúng ta cần những thứ như vậy ở đâu?

Ý tưởng của `WeakMap` là chúng ta có thể lưu trữ thứ gì đó cho một đối tượng chỉ tồn tại trong khi đối tượng tồn tại. Nhưng chúng ta không ép đối tượng sống bằng thực tế là chúng ta lưu trữ một cái gì đó cho nó.

```js
      weakMap.set(john, "secret documents");
      // if john dies, secret documents will be destroyed automatically
```

Điều đó hữu ích cho các tình huống khi chúng ta có một bộ lưu trữ chính cho các đối tượng ở đâu đó và cần giữ thông tin bổ sung, điều đó chỉ có liên quan trong khi đối tượng sống.

Hãy xem xét một ví dụ.

Chẳng hạn, chúng ta có mã giữ số lượt truy cập cho mỗi người dùng. Thông tin được lưu trữ trong map: người dùng là chìa khóa và số lượt truy cập là giá trị. Khi người dùng rời đi, chúng ta không muốn lưu trữ số lượt truy cập của họ nữa.

Một cách sẽ là theo dõi người dùng và khi họ rời đi -- làm sạch the map theo cách thủ công:

```js
      let john = { name: "John" };

      // map: user => visits count
      let visitsCountMap = new Map();

      // john is the key for the map
      visitsCountMap.set(john, 123);

      // now john leaves us, we don't need him anymore
      john = null;

      // but it's still in the map, we need to clean it!
      alert( visitsCountMap.size ); // 1
      // and john is also in the memory, because Map uses it as the key
      ```

      Một cách khác là sử dụng `WeakMap`:

      ```js
      let john = { name: "John" };

      let visitsCountMap = new WeakMap();

      visitsCountMap.set(john, 123);

      // now john leaves us, we don't need him anymore
      john = null;

      // there are no references except WeakMap,
      // so the object is removed both from the memory and from visitsCountMap automatically
```

Với một 'Map` thông thường, việc dọn dẹp sau khi người dùng rời đi trở thành một công việc tẻ nhạt: chúng ta không chỉ cần xóa người dùng khỏi bộ lưu trữ chính của nó (có thể là một biến hoặc một mảng), mà còn cần dọn sạch các stores bổ sung như `visitCountMap`. Và nó có thể trở nên cồng kềnh trong các trường hợp phức tạp hơn khi người dùng được quản lý ở một nơi của mã và cấu trúc bổ sung ở một nơi khác và không nhận được thông tin nào về việc xóa.

<br>

> ---

**📌 summary**

`WeakMap` có thể làm mọi thứ đơn giản hơn, vì nó được dọn sạch tự động. Thông tin trong đó giống như lượt truy cập được tính trong ví dụ trên chỉ tồn tại trong khi đối tượng chính tồn tại.

> ---

<br>

`Weakset` hành xử tương tự:

- Nó tương tự với `Set`, nhưng chúng ta chỉ có thể thêm các đối tượng vào `Weakset` (không phải là nguyên thủy).
- Một đối tượng tồn tại trong set trong khi nó có thể truy cập từ một nơi khác.
- Giống như `Set`, nó hỗ trợ `add`, `has` và `delete`, nhưng không phải là  `size`, `keys()` và không lặp lại (no iterations).

Chẳng hạn, chúng ta có thể sử dụng nó để theo dõi xem tin nhắn có được đọc hay không:

```js
      let messages = [
          {text: "Hello", from: "John"},
          {text: "How goes?", from: "John"},
          {text: "See you soon", from: "Alice"}
      ];

      // fill it with array elements (3 items)
      let unreadSet = new WeakSet(messages);

      // use unreadSet to see whether a message is unread
      alert(unreadSet.has(messages[1])); // true

      // remove it from the set after reading
      unreadSet.delete(messages[1]); // true

      // and when we shift our messages history, the set is cleaned up automatically
      messages.shift();

      // no need to clean unreadSet, it now has 2 items
      // (though technically we don't know for sure when the JS engine clears it)
```

Hạn chế đáng chú ý nhất của `WeakMap` và `Weakset` là sự vắng mặt của các lần lặp (iterations), và không có khả năng nhận được tất cả nội dung hiện tại. Điều đó có vẻ bất tiện, nhưng không ngăn cản `WeakMap/WeakSet` thực hiện công việc chính của họ -- là một kho lưu trữ dữ liệu "bổ sung" cho các đối tượng được lưu trữ/quản lý ở nơi khác.

## Tóm lược

Bộ sưu tập thông thường:

- `Map` -- là tập hợp các giá trị có khóa.
    Sự khác biệt so với một `Object` thông thường:
    - Bất kỳ keys, objects đều có thể là keys.
    - Lặp lại theo thứ tự chèn (Iterates in the insertion order).
    - Các phương thức thuận tiện bổ sung, thuộc tính `size`.

- `Set` -- là tập hợp các giá trị duy nhất.
    - Không giống như một mảng, không cho phép sắp xếp lại các phần tử.
    - Giữ thứ tự chèn.

Bộ sưu tập cho phép garbage-collection:

- `WeakMap` -- một biến thể của `Map` chỉ cho phép các đối tượng làm khóa và loại bỏ chúng một khi chúng không thể truy cập được bằng các cách thức khác.
    - Nó không hỗ trợ các hoạt động trên toàn bộ cấu trúc: không `size`, không `clear()`, không lặp lại (iterations).

- `WeakSet` -- là một biến thể của `Set` chỉ lưu trữ các đối tượng và loại bỏ chúng một khi chúng không thể truy cập được bằng các cách thức khác.
    - Cũng không hỗ trợ `size/clear()` và iterations.

`WeakMap` và `WeakSet` được sử dụng làm cấu trúc dữ liệu "phụ" bên cạnh bộ lưu trữ đối tượng "chính". Khi đối tượng được xóa khỏi bộ lưu trữ chính, nếu nó chỉ được tìm thấy trong `WeakMap/WeakSet`, nó sẽ tự động được dọn sạch.
