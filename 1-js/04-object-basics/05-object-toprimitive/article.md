
# Chuyển đổi đối tượng sang nguyên thủy

Điều gì xảy ra khi các đối tượng được thêm `obj1 + obj2`, hoặc trừ `obj1 - obj2` hoặc được in bằng cách sử dụng `alert(obj)`?

Có các phương thức đặc biệt trong các đối tượng thực hiện chuyển đổi.

Trong chương **chuyển đổi kiểu** chúng ta đã thấy các quy tắc cho chuyển đổi số, chuỗi và boolean của nguyên thủy. Nhưng chúng ta đã để lại một khoảng cách (gap) cho các đối tượng. Bây giờ, khi chúng ta biết về các methods và symbols, cũng là lúc có thể đóng khoảng cách đó lại.

Đối với các đối tượng, không có chuyển đổi thành boolean, bởi vì tất cả các đối tượng là `true` trong bối cảnh boolean. Vì vậy, chỉ có chuyển đổi chuỗi và số.

Việc chuyển đổi số xảy ra khi chúng ta trừ các đối tượng hoặc áp dụng các hàm toán học. Chẳng hạn, các đối tượng `Date` (sẽ được đề cập trong chương **date**) có thể bị trừ và kết quả của `date1 - date2` là chênh lệch thời gian giữa hai ngày.

Đối với chuyển đổi chuỗi -- nó thường xảy ra khi chúng ta xuất ra một đối tượng như `alert(obj)` và trong các bối cảnh tương tự.

## ToPrimitive

Khi một đối tượng được sử dụng trong bối cảnh một nguyên thủy là bắt buộc,  ví dụ, trong một `alert` hoặc các phép tính toán học, nó sẽ chuyển đổi thành giá trị nguyên thủy bằng thuật toán `ToPrimitive` ([specification](https://tc39.github.io/ecma262/#sec-toprimitive)).

Thuật toán đó cho phép chúng ta tùy chỉnh chuyển đổi sử dụng một object method đặc biệt.

Tùy thuộc vào ngữ cảnh, chuyển đổi có cái gọi là "gợi ý (hint)".

**`"string"`** 

Khi một phép tính mong đợi một chuỗi, cho các chuyển đổi object-to-string, như `alert`:

```js
      // output
      alert(obj);

      // using object as a property key
      anotherObj[obj] = 123;
```

**`"number"`** 

Khi một phép tính mong đợi một số, cho các chuyển đổi object-to-number, như toán học:

```js
      // explicit conversion
      let num = Number(obj);

      // maths (except binary plus)
      let n = +obj; // unary plus
      let delta = date1 - date2;

      // less/greater comparison
      let greater = user1 > user2;
```

**`"default"`** 

Xảy ra trong những trường hợp hiếm hoi khi phép tính "không chắc chắn" sẽ mong đợi kiểu nào.

Chẳng hạn, nhị phân cộng `+` có thể hoạt động cả với chuỗi (nối chúng) và số (thêm chúng), vì vậy cả chuỗi và số đều có thể. Hoặc khi một đối tượng được so sánh bằng cách sử dụng `==` với một chuỗi, số hoặc một symbol.

```js
      // binary plus
      let total = car1 + car2;

      // obj == string/number/symbol
      if (user == 1) { ... };
```

Toán tử lớn hơn/ít hơn `<>` cũng có thể hoạt động với cả chuỗi và số. Tuy nhiên, nó sử dụng gợi ý "number", không phải "default". Đó là vì lý do lịch sử.

Trong thực tế, tất cả các  built-in objects ngoại trừ một trường hợp (đối tượng `Date`, chúng ta sẽ tìm hiểu sau) thực hiện chuyển đổi `"default"` giống như `"number"`. Và có lẽ chúng ta nên làm như vậy.

Xin lưu ý -- chỉ có ba gợi ý. Nó đơn giản mà. Không có gợi ý "boolean" (tất cả các đối tượng là `true` trong ngữ cảnh boolean) hoặc bất cứ điều gì khác. Và nếu chúng ta đối xử với `"default"` và `"number"` giống nhau, giống như hầu hết các công cụ dựng sẵn, thì chỉ có hai chuyển đổi.

**Để thực hiện chuyển đổi, JavaScript cố gắng tìm và gọi ba phương thức đối tượng:**

1. Gọi `obj[Symbol.toPrimitive](hint)` nếu phương thức tồn tại,
2. Mặt khác, nếu gợi ý là `"string"`
    - thử `obj.toString()` và `obj.valueOf()`, bất cứ điều gì tồn tại.
3. Mặt khác, nếu gợi ý là `"number"` hoặc `"default"`
    - hãy thử `obj.valueOf()` và `obj.toString()`, bất cứ điều gì tồn tại.

## Symbol.toPrimitive

Hãy bắt đầu từ phương pháp đầu tiên. Có một built-in symbol có tên `Symbol.toPrimitive` nên được sử dụng để đặt tên cho phương thức chuyển đổi, như sau:

```js
      obj[Symbol.toPrimitive] = function(hint) {
        // return a primitive value
        // hint = one of "string", "number", "default"
      }
```

Chẳng hạn, ở đây đối tượng `user` thực hiện nó:

```js
      let user = {
        name: "John",
        money: 1000,

        [Symbol.toPrimitive](hint) {
          alert(`hint: ${hint}`);
          return hint == "string" ? `{name: "${this.name}"}` : this.money;
        }
      };

      // conversions demo:
      alert(user); // hint: string -> {name: "John"}
      alert(+user); // hint: number -> 1000
      alert(user + 500); // hint: default -> 1500
```

Như chúng ta có thể thấy từ mã, `user` trở thành một chuỗi tự mô tả hoặc một số tiền tùy thuộc vào chuyển đổi. Phương thức duy nhất `user[Symbol.toPrimitive]` xử lý tất cả các trường hợp chuyển đổi.

## toString/valueOf

Các phương thức `toString` và` valueOf` có từ thời cổ đại. Chúng không phải là symbols (symbols không tồn tại từ lâu), mà là các phương thức có tên chuỗi "thông thường". Chúng cung cấp một cách "old-style" khác để thực hiện chuyển đổi.

Nếu không có `Symbol.toPrimitive` thì JavaScript sẽ cố gắng tìm chúng và thử theo thứ tự:

- `toString -> valueOf` cho gợi ý "string".
- `valueOf -> toString` nếu không.

Chẳng hạn, ở đây `user` thực hiện tương tự như trên bằng cách sử dụng kết hợp `toString` và `valueOf`:

```js
      let user = {
        name: "John",
        money: 1000,

        // for hint="string"
        toString() {
          return `{name: "${this.name}"}`;
        },

        // for hint="number" or "default"
        valueOf() {
          return this.money;
        }

      };

      alert(user); // toString -> {name: "John"}
      alert(+user); // valueOf -> 1000
      alert(user + 500); // valueOf -> 1500
```

Thông thường chúng ta muốn một nơi "bắt tất cả (catch-all)" để xử lý tất cả các chuyển đổi nguyên thủy. Trong trường hợp này, chúng ta chỉ cần triển khai `toString`, như thế này:

```js
      let user = {
        name: "John",

        toString() {
          return this.name;
        }
      };

      alert(user); // toString -> John
      alert(user + 500); // toString -> John500
```

Trong trường hợp không có `Symbol.toPrimitive` và `valueOf`, `toString` sẽ xử lý tất cả các chuyển đổi nguyên thủy.

## ToPrimitive và ToString/ToNumber

Điều quan trọng cần biết về tất cả các phương thức chuyển đổi nguyên thủy là chúng không nhất thiết phải trả về nguyên thủy "đã gợi ý".

Không có kiểm soát nào cho dù `toString()` trả về chính xác một chuỗi hay liệu phương thức `Symbol.toPrimitive` trả về một số cho một gợi ý "number".

**Điều bắt buộc duy nhất: các phương thức này phải trả về nguyên thủy.**

Một phép tính bắt đầu chuyển đổi có được tính nguyên thủy đó, và sau đó tiếp tục làm việc với nó, áp dụng các chuyển đổi tiếp theo nếu cần thiết.

Ví dụ:

- Các phép tính toán (trừ binary plus) thực hiện chuyển đổi `ToNumber`:

    ```js
    let obj = {
      toString() { // toString handles all conversions in the absence of other methods
        return "2";
      }
    };

    alert(obj * 2); // 4, ToPrimitive gives "2", then it becomes 2
    ```

- Nhị phân cộng (binary plus) kiểm tra nguyên thủy -- nếu đó là một chuỗi, thì nó thực hiện nối, nếu không nó thực hiện `ToNumber` và làm việc với các số.

    Chuỗi ví dụ:
    
    ```js
    let obj = {
      toString() {
        return "2";
      }
    };

    alert(obj + 2); // 22 (ToPrimitive returned string => concatenation)
    ```

    Số ví dụ:
    
    ```js
    let obj = {
      toString() {
        return true;
      }
    };

    alert(obj + 2); // 3 (ToPrimitive returned boolean, not string => ToNumber)
    ```

<br>

> ---

**📌 Ghi chú lịch sử**

Vì các lý do lịch sử, các phương thức `toString` hoặc `valueOf` *nên* trả về một nguyên thủy: nếu bất kỳ trong số chúng trả về một đối tượng, thì không có lỗi, nhưng đối tượng đó bị bỏ qua (giống như nếu phương thức không tồn tại).

Ngược lại, `Symbol.toPrimitive` *phải* trả về một nguyên thủy, nếu không, sẽ có lỗi.

> ---

<br>

## Tóm lược

Chuyển đổi object-to-primitive được gọi tự động bởi nhiều built-in functions và operators mong đợi một giá trị nguyên thủy là một giá trị.

Có 3 loại (gợi ý) của nó:

- `"string"` (đối với `alert` và các chuyển đổi chuỗi khác)
- `"number"` (đối với toán học)
- `"default"` (vài toán tử)

Đặc tả mô tả rõ ràng toán tử nào sử dụng gợi ý nào. Có rất ít toán tử "không biết phải trông đợi điều gì" và sử dụng gợi ý `"default"`. Thông thường đối với các built-in objects, gợi ý `"default"` được xử lý giống như `"number"`, vì vậy trong thực tế, hai đối tượng cuối cùng thường được hợp nhất với nhau.

Thuật toán chuyển đổi là:

1. Gọi `obj[Symbol.toPrimitive](hint)` nếu phương thức tồn tại,
2. Mặt khác, nếu gợi ý là `"string"`
    - hãy thử `obj.toString()` và `obj.valueOf()`, bất cứ điều gì tồn tại.
3. Mặt khác, nếu gợi ý là `"number"` hoặc `"default"`
    - hãy thử `obj.valueOf()` và `obj.toString()`, bất cứ điều gì tồn tại.

Trong thực tế, thường chỉ cần triển khai `obj.toString()` như một phương thức "bắt tất cả (catch-all)" cho tất cả các chuyển đổi trả về đại diện "có thể đọc được" của một đối tượng, cho mục đích logging hoặc gỡ lỗi.  
