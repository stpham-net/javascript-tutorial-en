
# Object.keys, values, entries

Hãy bước ra khỏi các cấu trúc dữ liệu riêng lẻ và nói về các lần lặp qua chúng. 

Trong chương trước, chúng ta đã thấy các phương thức `map.keys()`, `map.values()`, `map.entries()`.

Các phương thức này có đặc điểm chung, có một thỏa thuận chung là sử dụng chúng cho các cấu trúc dữ liệu. Nếu chúng ta tự tạo một cấu trúc dữ liệu, chúng ta cũng nên thực hiện chúng. 

Chúng được hỗ trợ cho:

- `Map`
- `Set`
- `Array` (except `arr.values()`)

Các đối tượng bình thường cũng hỗ trợ các phương thức tương tự, nhưng cú pháp hơi khác một chút.

## Object.keys, values, entries

Đối với các đối tượng bình thường, các phương thức sau đây có sẵn:

- [Object.keys(obj)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/keys) -- trả về một loạt các khóa.
- [Object.values(obj)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/values) -- trả về một mảng các giá trị.
- [Object.entries(obj)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/entries) -- trả về một mảng các cặp `[key, value]`.

...Nhưng xin lưu ý sự khác biệt (ví dụ so với map):

|             | Map              | Object                                   |
|-------------|------------------|------------------------------------------|
| Call syntax | `map.keys()`     | `Object.keys(obj)`, but not `obj.keys()` |
| Returns     | iterable         | "real" Array                             |

Sự khác biệt đầu tiên là chúng ta phải gọi `Object.keys(obj)`, chứ không phải `obj.keys()`.

Tại sao vậy? Lý do chính là sự linh hoạt. Hãy nhớ rằng, các đối tượng là một cơ sở của tất cả các cấu trúc phức tạp trong JavaScript. Vì vậy, chúng ta có thể có một đối tượng của riêng mình như `order` thực hiện phương thức `order.values()` của chính nó. Và chúng ta vẫn có thể gọi `Object.values(order)` trên đó.

Sự khác biệt thứ hai là các phương thức `Object.*` trả về các đối tượng mảng "thực", không chỉ là một iterable. Đó là vì lý do lịch sử.

Ví dụ:

```js
      let user = {
        name: "John",
        age: 30
      };
```

- `Object.keys(user) = [name, age]`
- `Object.values(user) = ["John", 30]`
- `Object.entries(user) = [ ["name","John"], ["age",30] ]`

Đây là một ví dụ về việc sử dụng `Object.values` để lặp lại các giá trị thuộc tính:

```js
      let user = {
        name: "John",
        age: 30
      };

      // loop over values
      for (let value of Object.values(user)) {
        alert(value); // John, then 30
      }
```

## Object.keys/values/entries ignore symbolic properties

Giống như một vòng lặp `for..in`, các phương thức này bỏ qua các thuộc tính sử dụng `Symbol(...)` làm khóa.

Thông thường đó là thuận tiện. Nhưng nếu chúng ta cũng muốn các symbolic keys, thì sẽ có một phương thức riêng [Object.getOwnPropertySymbols](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/getOwnPropertySymbols) chỉ trả về một mảng các symbolic keys. Ngoài ra, phương thức [Reflect.ownKeys(obj)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Reflect/ownKeys) trả về *tất cả* các khóa.
