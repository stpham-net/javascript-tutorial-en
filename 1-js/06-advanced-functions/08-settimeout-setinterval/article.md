# Scheduling: setTimeout and setInterval

Chúng ta có thể quyết định thực hiện một function không ngay bây giờ, nhưng tại một thời điểm nhất định sau đó. Đó gọi là "lên lịch một cuộc gọi".

Có hai phương thức cho nó:

- `setTimeout` cho phép chạy một hàm sau khoảng thời gian.
- `setInterval` cho phép chạy một hàm thường xuyên với khoảng thời gian giữa các lần chạy.

Các phương thức này không phải là một phần của đặc tả JavaScript. Nhưng hầu hết các môi trường đều có bộ lập lịch nội bộ và cung cấp các phương thức này. Đặc biệt, chúng được hỗ trợ trong tất cả các trình duyệt và Node.js


## setTimeout

Cú pháp:

```js
let timerId = setTimeout(func|code, delay[, arg1, arg2...])
```

Parameters:

**`func|code`**

Hàm hoặc một chuỗi mã để thực thi.
Thông thường, đó là một function. Vì lý do lịch sử, một chuỗi mã có thể được thông qua, nhưng điều đó không được khuyến khích.

**`delay`**

Độ trễ trước khi chạy, tính bằng mili giây (1000 ms = 1 giây).

**`arg1`, `arg2`...**

Đối số cho function (không được hỗ trợ trong IE9-)

Chẳng hạn, mã này gọi `sayHi()` sau một giây:

```js
function sayHi() {
  alert('Hello');
}

setTimeout(sayHi, 1000);
```

Với các đối số:

```js
function sayHi(phrase, who) {
  alert( phrase + ', ' + who );
}

setTimeout(sayHi, 1000, "Hello", "John"); // Hello, John
```

Nếu đối số đầu tiên là một chuỗi, thì JavaScript tạo một hàm từ nó.

Vì vậy, điều này cũng sẽ làm việc:

```js
setTimeout("alert('Hello')", 1000);
```

Nhưng không nên sử dụng các chuỗi, sử dụng các hàm thay vì chúng, như thế này:

```js
setTimeout(() => alert('Hello'), 1000);
```

<br>

> ---

**📌 Truyền một hàm, nhưng không chạy nó**

Các nhà phát triển tập sự đôi khi mắc lỗi bằng cách thêm dấu ngoặc `()` sau hàm:

```js
// wrong!
setTimeout(sayHi(), 1000);
```

Điều đó không làm việc, vì `setTimeout` mong đợi một tham chiếu đến function. Và ở đây `sayHi()` chạy hàm và kết quả *của việc thực thi* được truyền cho `setTimeout`. Trong trường hợp của chúng ta, kết quả của `sayHi()` là `undefined` (hàm trả về không có gì), vì vậy không có gì được lên lịch.

> ---

<br>

### Hủy bỏ với clearTimeout

Một cuộc gọi đến `setTimeout` trả về "định danh bộ định thời gian (timer identifier)" `timerId` mà chúng ta có thể sử dụng để hủy thực thi.

Cú pháp để hủy bỏ:

```js
let timerId = setTimeout(...);
clearTimeout(timerId);
```

Trong mã dưới đây, chúng ta lên lịch cho function và sau đó hủy bỏ nó (chúng ta đã đổi ý). Kết quả là, không có gì xảy ra:

```js
let timerId = setTimeout(() => alert("never happens"), 1000);
alert(timerId); // timer identifier

clearTimeout(timerId);
alert(timerId); // same identifier (doesn't become null after canceling)
```

Như chúng ta có thể thấy từ đầu ra `alert`, trong trình duyệt, định danh bộ đếm thời gian (timer identifier) là một số. Trong các môi trường khác, đây có thể là một cái gì đó khác. Chẳng hạn, Node.JS trả về một đối tượng hẹn giờ (timer object) với các phương thức bổ sung.

Một lần nữa, không có đặc điểm kỹ thuật chung cho các phương thức này, vì vậy điều đó tốt.

Đối với trình duyệt, bộ tính giờ được mô tả trong [phần tính giờ](https://www.w3.org/TR/html5/webappapis.html#timers) của tiêu chuẩn HTML5.

## setInterval

Phương thức `setInterval` có cùng cú pháp với `setTimeout`:

```js
let timerId = setInterval(func|code, delay[, arg1, arg2...])
```

Tất cả các đối số có cùng một ý nghĩa. Nhưng không giống như `setTimeout`, nó chạy hàm không chỉ một lần, mà thường xuyên sau khoảng thời gian nhất định.

Để dừng các cuộc gọi tiếp theo, chúng ta nên gọi `clearInterval(timerId)`.

Ví dụ sau sẽ hiển thị thông báo cứ sau 2 giây. Sau 5 giây, đầu ra bị dừng:

```js
// repeat with the interval of 2 seconds
let timerId = setInterval(() => alert('tick'), 2000);

// after 5 seconds stop
setTimeout(() => { clearInterval(timerId); alert('stop'); }, 5000);
```

<br>

> ---

**📌 Modal windows đóng băng thời gian trong Chrome/Opera/Safari**

Trong trình duyệt IE và Firefox, bộ hẹn giờ bên trong tiếp tục "tích tắc" trong khi hiển thị `alert/confirm/prompt`, nhưng trong Chrome, Opera và Safari, bộ hẹn giờ bên trong sẽ bị "đóng băng".

Vì vậy, nếu bạn chạy mã ở trên và không bỏ qua cửa sổ `alert` trong một thời gian, thì trong Firefox/IE next `alert` sẽ được hiển thị ngay khi bạn thực hiện (2 giây được chuyển từ lệnh gọi trước) và trong Chrome/Opera/Safari -- sau 2 giây nữa (bộ đếm thời gian không tick trong khi `alert`).

> ---

<br>

## SetTimeout đệ quy (Recursive setTimeout)

Có hai cách để chạy một cái gì đó thường xuyên.

Một là `setInterval`. Một cái khác là một `setTimeout` đệ quy, như thế này:

```js
/** instead of:
let timerId = setInterval(() => alert('tick'), 2000);
*/

let timerId = setTimeout(function tick() {
  alert('tick');
  timerId = setTimeout(tick, 2000); // (*)
}, 2000);
```

`SetTimeout` ở trên lên lịch cuộc gọi tiếp theo ngay khi kết thúc cuộc gọi hiện tại `(*)`.

`SetTimeout` đệ quy là một phương thức linh hoạt hơn `setInterval`. Bằng cách này, cuộc gọi tiếp theo có thể được lên lịch khác nhau, tùy thuộc vào kết quả của cuộc gọi hiện tại.

Chẳng hạn, chúng ta cần viết một dịch vụ gửi yêu cầu đến máy chủ cứ sau 5 giây yêu cầu dữ liệu, nhưng trong trường hợp máy chủ bị quá tải, cần tăng khoảng thời gian lên 10, 20, 40 giây...

Đây là mã giả:

```js
let delay = 5000;

let timerId = setTimeout(function request() {
  ...send request...

  if (request failed due to server overload) {
    // increase the interval to the next run
    delay *= 2;
  }

  timerId = setTimeout(request, delay);

}, delay);
```

Và nếu chúng ta thường xuyên có các tác vụ ngốn CPU, thì chúng ta có thể đo thời gian thực hiện và lên kế hoạch cho cuộc gọi tiếp theo sớm hay muộn.

**Đệ quy `setTimeout` đảm bảo độ trễ giữa các lần thực thi, `setInterval` -- thì không.**

Hãy so sánh hai đoạn mã. Cái đầu tiên sử dụng `setInterval`:

```js
let i = 1;
setInterval(function() {
  func(i);
}, 100);
```

Cái thứ hai sử dụng đệ quy `setTimeout`:

```js
let i = 1;
setTimeout(function run() {
  func(i);
  setTimeout(run, 100);
}, 100);
```

Đối với `setInterval`, bộ lập lịch nội bộ sẽ chạy `func(i)` cứ sau 100ms:

![](setinterval-interval.png)

Bạn có để ý không?

**Độ trễ thực sự giữa các lệnh gọi `func` cho `setInterval` nhỏ hơn trong mã!**

Điều đó là bình thường, bởi vì thời gian thực hiện của `func`'s "tiêu tốn" một phần của khoảng thời gian (interval).

Có thể việc thực thi của func`'s hóa ra dài hơn chúng ta mong đợi và mất hơn 100ms.

Trong trường hợp này, engine chờ `func` hoàn thành, sau đó kiểm tra bộ lập lịch và nếu hết thời gian, chạy lại *ngay lập tức*.

Trong trường hợp góc cạnh, nếu hàm luôn thực thi lâu hơn `delay` ms, thì các cuộc gọi sẽ diễn ra mà không cần tạm dừng.

Và đây là hình ảnh cho đệ quy `setTimeout`:

![](settimeout-interval.png)

**`setTimeout` đệ quy đảm bảo độ trễ cố định (ở đây 100ms).**

Đó là bởi vì một cuộc gọi mới được lên kế hoạch vào cuối cuộc gọi trước.

<br>

> ---

**📌 Garbage collection**

Khi một hàm được truyền vào `setInterval/setTimeout`, một tham chiếu bên trong (internal reference) được tạo cho nó và được lưu trong bộ lập lịch. Nó ngăn function bị garbage collected, ngay cả khi không có tham chiếu nào khác.

```js
// the function stays in memory until the scheduler calls it
setTimeout(function() {...}, 100);
```

Đối với `setInterval`, hàm sẽ nằm trong bộ nhớ cho đến khi `clearInterval` được gọi.

Có tác dụng phụ. Một hàm tham chiếu outer lexical environment, vì vậy, trong khi nó sống, các outer variables cũng sống. Chúng có thể chiếm nhiều bộ nhớ hơn chính bản thân function. Vì vậy, khi chúng ta không cần function theo lịch trình nữa, tốt hơn hết là hủy bỏ nó, ngay cả khi nó rất nhỏ.

> ---

<br>

## setTimeout(...,0)

Có một trường hợp sử dụng đặc biệt: `setTimeout(func, 0)`.

Điều này lên lịch thực hiện `func` càng sớm càng tốt. Nhưng trình lập lịch biểu sẽ gọi nó chỉ sau khi mã hiện tại hoàn tất.

Vì vậy, chức năng được lên lịch để chạy "ngay sau" mã hiện tại. Nói cách khác, *không đồng bộ*.

Chẳng hạn, kết quả này xuất ra "Hello", sau đó ngay lập tức "World":

```js
setTimeout(() => alert("World"), 0);

alert("Hello");
```

Dòng đầu tiên "đặt cuộc gọi vào lịch sau 0ms". Nhưng bộ lập lịch sẽ chỉ "kiểm tra lịch" sau khi mã hiện tại hoàn tất, vì vậy `"Hello"` là đầu tiên và `"World"` -- sau nó.

### Phân chia các tác vụ ngốn CPU

Có một mẹo để phân chia các tác vụ ngốn CPU bằng cách sử dụng `setTimeout`.

Ví dụ, tập lệnh tô sáng cú pháp (được sử dụng để tô màu các ví dụ mã trên trang này) khá nặng về CPU. Để làm nổi bật mã, nó thực hiện phân tích, tạo ra nhiều yếu tố màu, thêm chúng vào tài liệu - cho một văn bản lớn cần nhiều. Nó thậm chí có thể khiến trình duyệt bị "treo", điều này không thể chấp nhận được.

Vì vậy, chúng ta có thể chia văn bản dài thành từng mảnh. 100 dòng đầu tiên, sau đó lên kế hoạch cho 100 dòng khác bằng cách sử dụng `setTimeout (..., 0)`, v.v.

Để rõ ràng, hãy lấy một ví dụ đơn giản hơn để xem xét. Chúng ta có một hàm để đếm từ `1` đến `1000000000`.

Nếu bạn chạy nó, CPU sẽ bị treo. Đối với JS phía máy chủ rõ ràng đáng chú ý và nếu bạn đang chạy nó trên trình duyệt, thì hãy thử nhấp vào các nút khác trên trang -- bạn sẽ thấy toàn bộ JavaScript thực sự bị tạm dừng, không có hành động nào khác hoạt động cho đến khi kết thúc.

```js
let i = 0;

let start = Date.now();

function count() {

  // do a heavy job
  for (let j = 0; j < 1e9; j++) {
    i++;
  }

  alert("Done in " + (Date.now() - start) + 'ms');
}

count();
```

Trình duyệt thậm chí có thể hiển thị cảnh báo "tập lệnh mất quá nhiều thời gian" (nhưng hy vọng nó sẽ không xảy ra, vì số lượng không quá lớn).

Hãy phân chia công việc bằng cách sử dụng `setTimeout` lồng nhau:

```js
let i = 0;

let start = Date.now();

function count() {

  // do a piece of the heavy job (*)
  do {
    i++;
  } while (i % 1e6 != 0);

  if (i == 1e9) {
    alert("Done in " + (Date.now() - start) + 'ms');
  } else {
    setTimeout(count, 0); // schedule the new call (**)
  }

}

count();
```

Bây giờ browser UI đã hoạt động đầy đủ trong quá trình "đếm".

Chúng ta thực hiện một phần công việc `(*)`:

1. Lần chạy đầu tiên: `i=1...1000000`.
2. Lần chạy thứ hai: `i=1000001..2000000`.
3. ...và cứ thế, `while` kiểm tra xem `i` có chia đều cho `1000000` không.

Sau đó, cuộc gọi tiếp theo được lên lịch trong `(**)` nếu chúng ta chưa hoàn thành.

Tạm dừng giữa các lần thực thi `Count` cung cấp vừa đủ "hơi thở" cho JavaScript engine để làm việc khác, để phản ứng với các hành động khác của người dùng.

Điều đáng chú ý là cả hai biến thể -- có và không chia công việc theo `setTimeout` -- đều có thể so sánh về tốc độ. Không có nhiều sự khác biệt trong thời gian đếm tổng thể.

Để làm cho chúng gần hơn, hãy cải thiện.

Chúng ta sẽ di chuyển lịch trình vào đầu `count()`:

```js
let i = 0;

let start = Date.now();

function count() {

  // move the scheduling at the beginning
  if (i < 1e9 - 1e6) {
    setTimeout(count, 0); // schedule the new call
  }

  do {
    i++;
  } while (i % 1e6 != 0);

  if (i == 1e9) {
    alert("Done in " + (Date.now() - start) + 'ms');
  }

}

count();
```

Bây giờ khi chúng ta bắt đầu `count()` và biết rằng chúng ta sẽ cần `count()` nhiều hơn, chúng ta lên lịch ngay lập tức, trước khi thực hiện công việc.

Nếu bạn chạy nó, thật dễ dàng để nhận thấy rằng nó mất ít thời gian hơn đáng kể.

<br>

> ---

**📌 Độ trễ tối thiểu của bộ định thời gian lồng nhau trong trình duyệt**

Trong trình duyệt, có một giới hạn về tần suất các bộ định thời gian lồng nhau có thể chạy. [Tiêu chuẩn HTML5](https://www.w3.org/TR/html5/webappapis.html#timers) nói: "sau năm bộ định thời gian lồng nhau, khoảng thời gian buộc phải có ít nhất bốn mili giây.".

Hãy chứng minh ý nghĩa của nó với ví dụ dưới đây. Cuộc gọi `setTimeout` trong đó tự lập lại lịch trình sau `0ms`. Mỗi cuộc gọi ghi nhớ thời gian thực từ cuộc gọi trước trong mảng `times`. Sự chậm trễ thực sự trông như thế nào? Hãy xem:

```js
let start = Date.now();
let times = [];

setTimeout(function run() {
  times.push(Date.now() - start); // remember delay from the previous call

  if (start + 100 < Date.now()) alert(times); // show the delays after 100ms
  else setTimeout(run, 0); // else re-schedule
}, 0);

// an example of the output:
// 1,1,1,1,9,15,20,24,30,35,40,45,50,55,59,64,70,75,80,85,90,95,100
```

Bộ hẹn giờ đầu tiên chạy ngay lập tức (giống như được ghi trong thông số kỹ thuật), và sau đó độ trễ xuất hiện và chúng ta thấy `9, 15, 20, 24...`.

Hạn chế đó xuất phát từ thời cổ đại và nhiều kịch bản dựa vào nó, vì vậy nó tồn tại vì lý do lịch sử.

Đối với JavaScript phía máy chủ, giới hạn đó không tồn tại và tồn tại các cách khác để lên lịch cho một công việc không đồng bộ ngay lập tức, như [process.nextTick](https://nodejs.org/api/process.html) và [setImmediate](https://nodejs.org/api/timers.html) cho Node.JS. Vì vậy, khái niệm này chỉ dành riêng cho trình duyệt.

> ---

<br>

### Cho phép trình duyệt hiển thị

Một lợi ích khác cho các tập lệnh trong trình duyệt là chúng có thể hiển thị progress bar hoặc thứ gì đó cho người dùng. Đó là bởi vì trình duyệt thường thực hiện tất cả "sơn lại" sau khi tập lệnh hoàn tất.

Vì vậy, nếu chúng ta thực hiện một function lớn duy nhất thì ngay cả khi nó thay đổi điều gì đó, những thay đổi sẽ không được phản ánh trong tài liệu cho đến khi hoàn thành.

Đây là bản demo:

```html
<div id="progress"></div>

<script>
  let i = 0;

  function count() {
    for (let j = 0; j < 1e6; j++) {
      i++;
      // put the current i into the <div>
      // (we'll talk more about innerHTML in the specific chapter, should be obvious here)
      progress.innerHTML = i;
    }
  }

  count();
</script>
```

Nếu bạn chạy nó, các thay đổi thành `i` sẽ hiển thị sau khi toàn bộ số đếm kết thúc.

Và nếu chúng ta sử dụng `setTimeout` để chia nó thành nhiều phần thì các thay đổi sẽ được áp dụng ở giữa các lần chạy, vì vậy điều này có vẻ tốt hơn:

```html
<div id="progress"></div>

<script>
  let i = 0;

  function count() {

    // do a piece of the heavy job (*)
    do {
      i++;
      progress.innerHTML = i;
    } while (i % 1e3 != 0);

    if (i < 1e9) {
      setTimeout(count, 0);
    }

  }

  count();
</script>
```

Bây giờ `<div>` hiển thị các giá trị tăng dần của `i`.

## Tóm lược

- Các phương thức `setInterval(func, delay, ...args)` và `setTimeout(func, delay, ...args)` cho phép chạy `func` thường xuyên/một lần sau `delay` mili giây.
- Để hủy thực thi, chúng ta nên gọi `clearInterval/clearTimeout` với giá trị được trả về bởi `setInterval/setTimeout`.
- Các lệnh gọi `setTimeout` lồng nhau là một sự thay thế linh hoạt hơn cho `setInterval`. Ngoài ra chúng có thể đảm bảo thời gian tối thiểu *giữa* các lần thực hiện.
- Zero-timeout scheduling `setTimeout(...,0)` được sử dụng để lên lịch cuộc gọi "càng sớm càng tốt, nhưng sau khi mã hiện tại hoàn tất".

Một số trường hợp sử dụng `setTimeout(...,0)`:
- Để phân chia các tác vụ ngốn CPU thành nhiều phần, để tập lệnh không bị "treo"
- Để cho trình duyệt làm một cái gì đó khác trong khi quá trình đang diễn ra (vẽ progress bar).

Xin lưu ý rằng tất cả các phương pháp lập lịch không *đảm bảo* độ trễ chính xác. Chúng ta không nên dựa vào điều đó trong mã theo lịch trình.

Ví dụ: bộ hẹn giờ trong trình duyệt có thể chậm lại vì nhiều lý do:
- CPU bị quá tải.
- Tab trình duyệt ở chế độ nền.
- Máy tính xách tay đang dùng pin.

Tất cả có thể tăng độ phân giải hẹn giờ tối thiểu (độ trễ tối thiểu) lên 300ms hoặc thậm chí 1000ms tùy thuộc vào trình duyệt và cài đặt.
