# Đệ quy và ngăn xếp (Recursion and stack)

Hãy quay trở lại các functions và nghiên cứu chúng sâu hơn.

Chủ đề đầu tiên của chúng ta sẽ là *đệ quy (recursion)*.

Nếu bạn đã quen với lập trình, thì có lẽ nó quen thuộc và bạn có thể bỏ qua chương này.

Đệ quy là một mẫu lập trình (programming pattern) hữu ích trong các tình huống khi một tác vụ có thể được phân chia tự nhiên thành nhiều nhiệm vụ cùng loại, nhưng đơn giản hơn. Hoặc khi một tác vụ có thể được đơn giản hóa thành một hành động dễ dàng cộng với một biến thể đơn giản hơn của cùng một tác vụ. Hoặc, như chúng ta sẽ thấy sớm, để đối phó với các cấu trúc dữ liệu nhất định.

Khi một function giải quyết một nhiệm vụ, trong quá trình nó có thể gọi nhiều functions khác. Một trường hợp của điều này là khi một hàm gọi *chính nó*. Đó gọi là *đệ quy*.

## Hai cách nghĩ (Two ways of thinking)

Cho một cái gì đó đơn giản để bắt đầu -- hãy viết một hàm `pow(x, n)` làm tăng `x` lên một mũ của `n`. Nói cách khác, nhân `x` với chính nó `n` lần.

```js
pow(2, 2) = 4
pow(2, 3) = 8
pow(2, 4) = 16
```

Có hai cách để thực hiện nó.

1. Tư duy lặp lại: vòng lặp `for` (Iterative thinking: the `for` loop):

    ```js
    function pow(x, n) {
      let result = 1;

      // multiply result by x n times in the loop
      for (let i = 0; i < n; i++) {
        result *= x;
      }

      return result;
    }

    alert( pow(2, 3) ); // 8
    ```

2. Tư duy đệ quy: đơn giản hóa nhiệm vụ và tự gọi:

    ```js
    function pow(x, n) {
      if (n == 1) {
        return x;
      } else {
        return x * pow(x, n - 1);
      }
    }

    alert( pow(2, 3) ); // 8
    ```

Xin lưu ý cách biến thể đệ quy về cơ bản là khác nhau.

Khi `pow(x, n)` được gọi, việc thực thi chia thành hai nhánh:

```js
              if n==1  = x
             /
pow(x, n) =
             \       
              else     = x * pow(x, n - 1)
```

1. Nếu `n == 1`, thì mọi thứ đều bình thường. Nó được gọi là *the base* của đệ quy, bởi vì nó ngay lập tức tạo ra kết quả rõ ràng: `pow(x, 1)` bằng `x`.
2. Mặt khác, chúng ta có thể biểu diễn `pow(x, n)` là `x * pow(x, n - 1)`. Trong toán học, người ta sẽ viết <code>x<sup>n</sup> = x * x<sup>n-1</sup></code>. Đây được gọi là *một bước đệ quy*: chúng ta chuyển đổi tác vụ thành một hành động đơn giản hơn (nhân với `x`) và một lệnh gọi đơn giản hơn của cùng một tác vụ (`pow` với `n` thấp hơn). Các bước tiếp theo đơn giản hóa nó ngày càng xa hơn cho đến khi `n` đạt `1`.

Chúng ta cũng có thể nói rằng `pow` *gọi đệ quy chính nó* cho đến khi `n == 1`.

![recursive diagram of pow](recursion-pow.png)


Ví dụ, để tính toán `pow (2, 4)`, biến thể đệ quy thực hiện các bước sau:

1. `pow(2, 4) = 2 * pow(2, 3)`
2. `pow(2, 3) = 2 * pow(2, 2)`
3. `pow(2, 2) = 2 * pow(2, 1)`
4. `pow(2, 1) = 2`

Vì vậy, đệ quy giảm một cuộc gọi hàm đến một cuộc gọi đơn giản hơn, và sau đó -- thậm chí còn đơn giản hơn, và cho đến khi kết quả trở nên rõ ràng.

<br>

> ---

**📌 Đệ quy thường ngắn hơn**

Một giải pháp đệ quy thường ngắn hơn một giải pháp lặp lại.

Ở đây chúng ta có thể viết lại tương tự bằng cách sử dụng toán tử ternary `?` thay vì `if` để làm cho `pow(x, n)` ngắn gọn hơn và vẫn rất dễ đọc:

```js
function pow(x, n) {
  return (n == 1) ? x : (x * pow(x, n - 1));
}
```

> ---

<br>

Số lượng tối đa của cuộc gọi lồng nhau (bao gồm cuộc gọi đầu tiên) được gọi là *độ sâu đệ quy*. Trong trường hợp của chúng ta, nó sẽ chính xác là `n`.

Độ sâu đệ quy tối đa bị giới hạn bởi JavaScript engine. Chúng ta có thể đảm bảo khoảng 10000, một số engines cho phép nhiều hơn, nhưng 100000 có thể vượt quá giới hạn cho phần lớn trong số chúng. Có các tối ưu hóa tự động giúp làm nhẹ bớt điều này ("tối ưu hóa cuộc gọi đuôi (tail calls optimizations)"), nhưng chúng chưa được hỗ trợ ở mọi nơi và chỉ hoạt động trong các trường hợp đơn giản.

Điều đó giới hạn việc áp dụng đệ quy, nhưng nó vẫn còn rất rộng. Có nhiều nhiệm vụ trong đó cách suy nghĩ đệ quy cho mã đơn giản hơn, dễ bảo trì hơn.

## Ngăn xếp thực thi (The execution stack)

Bây giờ hãy xem xét cách các cuộc gọi đệ quy hoạt động. Cho rằng chúng ta sẽ xem xét các functions.

Thông tin về function chạy được lưu trữ trong *bối cảnh thực thi (execution context)* của nó.

[Bối cảnh thực thi (execution context)](https://tc39.github.io/ecma262/#sec-execut-contexts) là một cấu trúc dữ liệu nội bộ có chứa các chi tiết về việc thực thi hàm: nơi mà control flow đang hiện tại ở đó, các biến hiện tại, giá trị của `this` (chúng ta không sử dụng nó ở đây) và một vài chi tiết nội bộ khác.

Một function call có chính xác một execution context liên quan đến nó.

Khi một function thực hiện một cuộc gọi lồng nhau, điều sau đây xảy ra:

- The current function bị tạm dừng.
- The execution context liên quan đến nó được ghi nhớ trong một cấu trúc dữ liệu đặc biệt gọi là *ngăn xếp bối cảnh thực thi (execution context stack)*.
- Cuộc gọi lồng nhau được thực thi.
- Sau khi kết thúc, bối cảnh thực thi cũ được lấy từ ngăn xếp và function bên ngoài được nối lại từ nơi nó dừng lại.

Chúng ta hãy xem những gì xảy ra trong cuộc gọi `pow(2, 3)`.

### pow(2, 3)

Khi bắt đầu cuộc gọi `pow(2, 3)` bối cảnh thực thi sẽ lưu trữ các biến: `x = 2, n = 3`, luồng thực thi nằm ở dòng `1` của hàm.

Chúng ta có thể phác họa nó như sau:

- **Context: { x: 2, n: 3, at line 1 }** | call: pow(2, 3)

Đó là khi function bắt đầu thực thi. Điều kiện `n == 1` là sai, vì vậy luồng tiếp tục vào nhánh thứ hai của `if`:

```js
function pow(x, n) {
  if (n == 1) {
    return x;
  } else {
    return x * pow(x, n - 1);
  }
}

alert( pow(2, 3) );
```


Các biến giống nhau, nhưng dòng thay đổi, vì vậy bối cảnh bây giờ là:

- **Context: { x: 2, n: 3, at line 5 }** | call: pow(2, 3)

Để tính toán `x * pow(x, n - 1)`, chúng ta cần tạo một cuộc gọi con (subcall) của `pow` với các đối số mới `pow(2, 2)`.

### pow(2, 2)

Để thực hiện một cuộc gọi lồng nhau, JavaScript ghi nhớ bối cảnh thực thi hiện tại trong *ngăn xếp bối cảnh thực thi*.

Ở đây chúng ta gọi cùng một hàm `pow`, nhưng nó hoàn toàn không thành vấn đề. Quá trình này giống nhau cho tất cả các functions:

1. Bối cảnh hiện tại "đã ghi nhớ (remembered)" trên đầu của ngăn xếp.
2. Bối cảnh mới được tạo ra cho subcall.
3. Khi subcall kết thúc -- bối cảnh trước đó được bật ra khỏi ngăn xếp và quá trình thực thi của nó tiếp tục.

Đây là ngăn xếp bối cảnh khi chúng ta vào trong subcall `pow(2, 2)`:

- **Context: { x: 2, n: 2, at line 1 }** | call: pow(2, 2)
- Context: { x: 2, n: 3, at line 5 }     | call: pow(2, 3)

Bối cảnh thực thi hiện tại mới ở trên cùng và bối cảnh đã ghi nhớ trước đó ở bên dưới.

Khi chúng ta hoàn thành subcall -- thật dễ dàng để tiếp tục bối cảnh trước đó, bởi vì nó giữ cả hai biến và vị trí chính xác của mã nơi nó dừng lại. Ở đây trong bức ảnh, chúng ta sử dụng từ "dòng", nhưng tất nhiên nó rõ ràng hơn.

### pow(2, 1)

Quá trình lặp lại: một subcall mới được tạo ở dòng `5`, bây giờ với các đối số `x=2`, `n=1`.

Một bối cảnh thực thi mới được tạo ra, bối cảnh trước đó được đẩy lên đầu của ngăn xếp:

- **Context: { x: 2, n: 1, at line 1 }** | call: pow(2, 1)
- Context: { x: 2, n: 2, at line 5 }     | call: pow(2, 2)
- Context: { x: 2, n: 3, at line 5 }     | call: pow(2, 3)

Hiện tại có 2 bối cảnh cũ và 1 bối cảnh hiện đang chạy cho `pow(2, 1)`.

### The exit

Trong quá trình thực thi `pow(2, 1)`, không giống như trước đây, điều kiện `n == 1` là đúng, do đó nhánh đầu tiên của `if` hoạt động:

```js
function pow(x, n) {
  if (n == 1) {
    return x;
  } else {
    return x * pow(x, n - 1);
  }
}
```

Không có các cuộc gọi lồng nhau nữa, vì vậy hàm kết thúc, trả về `2`.

Khi function kết thúc, bối cảnh thực thi của nó không còn cần thiết nữa, vì vậy nó bị xóa khỏi bộ nhớ. Cái trước được khôi phục khỏi đầu ngăn xếp:

- **Context: { x: 2, n: 2, at line 5 }** | call: pow(2, 2)
- Context: { x: 2, n: 3, at line 5 }     | call: pow(2, 3)

Việc thực thi `pow(2, 2)` được nối lại. Nó có kết quả của subcall `pow(2, 1)`, do đó, nó cũng có thể hoàn thành việc đánh giá `x * pow(x, n - 1)`, trả về `4`.

Sau đó, bối cảnh trước đó được khôi phục:

- **Context: { x: 2, n: 3, at line 5 }** | call: pow(2, 3)

Khi nó kết thúc, chúng ta có kết quả `pow(2, 3) = 8`.

Độ sâu đệ quy trong trường hợp này là: **3**.

Như chúng ta có thể thấy từ các minh họa ở trên, độ sâu đệ quy bằng với số lượng bối cảnh tối đa trong ngăn xếp.

Lưu ý các yêu cầu bộ nhớ. Bối cảnh cần bộ nhớ. Trong trường hợp của chúng ta, việc tăng số mũ của `n` thực sự đòi hỏi bộ nhớ cho các bối cảnh `n`, cho tất cả các giá trị thấp hơn của `n`.

Một thuật toán dựa trên vòng lặp sẽ tiết kiệm bộ nhớ hơn:

```js
function pow(x, n) {
  let result = 1;

  for (let i = 0; i < n; i++) {
    result *= x;
  }

  return result;
}
```

The iterative `pow` sử dụng một bối cảnh duy nhất thay đổi `i` và `result` trong quá trình. Yêu cầu bộ nhớ của nó là nhỏ, cố định và không phụ thuộc vào `n`.

**Bất kỳ đệ quy có thể được viết lại như một vòng lặp. Các biến thể vòng lặp thường có thể được thực hiện hiệu quả hơn.**

...Nhưng đôi khi việc viết lại là không tầm thường, đặc biệt là khi hàm sử dụng các tập con đệ quy khác nhau tùy thuộc vào điều kiện và hợp nhất kết quả của chúng hoặc khi phân nhánh phức tạp hơn. Và việc tối ưu hóa có thể là không cần thiết và hoàn toàn không xứng đáng với những nỗ lực.

Đệ quy có thể cung cấp một mã ngắn hơn, dễ hiểu và hỗ trợ hơn. Tối ưu hóa không bắt buộc ở mọi nơi, chủ yếu chúng ta cần một mã tốt, đó là lý do tại sao nó được sử dụng.

## Đệ quy giao nhau (Recursive traversals)

Một ứng dụng tuyệt vời khác của đệ quy là một recursive traversal.

Hãy tưởng tượng, chúng ta có một công ty. Cấu trúc nhân viên có thể được trình bày như một đối tượng:

```js
let company = {
  sales: [{
    name: 'John',
    salary: 1000
  }, {
    name: 'Alice',
    salary: 600
  }],

  development: {
    sites: [{
      name: 'Peter',
      salary: 2000
    }, {
      name: 'Alex',
      salary: 1800
    }],

    internals: [{
      name: 'Jack',
      salary: 1300
    }]
  }
};
```

Nói cách khác, một công ty có các phòng ban.

- Một bộ phận có thể có một loạt các nhân viên. Chẳng hạn, bộ phận `sales` có 2 nhân viên: John và Alice.
- Hoặc một bộ phận có thể tách thành các bộ phận, như `development` có hai nhánh: `site` và `internals`. Mỗi trong số họ có nhân viên riêng.
- Cũng có thể khi một phân khu phát triển, nó sẽ chia thành các phân khu (hoặc các nhóm).

    Chẳng hạn, bộ phận `site` trong tương lai có thể được chia thành các nhóm cho `siteA` và `siteB`. Và họ, có khả năng, có thể chia nhiều hơn nữa. Đó không phải là trên hình ảnh, chỉ là một cái gì đó để có trong trí tưởng tượng.

Bây giờ hãy nói rằng chúng ta muốn một function để có được tổng số tiền lương. Làm thế nào chúng ta có thể làm điều đó?

Một cách tiếp cận lặp đi lặp lại (iterative) là không dễ dàng, bởi vì cấu trúc không đơn giản. Ý tưởng đầu tiên có thể là tạo ra một vòng lặp `for` lặp qua cả `company` với subloop lồng nhau trên các phòng ban cấp 1. Nhưng sau đó, chúng ta cần nhiều subloops lồng nhau hơn để lặp qua các nhân viên trong các phòng ban cấp 2 như `site`. ...Và sau đó một subloop khác bên trong cho các phòng ban cấp 3 có thể xuất hiện trong tương lai? Chúng ta nên dừng lại ở cấp độ 3 hoặc thực hiện 4 cấp độ vòng lặp? Nếu chúng ta đặt 3-4 subloop lồng nhau trong mã để duyệt qua một đối tượng, nó sẽ trở nên khá xấu xí.

Hãy thử đệ quy.

Như chúng ta có thể thấy, khi function của chúng ta lấy một bộ phận để tính tổng lương, có hai trường hợp có thể xảy ra:

1. Đó là một bộ phận "đơn giản" với một *array of people* -- sau đó chúng ta có thể tính tổng lương theo một vòng lặp đơn giản.
2. Hoặc đó là *an object with `N` subdepartments* -- sau đó chúng ta có thể thực hiện các cuộc gọi đệ quy `N` để lấy tổng cho mỗi subdeps và kết hợp các kết quả.

The (1) là the base của đệ quy, the trivial case.

The (2) là bước đệ quy. Một nhiệm vụ phức tạp được chia thành các subtasks cho các phòng ban nhỏ hơn. Chúng có thể lần lượt tách ra một lần nữa, nhưng sớm hay muộn thì sự phân chia sẽ kết thúc ở (1).

Thuật toán có thể dễ đọc hơn từ mã:

```js
let company = { // the same object, compressed for brevity
  sales: [{name: 'John', salary: 1000}, {name: 'Alice', salary: 600 }],
  development: {
    sites: [{name: 'Peter', salary: 2000}, {name: 'Alex', salary: 1800 }],
    internals: [{name: 'Jack', salary: 1300}]
  }
};

// The function to do the job
function sumSalaries(department) {
  if (Array.isArray(department)) { // case (1)
    return department.reduce((prev, current) => prev + current.salary, 0); // sum the array
  } else { // case (2)
    let sum = 0;
    for (let subdep of Object.values(department)) {
      sum += sumSalaries(subdep); // recursively call for subdepartments, sum the results
    }
    return sum;
  }
}

alert(sumSalaries(company)); // 6700
```

Mã này ngắn và dễ hiểu (hy vọng vậy?). Đó là sức mạnh của đệ quy. Nó cũng hoạt động cho bất kỳ cấp độ phòng ban lồng nhau nào.

Đây là sơ đồ của các cuộc gọi:

![recursive salaries](recursive-salaries.png)

Chúng ta có thể dễ dàng thấy nguyên tắc: đối với một đối tượng `{...}` các subcalls được tạo ra, trong khi các mảng `[...]` là "lá" của cây đệ quy, chúng cho kết quả ngay lập tức.

Lưu ý rằng mã sử dụng các tính năng thông minh mà chúng ta đã đề cập trước đây:

- Phương thức `arr.reduce` đã giải thích trong chương **array-methods** để lấy tổng của mảng.
- Vòng lặp `for(val of Object.values(obj))` để iterate qua các giá trị đối tượng: `Object.values` trả về một mảng của chúng.

## Cấu trúc đệ quy (Recursive structures)

Cấu trúc dữ liệu đệ quy (định nghĩa đệ quy) là cấu trúc tự sao chép chính nó trong các phần.

Chúng ta vừa thấy nó trong ví dụ về cấu trúc công ty ở trên.

Một *bộ phận* công ty là:
- Hoặc là một mảng của mọi người.
- Hoặc một đối tượng có các *bộ phận*.

Đối với các nhà phát triển web, có nhiều ví dụ phổ biến hơn: tài liệu HTML và XML.

Trong tài liệu HTML, một *HTML-tag* có thể chứa danh sách:
- Text pieces.
- HTML-comments.
- Other *HTML-tags* (that in turn may contain text pieces/comments or other tags etc).

Đó lại là một định nghĩa đệ quy.

Để hiểu rõ hơn, chúng ta sẽ đề cập đến một cấu trúc đệ quy có tên "Linked list" có thể là sự thay thế tốt hơn cho các mảng trong một số trường hợp.

### Linked list

Hãy tưởng tượng, chúng ta muốn lưu trữ một danh sách các đối tượng theo thứ tự.

Sự lựa chọn tự nhiên sẽ là một mảng:

```js
let arr = [obj1, obj2, obj3];
```

...Nhưng có một vấn đề với mảng. Các hoạt động "delete element" và "insert element" rất tốn kém. Ví dụ, hoạt động `arr.unshift(obj)` phải đánh số lại tất cả các phần tử để nhường chỗ cho một `obj` mới và nếu mảng lớn, sẽ mất thời gian. Tương tự với `arr.shift()`.

Các sửa đổi cấu trúc duy nhất không yêu cầu đánh số lại hàng loạt là những sửa đổi hoạt động với phần cuối của mảng: `arr.push/pop`. Vì vậy, một mảng có thể khá chậm cho các hàng đợi lớn.

Ngoài ra, nếu chúng ta thực sự cần chèn/xóa nhanh, chúng ta có thể chọn một cấu trúc dữ liệu khác gọi là [linked list](https://en.wikipedia.org/wiki/Linked_list).

The *linked list element* được định nghĩa đệ quy là một đối tượng với:
- `value`.
- thuộc tính `next` tham chiếu tới *linked list element* tiếp theo * hoặc `null` nếu kết thúc.

Ví dụ:

```js
let list = {
  value: 1,
  next: {
    value: 2,
    next: {
      value: 3,
      next: {
        value: 4,
        next: null
      }
    }
  }
};
```

Biểu diễn đồ họa của danh sách:

![linked list](linked-list.png)

Một mã thay thế để tạo:

```js
let list = { value: 1 };
list.next = { value: 2 };
list.next.next = { value: 3 };
list.next.next.next = { value: 4 };
```

Ở đây chúng ta thậm chí có thể thấy rõ hơn rằng có nhiều đối tượng, mỗi đối tượng có `value` và `next` chỉ (pointing) tới hàng xóm. Biến `list` là đối tượng đầu tiên trong chuỗi, vì vậy, sau các con trỏ `next` từ nó, chúng ta có thể tiếp cận bất kỳ phần tử nào.

Danh sách có thể dễ dàng chia thành nhiều phần và sau đó được nối lại:

```js
let secondList = list.next.next;
list.next.next = null;
```

![linked list split](linked-list-split.png)

To join:

```js
list.next.next = secondList;
```

Và chắc chắn chúng ta có thể chèn hoặc loại bỏ các items ở bất kỳ nơi nào.

Chẳng hạn, để thêm một giá trị mới, chúng ta cần cập nhật phần đầu của danh sách:

```js
let list = { value: 1 };
list.next = { value: 2 };
list.next.next = { value: 3 };
list.next.next.next = { value: 4 };

// prepend the new value to the list
list = { value: "new item", next: list };
```

![linked list](linked-list-0.png)

Để xóa một giá trị từ giữa, thay đổi `next` của giá trị trước:

```js
list.next = list.next.next;
```

![linked list](linked-list-remove-1.png)

Chúng ta đã thực hiện `list.next` nhảy qua `1` thành giá trị `2`. Giá trị `1` hiện được loại trừ khỏi chuỗi. Nếu nó không được lưu trữ ở bất kỳ nơi nào khác, nó sẽ tự động bị xóa khỏi bộ nhớ.

Không giống như mảng, không phải đánh số lại, chúng ta có thể dễ dàng sắp xếp lại các phần tử.

Đương nhiên, lists không phải lúc nào cũng tốt hơn mảng. Nếu không mọi người sẽ chỉ sử dụng lists.

Hạn chế chính là chúng ta không thể dễ dàng truy cập một phần tử bằng số của nó. Trong một mảng dễ dàng để: `arr[n]` là một tham chiếu trực tiếp. Nhưng trong list, chúng ta cần bắt đầu từ item đầu tiên và đi `next` `N` lần để lấy phần tử N.

...Nhưng chúng ta không luôn cần những thao tác như vậy. Chẳng hạn, khi chúng ta cần một hàng đợi hoặc thậm chí là [deque](https://en.wikipedia.org/wiki/Double-ended_queue) -- cấu trúc được sắp xếp phải cho phép thêm/xóa các phần tử từ cả hai đầu rất nhanh.

Đôi khi, đáng để thêm một biến khác có tên `tail` để theo dõi phần tử cuối cùng của danh sách (và cập nhật nó khi thêm/xóa các phần tử từ cuối). Đối với các bộ phần tử lớn, sự khác biệt về tốc độ so với mảng là rất lớn.

## Tóm lược

Điều kiện:

- *Đệ quy* là thuật ngữ lập trình có nghĩa là "self-calling" function. Các functions như vậy có thể được sử dụng để giải quyết các nhiệm vụ nhất định theo những cách thanh lịch.

    Khi một function tự gọi, đó gọi là *bước đệ quy*. *Điều cơ bản* của đệ quy là các đối số hàm làm cho tác vụ thật đơn giản để hàm không cần thực hiện nhiều cuộc gọi xa hơn nữa.

- Cấu trúc dữ liệu [được định nghĩa đệ quy](https://en.wikipedia.org/wiki/Recursive_data_type) là cấu trúc dữ liệu có thể được xác định bằng chính nó.

    Ví dụ, linked list có thể được định nghĩa là một cấu trúc dữ liệu bao gồm một đối tượng tham chiếu đến một list (hoặc null).

    ```js
    list = { value, next -> list }
    ```

    Các cây như cây phần tử HTML hoặc cây bộ phận từ chương này cũng được đệ quy một cách tự nhiên: chúng phân nhánh và mỗi nhánh có thể có các nhánh khác.

    Các hàm đệ quy có thể được sử dụng để đi qua chúng như chúng ta đã thấy trong ví dụ `sumSalary`.

Bất kỳ hàm đệ quy nào cũng có thể được viết lại thành một hàm lặp. Và điều đó đôi khi được yêu cầu để tối ưu hóa. Nhưng đối với nhiều tác vụ, một giải pháp đệ quy là đủ nhanh và dễ dàng hơn để viết và hỗ trợ.

