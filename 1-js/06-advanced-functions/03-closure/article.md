
# Closure

JavaScript là một ngôn ngữ rất function-oriented (hướng chức năng). Nó cho chúng ta rất nhiều tự do (freedom). Một hàm có thể được tạo tại một thời điểm, sau đó được sao chép sang một biến khác hoặc được chuyển làm đối số cho hàm khác và được gọi từ một vị trí hoàn toàn khác sau đó.

Chúng ta biết rằng một hàm có thể truy cập các biến bên ngoài nó; tính năng này được sử dụng khá thường xuyên.

Nhưng điều gì xảy ra khi một biến ngoài thay đổi? Liệu một function có lấy được giá trị gần đây nhất hay sẽ lấy giá trị đã tồn tại khi function đã được tạo?

Ngoài ra, điều gì xảy ra khi một hàm di chuyển đến một nơi khác trong mã và được gọi từ đó -- nó có được quyền truy cập vào các biến bên ngoài của địa điểm mới không?

Các ngôn ngữ khác nhau hành xử khác nhau, ở đây và trong chương này, chúng ta đề cập đến hành vi của JavaScript.

## Một vài câu hỏi (A couple of questions)

Chúng ta hãy xem xét hai tình huống để bắt đầu, và sau đó nghiên cứu từng phần cơ học bên trong, để bạn có thể trả lời các câu hỏi sau và những câu hỏi phức tạp hơn trong tương lai.

1. Hàm `sayHi` sử dụng một biến bên ngoài (external variable) `name`. Khi hàm chạy, nó sẽ sử dụng giá trị nào?

    ```js
    let name = "John";

    function sayHi() {
      alert("Hi, " + name);
    }

    name = "Pete";

    sayHi(); // what will it show: "John" or "Pete"?
    ```

    Những tình huống như vậy là phổ biến cả trong phát triển phía trình duyệt và máy chủ. Một function có thể được lên lịch để thực hiện muộn hơn so với khi nó được tạo, ví dụ sau hành động của người dùng hoặc network request.

    Vì vậy, câu hỏi là: nó có nhận được những thay đổi mới nhất không?

2. Hàm `makeWorker` tạo một hàm khác và trả về nó. Function mới đó có thể được gọi từ một nơi khác. Nó sẽ có quyền truy cập vào các biến bên ngoài từ nơi tạo của nó, hoặc nơi gọi, hoặc cả hai?

    ```js
    function makeWorker() {
      let name = "Pete";

      return function() {
        alert(name);
      };
    }

    let name = "John";

    // create a function
    let work = makeWorker();

    // call it
    work(); // what will it show? "Pete" (name where created) or "John" (name where called)?
    ```

## Môi trường từ vựng (Lexical Environment)

Để hiểu những gì đang xảy ra, trước tiên chúng ta hãy thảo luận về "biến" thực sự là gì.

Trong JavaScript, mọi hàm chạy, khối mã (code block) và toàn bộ script đều có một đối tượng liên quan được gọi là *Lexical Environment*.

The Lexical Environment object bao gồm hai phần:

1. *Environment Record* -- một đối tượng có tất cả các biến cục bộ làm thuộc tính của nó (và một số thông tin khác như giá trị của `this`).
2. Một tham chiếu đến *outer lexical environment*, thường là liên kết với code lexically ngay bên ngoài nó (bên ngoài dấu ngoặc nhọn hiện tại).

Vì vậy, một "biến" chỉ là một thuộc tính của đối tượng nội bộ đặc biệt, Environment Record. "Để lấy hoặc thay đổi một biến" có nghĩa là "để lấy hoặc thay đổi một thuộc tính của Lexical Environment".

Chẳng hạn, trong mã đơn giản này, chỉ có một Lexical Environment:

![lexical environment](lexical-environment-global.png)

Đây là cái gọi là global Lexical Environment, gắn liền với toàn bộ script. Đối với trình duyệt, tất cả các thẻ `<script>` đều có chung global environment.

Trên hình trên, hình chữ nhật có nghĩa là Environment Record (variable store) và mũi tên có nghĩa là tham chiếu bên ngoài. The global Lexical Environment không có tham chiếu bên ngoài, vì vậy nó trỏ đến `null`.

Đây là bức tranh lớn hơn về cách các biến `let` hoạt động:

![lexical environment](lexical-environment-global-2.png)

Hình chữ nhật ở phía bên phải cho thấy global Lexical Environment thay đổi như thế nào trong quá trình thực thi:

1. Khi script bắt đầu, the Lexical Environment trống rỗng.
2. Định nghĩa `let phrase` xuất hiện. Nó đã được gán no value, vì vậy `undefined` được lưu trữ.
3. `phrase` được gán một giá trị.
4. `phrase` tham chiếu đến một giá trị mới.

Hiện tại mọi thứ trông thật đơn giản phải không?

Để tóm tắt:

- Biến là một thuộc tính của một đối tượng nội bộ đặc biệt, được liên kết với block/function/script hiện đang thực thi.
- Làm việc với các biến là thực sự làm việc với các thuộc tính của đối tượng đó.

### Function Declaration

Function Declarations là đặc biệt. Không giống như các biến `let`, chúng được xử lý không phải khi thực thi đến chúng, mà khi Lexical Environment được tạo. Đối với global Lexical Environment, nó có nghĩa là thời điểm script được bắt đầu.

Đó là lý do tại sao chúng ta có thể gọi một function declaration trước khi nó được định nghĩa.

Mã dưới đây chứng minh rằng Lexical Environment không trống ngay từ đầu. Nó có `say`, vì đó là Function Declaration. Và sau đó, nó nhận được `phrase`, được khai báo với `let`:

![lexical environment](lexical-environment-global-3.png)


### Inner and outer Lexical Environment

Trong cuộc gọi, `say()` sử dụng một outer variable, vì vậy hãy xem chi tiết những gì đang diễn ra.

Đầu tiên, khi một function chạy, một function mới Lexical Environment được tạo tự động. Đó là một quy tắc chung cho tất cả các functions. That Lexical Environment được sử dụng để lưu trữ các biến và tham số cục bộ của cuộc gọi.

Đây là hình ảnh của Lexical Environments khi thực thi bên trong `say("John")`, tại dòng được dán nhãn bằng một mũi tên:

![lexical environment](lexical-environment-simple.png)

Trong cuộc gọi function, chúng ta có hai Lexical Environments: bên trong (đối với cuộc gọi function) và bên ngoài (global):

- The inner Lexical Environment tương ứng với thực thi hiện tại của `say`. Nó có một biến duy nhất: `name`, đối số hàm. Chúng ta đã gọi `say("John")`, vì vậy giá trị của `name` là `"John"`.
-  The outer Lexical Environment là global Lexical Environment.

The inner Lexical Environment có tham chiếu `outer` đến bên ngoài.

**Khi mã muốn truy cập vào một biến -- đầu tiên nó được tìm kiếm trong inner Lexical Environment, sau đó ở bên ngoài, sau đó là một bên ngoài và tiếp tục cho đến khi kết thúc chuỗi.**

Nếu không tìm thấy biến ở bất cứ đâu, đó là lỗi trong strict mode. Không có `use strict`, việc gán cho một undefined variable sẽ tạo ra một global variable, để tương thích ngược.

Hãy xem cách tìm kiếm thực hiện trong ví dụ của chúng ta:

- Khi `alert` bên trong `say` muốn truy cập `name`, nó sẽ tìm thấy nó ngay lập tức trong function Lexical Environment.
- Khi nó muốn truy cập `phrase`, thì không có `phrase` cục bộ, vì vậy nó đi theo tham chiếu `outer` và tìm thấy nó trên toàn cầu (globally).

![lexical environment lookup](lexical-environment-simple-lookup.png)

Bây giờ chúng ta có thể đưa ra câu trả lời cho câu hỏi đầu tiên từ đầu chương.

**Một hàm có các outer variables như chúng hiện tại đang là; nó sử dụng các giá trị gần đây nhất.**

Đó là vì cơ chế được mô tả. Các giá trị biến cũ không được lưu ở bất cứ đâu. Khi một hàm muốn chúng, nó lấy các giá trị hiện tại từ chính nó hoặc một outer Lexical Environment.

Vì vậy, câu trả lời cho câu hỏi đầu tiên là `Pete`:

```js
let name = "John";

function sayHi() {
  alert("Hi, " + name);
}

name = "Pete"; // (*)

sayHi(); // Pete
```

Luồng thực thi của mã ở trên:

1. The global Lexical Environment có `name: "John"`.
2. Tại dòng `(*)` the global variable được thay đổi, bây giờ nó có `name: "Pete"`.
3. Khi hàm `sayHi()`, được thực thi và lấy `name` từ bên ngoài. Đây là từ global Lexical Environment nơi nó đã là `"Pete"`.

<br>

> ---

**📌 Một cuộc gọi -- một môi trường từ điển (One call -- one Lexical Environment)**

Xin lưu ý rằng một new function Lexical Environment được tạo ra mỗi khi một function chạy.

Và nếu một hàm được gọi nhiều lần, thì mỗi lần gọi sẽ có Lexical Environment riêng, với các biến cục bộ và tham số cụ thể cho lần chạy đó.

> ---

<br>
<br>

> ---

**📌 Lexical Environment là một đối tượng đặc tả (specification object)**

"Lexical Environment" là một specification object. Chúng ta không thể lấy đối tượng này trong mã của mình và thao tác trực tiếp với nó. Các JavaScript engines cũng có thể tối ưu hóa nó, loại bỏ các biến không được sử dụng để tiết kiệm bộ nhớ và thực hiện các thủ thuật nội bộ khác, nhưng hành vi hiển thị phải được mô tả.

> ---

<br>

## Hàm lồng nhau (Nested functions)

Một hàm được gọi là "lồng nhau" khi nó được tạo bên trong một hàm khác.

Có thể dễ dàng thực hiện điều này với JavaScript.

Chúng ta có thể sử dụng nó để tổ chức mã của mình, như thế này:

```js
function sayHiBye(firstName, lastName) {

  // helper nested function to use below
  function getFullName() {
    return firstName + " " + lastName;
  }

  alert( "Hello, " + getFullName() );
  alert( "Bye, " + getFullName() );

}
```

Ở đây, *nested* function `getFullName()` được tạo ra để thuận tiện. Nó có thể truy cập các biến bên ngoài và do đó có thể trả lại tên đầy đủ.

Điều thú vị hơn, một hàm lồng nhau có thể được trả về: như là một thuộc tính của một đối tượng mới (nếu hàm bên ngoài tạo một đối tượng bằng các phương thức) hoặc là kết quả của chính nó. Sau đó nó có thể được sử dụng ở một nơi khác. Bất kể ở đâu, nó vẫn có quyền truy cập vào các biến bên ngoài tương tự.

Một ví dụ với constructor function (xem chương **constructor-new**):

```js
// constructor function returns a new object
function User(name) {

  // the object method is created as a nested function
  this.sayHi = function() {
    alert(name);
  };
}

let user = new User("John");
user.sayHi(); // the method code has access to the outer "name"
```

Một ví dụ với việc trả về một hàm:

```js
function makeCounter() {
  let count = 0;

  return function() {
    return count++; // has access to the outer counter
  };
}

let counter = makeCounter();

alert( counter() ); // 0
alert( counter() ); // 1
alert( counter() ); // 2
```

Hãy tiếp tục với ví dụ `makeCorer`. Nó tạo ra "counter" function trả về số tiếp theo trên mỗi lệnh gọi. Mặc dù đơn giản, các biến thể được sửa đổi một chút của mã đó có thể sử dụng thực tế, ví dụ như [pseudorandom number generator](https://en.wikipedia.org/wiki/Pseudorandom_number_generator), v.v. Vì vậy, ví dụ không phải là giả như nó có thể thể hiện.

Làm thế nào để các counter làm việc nội bộ?

Khi hàm bên trong chạy, biến trong `count++` được tìm kiếm từ trong ra ngoài. Đối với ví dụ trên, thứ tự sẽ là:

![](lexical-search-order.png)

1. The locals of the nested function...
2. Các biến của outer function...
3. Và như vậy cho đến khi nó đạt đến các global variables.

Trong ví dụ này, `count` được tìm thấy ở bước `2`. Khi một outer variable được sửa đổi, nó sẽ thay đổi ở nơi nó được tìm thấy. Vì vậy, `count++` tìm thấy outer variable và tăng nó trong Lexical Environment nơi nó thuộc về. Giống như nếu chúng ta có `let count = 1`.

Đây là hai câu hỏi để xem xét:

1. Bằng cách nào đó chúng ta có thể đặt lại `counter` từ mã không thuộc về `makeCounter` không? Ví dụ, sau khi `alert` gọi trong ví dụ trên.
2. Nếu chúng ta gọi `makeCounter()` nhiều lần -- nó sẽ trả về nhiều hàm `counter`. Chúng độc lập hay chúng chia sẻ cùng một `count`?

Hãy cố gắng trả lời chúng trước khi bạn tiếp tục đọc.

...

All done?

Được rồi, chúng ta hãy đi qua các câu trả lời.

1. Không có cách nào. The `counter` là một local function variable, chúng ta không thể truy cập nó từ bên ngoài.
2. Đối với mỗi lệnh gọi tới `makeCounter()`, một new function Lexical Environment được tạo ra, với `counter` của chính nó. Vì vậy, các hàm `counter` kết quả là độc lập.

Đây là bản demo:

```js
function makeCounter() {
  let count = 0;
  return function() {
    return count++;
  };
}

let counter1 = makeCounter();
let counter2 = makeCounter();

alert( counter1() ); // 0
alert( counter1() ); // 1

alert( counter2() ); // 0 (independent)
```

Hy vọng rằng, trạng thái với các outer variables là khá rõ ràng cho bạn bây giờ. Nhưng trong những tình huống phức tạp hơn, có thể cần phải hiểu sâu hơn về nội bộ. Vì vậy, hãy lặn sâu hơn.

## Environments in detail

Bây giờ bạn đã hiểu cách closures hoạt động nói chung, chúng ta có thể đi xuống các đai ốc và bu lông.

Đây là những gì đang diễn ra trong ví dụ `makeCounter` từng bước, hãy theo nó để đảm bảo rằng bạn hiểu mọi thứ. Vui lòng lưu ý thuộc tính `[[Environment]]` bổ sung mà chúng ta chưa bao gồm.

1. Khi kịch bản vừa mới bắt đầu, chỉ có global Lexical Environment:

    ![](lexenv-nested-makecounter-1.png)

    Tại thời điểm bắt đầu đó, chỉ có `makeCounter` function, bởi vì đó là Function Declaration. Nó chưa chạy.

    Tất cả các functions "khi sinh" nhận được một thuộc tính ẩn `[[Environment]]` với tham chiếu đến Lexical Environment được tạo thành của chúng. Chúng ta chưa nói về nó, nhưng đó là cách function biết nó được tạo ra ở đâu.

    Ở đây, `makeCounter` được tạo ra trong global Lexical Environment, vì vậy `[[Environment]]` giữ một tham chiếu đến nó.

    Nói cách khác, một function được "in dấu" với tham chiếu đến Lexical Environment nơi nó được sinh ra. Và `[[Environment]]` là thuộc tính hàm ẩn có tham chiếu đó.

2. Mã chạy trên, biến toàn cục mới `counter` được khai báo và với giá trị của nó `makeCounter()` được gọi. Đây là một ảnh chụp khoảnh khắc khi thực thi ở dòng đầu tiên bên trong `makeCounter()`:

    ![](lexenv-nested-makecounter-2.png)

    Tại thời điểm cuộc gọi của `makeCounter()`, the Lexical Environment được tạo ra, để giữ các biến và đối số của nó.

    Như tất cả các Lexical Environments, nó lưu trữ hai điều:
    1. Một Environment Record với các biến cục bộ. Trong trường hợp của chúng ta, `count` là biến cục bộ duy nhất (xuất hiện khi dòng `let count` được thực thi).
    2. Tham chiếu outer lexical, được đặt thành `[[Environment]]` của hàm. Ở đây `[[Environment]]` của `makeCounter` tham chiếu đến the global Lexical Environment.

    Vì vậy, bây giờ chúng ta có hai Lexical Environments: cái đầu tiên là toàn cầu, cái thứ hai là cho cuộc gọi `makeCounter` hiện tại, với tham chiếu outer là global.

3. Trong quá trình thực thi `makeCounter()`, một hàm lồng nhỏ được tạo ra.

    Không quan trọng là function được tạo bằng Function Declaration hay Function Expression. Tất cả các hàm đều có thuộc tính `[[Environment]]` tham chiếu đến Lexical Environment mà chúng được tạo. Vì vậy, hàm lồng nhỏ mới của chúng ta cũng có được nó.

    Đối với hàm lồng nhau mới của chúng ta, giá trị của `[[Environment]]` là Lexical Environment hiện tại của `makeCounter()` (nơi nó được sinh ra):

    ![](lexenv-nested-makecounter-3.png)

    Xin lưu ý rằng ở bước này, inner function đã được tạo, nhưng chưa được gọi. Mã bên trong `function() { return count++; }` không chạy; chúng ta sẽ sớm trả lại nó.

4. Khi việc thực thi diễn ra, lệnh gọi `makeCounter()` kết thúc và kết quả (hàm lồng nhỏ) được gán cho global variable `counter`:

    ![](lexenv-nested-makecounter-4.png)

    Hàm đó chỉ có một dòng: `return count++`, sẽ được thực thi khi chúng ta chạy nó.

5. Khi `counter()` được gọi, Một Lexical Environment "trống" được tạo cho nó. Nó không có local variables. Nhưng `[[Environment]]` của `counter` được sử dụng làm tham chiếu bên ngoài cho nó, vì vậy nó có quyền truy cập vào các biến của lệnh `makeCounter()` trước đây khi nó được tạo:

    ![](lexenv-nested-makecounter-5.png)

    Bây giờ nếu nó truy cập vào một biến, đầu tiên nó sẽ tìm kiếm Lexical Environment riêng của nó (trống), sau đó là Lexical Environment của cuộc gọi `makeCounter()` trước đó, sau đó là global.

    Khi tìm kiếm `count`, nó tìm thấy nó trong số các biến `makeCounter`, trong Lexical Environment bên ngoài gần nhất.

    Xin lưu ý cách memory management hoạt động ở đây. Mặc dù cuộc gọi `makeCounter()` đã kết thúc cách đây một thời gian, Lexical Environment của nó vẫn được giữ lại trong bộ nhớ, bởi vì có một hàm lồng nhau với `[[Environment]]` tham chiếu đến nó.

    Nói chung, một đối tượng Lexical Environment vẫn sống khi mà có một function có thể sử dụng nó. Và chỉ khi không còn, nó mới bị xóa.

6. Lệnh gọi `counter()` không chỉ trả về giá trị của `count`, mà còn tăng nó. Lưu ý rằng việc sửa đổi được thực hiện "tại chỗ". Giá trị của `count` được sửa đổi chính xác trong môi trường nơi nó được tìm thấy.

    ![](lexenv-nested-makecounter-6.png)

    Vì vậy, chúng ta trở lại bước trước với một thay đổi duy nhất -- giá trị mới của `count`. Các cuộc gọi sau đều làm như vậy.

7. Các lệnh `counter()` tiếp theo làm tương tự.

Câu trả lời cho câu hỏi thứ hai từ đầu chương nên rõ ràng.

Hàm `work()` trong đoạn mã dưới đây sử dụng `name` từ vị trí gốc của nó thông qua tham chiếu outer lexical environment:

![](lexenv-nested-work.png)

Vì vậy, kết quả là `"Pete"` ở đây.

Nhưng nếu không có `let name` trong `makeWorker()`, thì tìm kiếm sẽ đi ra ngoài và lấy global variable như chúng ta có thể thấy từ chuỗi bên trên. Trong trường hợp đó, nó sẽ là `"John"`.

<br>

> ---

**📌 Closures**

Có một thuật ngữ lập trình chung "closure", mà các nhà phát triển thường nên biết.

Một [closure](https://en.wikipedia.org/wiki/Closure_(computer_programming)) là một hàm ghi nhớ các biến ngoài của nó và có thể truy cập chúng. Trong một số ngôn ngữ, điều đó là không thể, hoặc một function nên được viết theo một cách đặc biệt để làm cho nó xảy ra. Nhưng như đã giải thích ở trên, trong JavaScript, tất cả các hàm đều được đóng kín một cách tự nhiên (naturally closures) (chỉ có một loại trừ, được đề cập trong **new-function**).

Đó là: chúng tự động nhớ nơi chúng được tạo bằng thuộc tính `[[Environment]]` và tất cả chúng có thể truy cập các biến ngoài.

Khi phỏng vấn, một nhà phát triển frontend nhận được câu hỏi về "what's a closure?.

> ---

<br>

## Khối mã và vòng lặp, IIFE

Các ví dụ trên tập trung vào các functions. Nhưng Lexical Environments cũng tồn tại đối với các khối mã `{...}`.

Chúng được tạo khi một code block chạy và chứa các block-local variables. Dưới đây là một vài ví dụ.

## If

Trong ví dụ dưới đây, khi thực thi đi vào khối `if`, the new "if-only" Lexical Environment được tạo cho nó:

![](lexenv-if.png)

The new Lexical Environment lấy bọc chung quanh nó làm tham chiếu bên ngoài, vì vậy có thể tìm thấy `phrase`. Nhưng tất cả các biến và Function Expressions được khai báo bên trong `if` nằm trong Lexical Environment đó và không thể nhìn thấy từ bên ngoài.

Chẳng hạn, sau khi `if` kết thúc, `alert` bên dưới sẽ không thấy `user`, do đó xảy ra lỗi.

## For, while

Đối với một vòng lặp, mỗi lần lặp lại có một Lexical Environment riêng biệt. Nếu một biến được khai báo trong `for`, thì nó cũng cục bộ với Lexical Environment đó:

```js
for (let i = 0; i < 10; i++) {
  // Each loop has its own Lexical Environment
  // {i: value}
}

alert(i); // Error, no such variable
```

Đó thực sự là một ngoại lệ, bởi vì `let i` nằm ngoài trực quan của `{...}`. Nhưng trên thực tế, mỗi lần chạy của vòng lặp đều có Lexical Environment riêng với `i` hiện tại trong đó.

Sau vòng lặp, `i` không hiển thị.

### Code blocks

Chúng ta cũng có thể sử dụng một khối mã "trần" ("bare" code block) `{…}` để tách các biến thành một "phạm vi cục bộ (local scope)".

Chẳng hạn, trong một trình duyệt web, tất cả các scripts chia sẻ cùng một khu vực toàn cầu (global area). Vì vậy, nếu chúng ta tạo một global variable trong một script, nó sẽ có sẵn cho những cái khác. Nhưng điều đó trở thành một nguồn xung đột nếu hai scripts sử dụng cùng một tên biến và ghi đè lên nhau.

Điều đó có thể xảy ra nếu tên biến là một từ phổ biến và các tác giả script không biết về nhau.

Nếu chúng ta muốn tránh điều đó, chúng ta có thể sử dụng một khối mã để cô lập toàn bộ script hoặc một phần của script:

```js
{
  // do some job with local variables that should not be seen outside

  let message = "Hello";

  alert(message); // Hello
}

alert(message); // Error: message is not defined
```

Mã bên ngoài khối (hoặc bên trong script khác) không thấy các biến bên trong khối, bởi vì khối có Lexical Environment riêng.

### IIFE

Trong các tập lệnh cũ, người ta có thể tìm thấy cái gọi là "biểu thức hàm được gọi ngay lập tức" (viết tắt là IIFE) được sử dụng cho mục đích này.

Chúng trông như thế này:

```js
(function() {

  let message = "Hello";

  alert(message); // Hello

})();
```

Ở đây một biểu thức hàm được tạo và được gọi ngay lập tức. Vì vậy, mã thực thi ngay lập tức và có các biến riêng của nó.

Biểu thức hàm được gói bằng dấu ngoặc `(function {...})`, vì khi JavaScript gặp `"function"` trong luồng mã chính, nó hiểu nó là khởi đầu của một khai báo hàm. Nhưng một Function Declaration phải có tên, do đó sẽ có lỗi:

```js
// Error: Unexpected token (
function() { // <-- JavaScript cannot find function name, meets ( and gives error

  let message = "Hello";

  alert(message); // Hello

}();
```

Chúng ta có thể nói "được thôi, hãy để nó là Function Declaration, hãy thêm tên", nhưng nó sẽ không hoạt động. JavaScript không cho phép Function Declarations được gọi ngay lập tức:

```js
// syntax error because of brackets below
function go() {

}(); // <-- can't call Function Declaration immediately
```

Vì vậy, dấu ngoặc tròn là cần thiết để cho JavaScript biết rằng hàm được tạo trong ngữ cảnh của biểu thức khác và do đó là Function Expression. Nó không cần tên và có thể được gọi ngay lập tức.

Có nhiều cách khác để nói với JavaScript rằng chúng tôi muốn nói đến Function Expression:

```js
// Ways to create IIFE

(function() {
  alert("Brackets around the function");
})();

(function() {
  alert("Brackets around the whole thing");
}());

!function() {
  alert("Bitwise NOT operator starts the expression");
}();

+function() {
  alert("Unary plus starts the expression");
}();
```

Trong tất cả các trường hợp trên, chúng ta khai báo Function Expression và chạy nó ngay lập tức.

## Thu gom dữ liệu rác (Garbage collection)

Lexical Environment objects mà chúng ta đã nói đến phải tuân theo các quy tắc memory management giống như các giá trị thông thường.

- Thông thường, Lexical Environment được dọn sạch sau khi function chạy. Ví dụ:

    ```js
    function f() {
      let value1 = 123;
      let value2 = 456;
    }

    f();
    ```

    Ở đây có hai giá trị về mặt kỹ thuật là các thuộc tính của Lexical Environment. Nhưng sau khi `f()` kết thúc, Lexical Environment trở nên không thể truy cập được, vì vậy nó bị xóa khỏi bộ nhớ.

- ...Nhưng nếu có một hàm lồng nhau vẫn có thể truy cập được sau khi kết thúc `f`, thì tham chiếu `[[Environment]]` của nó giữ outer lexical environment vẫn tồn tại:

    ```js
    function f() {
      let value = 123;

      function g() { alert(value); }

      return g;
    }

    let g = f(); // g is reachable, and keeps the outer lexical environment in memory
    ```

- Xin lưu ý rằng nếu `f()` được gọi nhiều lần và các resulting functions được lưu, thì các đối tượng Lexical Environment tương ứng cũng sẽ được giữ lại trong bộ nhớ. Tất cả 3 trong số chúng trong mã dưới đây:

    ```js
    function f() {
      let value = Math.random();

      return function() { alert(value); };
    }

    // 3 functions in array, every one of them links to Lexical Environment
    // from the corresponding f() run
    //         LE   LE   LE
    let arr = [f(), f(), f()];
    ```

- Một Lexical Environment object chết khi nó không thể truy cập được: khi không còn các hàm lồng nhau nào tham chiếu đến nó. Trong đoạn mã dưới đây, sau khi `g` trở nên không thể truy cập được, `value` cũng được xóa khỏi bộ nhớ;

    ```js
    function f() {
      let value = 123;

      function g() { alert(value); }

      return g;
    }

    let g = f(); // while g is alive
    // there corresponding Lexical Environment lives

    g = null; // ...and now the memory is cleaned up
    ```

### Tối ưu hóa thực tế (Real-life optimizations)

Như chúng ta đã thấy, trên lý thuyết trong khi một hàm còn sống, tất cả các outer variables cũng được giữ lại.

Nhưng trong thực tế, các JavaScript engines cố gắng tối ưu hóa điều đó. Chúng phân tích việc sử dụng biến và nếu dễ dàng thấy rằng một outer variable không được sử dụng -- nó sẽ bị xóa.

**Một tác dụng phụ quan trọng trong V8 (Chrome, Opera) là biến đó sẽ không khả dụng trong việc gỡ lỗi.**

Hãy thử chạy ví dụ bên dưới trong Chrome với Developer Tools mở.

Khi nó tạm dừng, trong console gõ `alert(value)`.

```js
function f() {
  let value = Math.random();

  function g() {
    debugger; // in console: type alert( value ); No such variable!
  }

  return g;
}

let g = f();
g();
```

Như bạn có thể thấy -- không có biến như vậy! Về lý thuyết, nó có thể truy cập được, nhưng engine đã tối ưu hóa nó.

Điều đó có thể dẫn đến các vấn đề gỡ lỗi hài hước. Một trong số chúng -- chúng ta có thể thấy một outer variable cùng tên thay vì dự kiến:

```js
let value = "Surprise!";

function f() {
  let value = "the closest value";

  function g() {
    debugger; // in console: type alert( value ); Surprise!
  }

  return g;
}

let g = f();
g();
```

<br>

> ---

**📌 Hẹn gặp lại!**

Tính năng này của V8 là tốt để biết. Nếu bạn đang gỡ lỗi với Chrome/Opera, sớm muộn bạn cũng sẽ gặp nó.

Đó không phải là một lỗi trong trình gỡ lỗi, mà là một tính năng đặc biệt của V8. Có lẽ nó sẽ được thay đổi một lúc nào đó.
Bạn luôn có thể kiểm tra nó bằng cách chạy các ví dụ trên trang này.

> ---
