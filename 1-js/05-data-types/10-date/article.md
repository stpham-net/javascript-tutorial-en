# Date and time

Chúng ta hãy gặp một built-in object mới: [Date](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date). Nó lưu trữ ngày, giờ và cung cấp các phương thức để quản lý date/time.

Chẳng hạn, chúng ta có thể sử dụng nó để lưu trữ thời gian tạo/sửa đổi, để đo thời gian, hoặc chỉ để in ra ngày hiện tại.

## Creation

Để tạo một đối tượng `Date` mới gọi `new Date()` với một trong các đối số sau:

**`new Date()`**

Không có đối số -- tạo một đối tượng `Date` cho ngày và giờ hiện tại:

```js
      let now = new Date();
      alert( now ); // shows current date/time
```

**`new Date(milliseconds)`**

Tạo một đối tượng `Date` với thời gian bằng số mili giây (1/1000 giây) được truyền sau ngày 1 tháng 1 năm 1970 UTC+0.

```js
      // 0 means 01.01.1970 UTC+0
      let Jan01_1970 = new Date(0);
      alert( Jan01_1970 );

      // now add 24 hours, get 02.01.1970 UTC+0
      let Jan02_1970 = new Date(24 * 3600 * 1000);
      alert( Jan02_1970 );
```

Số mili giây đã trôi qua kể từ đầu năm 1970 được gọi là *timestamp*.

Đó là một đại diện số nhẹ (lightweight numeric) của một ngày. Chúng ta luôn có thể tạo một ngày từ dấu thời gian (timestamp) bằng cách sử dụng `new Date(timestamp)` và chuyển đổi đối tượng `Date` hiện tại thành dấu thời gian (timestamp) bằng phương thức `date.getTime()` (xem bên dưới).

**`new Date(datestring)`**

Nếu có một đối số duy nhất và đó là một chuỗi, thì nó được phân tích cú pháp bằng thuật toán `Date.parse` (xem bên dưới).

```js
      let date = new Date("2017-01-26");
      alert(date); // Thu Jan 26 2017 ...
```

**`new Date(year, month, date, hours, minutes, seconds, ms)`**

Tạo ngày với các thành phần nhất định theo local time zone. Chỉ có hai đối số đầu tiên là bắt buộc.

Chú thích:

- The `year` phải có 4 chữ số: `2013` không sao, `98` thì không được.
- The `month` bắt đầu bằng `0` (tháng 1), tối đa `11` (tháng 12).
- The `date` tham số thực sự là ngày trong tháng, nếu vắng mặt thì `1` được giả định.
- Nếu `hours/minutes/seconds/ms` vắng mặt, chúng được coi là bằng `0`.

Ví dụ:

```js
      new Date(2011, 0, 1, 0, 0, 0, 0); // // 1 Jan 2011, 00:00:00
      new Date(2011, 0, 1); // the same, hours etc are 0 by default
```

Độ chính xác tối thiểu là 1 ms (1/1000 giây):

```js
      let date = new Date(2011, 0, 1, 2, 3, 4, 567);
      alert( date ); // 1.01.2011, 02:03:04.567
```

## Access date components

Có nhiều phương thức để truy cập năm, tháng, v.v. từ đối tượng `Date`. Nhưng chúng có thể dễ dàng được ghi nhớ khi phân loại.

**[getFullYear()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date/getFullYear)**

Lấy năm (4 chữ số)

**[getMonth()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date/getMonth)**

Lấy tháng, **từ 0 đến 11**.

**[getDate()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date/getDate)**

Lấy ngày trong tháng, từ 1 đến 31, tên của phương thức trông hơi lạ.

**[getHours()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date/getHours), [getMinutes()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date/getMinutes), [getSeconds()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date/getSeconds), [getMilliseconds()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date/getMilliseconds)**

Lấy các thành phần thời gian tương ứng.

<br>

> ---

**📌 Không phải `getYear()`, mà là `getFullYear()`**

Nhiều JavaScript engines triển khai một phương thức không chuẩn `getYear()`. Phương thức này không được chấp nhận. Nó trả về năm 2 chữ số đôi khi. Xin đừng bao giờ sử dụng nó. Có `getFullYear()` cho năm.

> ---

<br>

Ngoài ra, chúng ta có thể nhận được một ngày trong tuần:

**[getDay()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date/getDay)**

Lấy ngày trong tuần, từ `0` (Chủ nhật) đến `6` (Thứ bảy). Ngày đầu tiên luôn là Chủ nhật, ở một số quốc gia không như vậy, nhưng không thể thay đổi.

**Tất cả các phương thức trên trả về các thành phần liên quan đến múi giờ địa phương (local time zone).**

Ngoài ra còn có các đối tác UTC của chúng (UTC-counterparts), mà trả về ngày, tháng, năm, v.v. theo múi giờ UTC+0: [getUTCFullYear()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date/getUTCFullYear), [getUTCMonth()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date/getUTCMonth), [getUTCDay()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date/getUTCDay). Chỉ cần chèn `"UTC"` ngay sau `"get"`.

Nếu múi giờ địa phương của bạn bị thay đổi so với UTC, thì mã bên dưới hiển thị các giờ khác nhau:

```js
      // current date
      let date = new Date();

      // the hour in your current time zone
      alert( date.getHours() );

      // the hour in UTC+0 time zone (London time without daylight savings)
      alert( date.getUTCHours() );
```

Bên cạnh các phương thức đã cho, có hai phương thức đặc biệt không có biến thể UTC:

**[getTime()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date/getTime)**

Trả về dấu thời gian (timestamp) cho date -- một số mili giây được tính từ ngày 1 tháng 1 năm 1970 UTC+0.

**[getTimezoneOffset()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date/getTimezoneOffset)**

Trả về sự khác biệt giữa local time zone và UTC, tính bằng phút:

```js
      // if you are in timezone UTC-1, outputs 60
      // if you are in timezone UTC+3, outputs -180
      alert( new Date().getTimezoneOffset() );
```

## Setting date components

Các phương thức sau đây cho phép đặt các thành phần ngày/giờ:

- [`setFullYear(year [, month, date])`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date/setFullYear)
- [`setMonth(month [, date])`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date/setMonth)
- [`setDate(date)`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date/setDate)
- [`setHours(hour [, min, sec, ms])`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date/setHours)
- [`setMinutes(min [, sec, ms])`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date/setMinutes)
- [`setSeconds(sec [, ms])`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date/setSeconds)
- [`setMilliseconds(ms)`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date/setMilliseconds)
- [`setTime(milliseconds)`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date/setTime) (sets the whole date by milliseconds since 01.01.1970 UTC)

Mỗi một trong số chúng ngoại trừ `setTime()` đều có biến thể UTC, ví dụ: `setUTCHours()`.

Như chúng ta có thể thấy, một số phương thức có thể thiết lập nhiều thành phần cùng một lúc, ví dụ `setHours`. Các thành phần không được đề cập không bị sửa đổi.

Ví dụ:

```js
      let today = new Date();

      today.setHours(0);
      alert(today); // still today, but the hour is changed to 0

      today.setHours(0, 0, 0, 0);
      alert(today); // still today, now 00:00:00 sharp.
```

## Autocorrection

The *autocorrection* là một tính năng rất tiện dụng của các đối tượng `Date`. Chúng ta có thể đặt các giá trị ngoài phạm vi (out-of-range) và nó sẽ tự động điều chỉnh (auto-adjust).

Ví dụ:

```js
      let date = new Date(2013, 0, 32); // 32 Jan 2013 ?!?
      alert(date); // ...is 1st Feb 2013!
```

Các thành phần ngày ngoài phạm vi được phân phối tự động.

Giả sử chúng ta cần tăng ngày "28 tháng 2 năm 2016" thêm 2 ngày. Nó có thể là "2 tháng 3" hoặc "1 tháng 3" trong trường hợp năm nhuận. Chúng ta không cần phải suy nghĩ về nó. Chỉ cần thêm 2 ngày. Đối tượng `Date` sẽ làm phần còn lại:

```js
      let date = new Date(2016, 1, 28);
      date.setDate(date.getDate() + 2);

      alert( date ); // 1 Mar 2016
```

Tính năng đó thường được sử dụng để lấy date sau khoảng thời gian nhất định. Chẳng hạn, hãy lấy date "70 giây sau":

```js
      let date = new Date();
      date.setSeconds(date.getSeconds() + 70);

      alert( date ); // shows the correct date
```

Chúng ta cũng có thể đặt giá trị 0 hoặc thậm chí âm. Ví dụ:

```js
      let date = new Date(2016, 0, 2); // 2 Jan 2016

      date.setDate(1); // set day 1 of month
      alert( date );

      date.setDate(0); // min day is 1, so the last day of the previous month is assumed
      alert( date ); // 31 Dec 2015
```

## Date to number, date diff

Khi một đối tượng `Date` được chuyển đổi thành số, nó sẽ trở thành dấu thời gian giống như `date.getTime()`:

```js
      let date = new Date();
      alert(+date); // the number of milliseconds, same as date.getTime()
```

Tác dụng phụ quan trọng: ngày có thể được trừ, kết quả là sự khác biệt của chúng trong ms.

Điều đó có thể được sử dụng để đo thời gian:

```js
      let start = new Date(); // start counting

      // do the job
      for (let i = 0; i < 100000; i++) {
        let doSomething = i * i * i;
      }

      let end = new Date(); // done

      alert( `The loop took ${end - start} ms` );
```

## Date.now()

Nếu chúng ta chỉ muốn đo lường sự khác biệt, chúng ta không cần đối tượng `Date`.

Có một phương thức đặc biệt `Date.now()` trả về dấu thời gian hiện tại.

Nó tương đương về mặt ngữ nghĩa với `new Date().getTime()`, nhưng nó không tạo ra một đối tượng `Date` trung gian. Vì vậy, nó nhanh hơn và không gây áp lực lên garbage collection.

Nó được sử dụng chủ yếu để thuận tiện hoặc khi vấn đề hiệu suất, như trong các trò chơi trong JavaScript hoặc các ứng dụng chuyên dụng khác.

Vì vậy, điều này có lẽ tốt hơn:

```js
      let start = Date.now(); // milliseconds count from 1 Jan 1970

      // do the job
      for (let i = 0; i < 100000; i++) {
        let doSomething = i * i * i;
      }

      let end = Date.now(); // done

      alert( `The loop took ${end - start} ms` ); // subtract numbers, not dates
```

## Benchmarking

Nếu chúng ta muốn một điểm chuẩn đáng tin cậy của CPU-hungry function, chúng ta nên cẩn thận.

Chẳng hạn, hãy đo hai hàm tính toán sự khác biệt giữa hai ngày: cái nào nhanh hơn?

```js
      // we have date1 and date2, which function faster returns their difference in ms?
      function diffSubtract(date1, date2) {
        return date2 - date1;
      }

      // or
      function diffGetTime(date1, date2) {
        return date2.getTime() - date1.getTime();
      }
```

Hai cái này thực hiện chính xác cùng một thứ, nhưng một trong số chúng sử dụng một `date.getTime()` rõ ràng để lấy ngày tính bằng ms và cái còn lại dựa vào biến đổi ngày thành số (date-to-number transform). Kết quả của chúng luôn giống nhau.

Vậy, cái nào nhanh hơn?

Ý tưởng đầu tiên có thể là chạy chúng nhiều lần liên tiếp và đo chênh lệch thời gian. Đối với trường hợp của chúng ta, các chức năng rất đơn giản, vì vậy chúng ta phải thực hiện khoảng 100000 lần.

Hãy đo:

```js
      function diffSubtract(date1, date2) {
        return date2 - date1;
      }

      function diffGetTime(date1, date2) {
        return date2.getTime() - date1.getTime();
      }

      function bench(f) {
        let date1 = new Date(0);
        let date2 = new Date();

        let start = Date.now();
        for (let i = 0; i < 100000; i++) f(date1, date2);
        return Date.now() - start;
      }

      alert( 'Time of diffSubtract: ' + bench(diffSubtract) + 'ms' );
      alert( 'Time of diffGetTime: ' + bench(diffGetTime) + 'ms' );
```

Ồ Sử dụng `getTime()` nhanh hơn rất nhiều! Đó là bởi vì không có chuyển đổi kiểu, engines sẽ dễ dàng tối ưu hóa hơn nhiều.

Được rồi, chúng ta có một cái gì đó. Nhưng đó chưa phải là một benchmark tốt.

Hãy tưởng tượng rằng tại thời điểm chạy `bench(diffSubtract)` CPU đang làm việc gì đó song song và nó đang lấy tài nguyên. Và tại thời điểm chạy `bench(diffGetTime)`, công việc đã hoàn thành.

Một kịch bản khá thực tế cho một hệ điều hành multi-process hiện đại.

Do đó, benchmark đầu tiên sẽ có ít tài nguyên CPU hơn lần thứ hai. Điều đó có thể dẫn đến kết quả sai.

**Để có benchmarking đáng tin cậy hơn, toàn bộ gói benchmarks phải được chạy lại nhiều lần.**

Đây là ví dụ mã:

```js
      function diffSubtract(date1, date2) {
        return date2 - date1;
      }

      function diffGetTime(date1, date2) {
        return date2.getTime() - date1.getTime();
      }

      function bench(f) {
        let date1 = new Date(0);
        let date2 = new Date();

        let start = Date.now();
        for (let i = 0; i < 100000; i++) f(date1, date2);
        return Date.now() - start;
      }

      let time1 = 0;
      let time2 = 0;

      // run bench(upperSlice) and bench(upperLoop) each 10 times alternating
      for (let i = 0; i < 10; i++) {
        time1 += bench(diffSubtract);
        time2 += bench(diffGetTime);
      }

      alert( 'Total time for diffSubtract: ' + time1 );
      alert( 'Total time for diffGetTime: ' + time2 );
```

Các JavaScript engines hiện đại bắt đầu áp dụng tối ưu hóa nâng cao chỉ cho "hot code" thực thi nhiều lần (không cần tối ưu hóa các thứ hiếm khi được thực thi). Vì vậy, trong ví dụ trên, các lần thực hiện đầu tiên không được tối ưu hóa tốt. Chúng ta có thể muốn thêm một hoạt động tăng nhiệt (heat-up run):

```js
      // added for "heating up" prior to the main loop
      bench(diffSubtract);
      bench(diffGetTime);

      // now benchmark
      for (let i = 0; i < 10; i++) {
        time1 += bench(diffSubtract);
        time2 += bench(diffGetTime);
      }
```

<br>

> ---

**📌 Cẩn thận khi thực hiện microbenchmarking**

Các JavaScript engines hiện đại thực hiện nhiều tối ưu hóa. Chúng có thể điều chỉnh kết quả của "thử nghiệm nhân tạo" so với "sử dụng thông thường", đặc biệt là khi chúng ta benchmark một cái gì đó rất nhỏ. Vì vậy, nếu bạn nghiêm túc muốn hiểu hiệu suất, thì hãy nghiên cứu cách hoạt động của JavaScript engine. Và sau đó có lẽ bạn sẽ không cần microbenchmarks.

Có thể tìm thấy gói bài viết tuyệt vời về V8 tại <http://mrale.ph>.

> ---

<br>

## Date.parse from a string

Phương thức [Date.parse(str)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date/parse) có thể đọc date từ chuỗi.

Định dạng chuỗi phải là: `YYYY-MM-DDTHH:mm:ss.sssZ`, trong đó:

- `YYYY-MM-DD` -- là date: year-month-day.
- Ký tự `"T"` được sử dụng làm dấu phân cách.
- `HH:mm:ss.sss` -- là thời gian: giờ, phút, giây và mili giây.
- Phần `'Z'` tùy chọn biểu thị múi giờ theo định dạng `+-hh:mm`. Một chữ cái `Z` có nghĩa là UTC+0.

Các biến thể ngắn hơn cũng có thể, như `YYYY-MM-DD` hoặc` YYYY-MM` hoặc thậm chí `YYYY`.

Cuộc gọi tới `Date.parse(str)` phân tích chuỗi theo định dạng đã cho và trả về dấu thời gian (số mili giây từ ngày 1 tháng 1 năm 1970 UTC+0). Nếu định dạng không hợp lệ, trả về `NaN`.

Ví dụ:

```js
      let ms = Date.parse('2012-01-26T13:51:50.417-07:00');

      alert(ms); // 1327611110417  (timestamp)
```

Chúng ta có thể ngay lập tức tạo một đối tượng `new Date` từ dấu thời gian:

```js
      let date = new Date( Date.parse('2012-01-26T13:51:50.417-07:00') );

      alert(date);  
```

## Tóm lược

- Ngày và giờ trong JavaScript được biểu thị bằng đối tượng [Date](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date). Chúng ta không thể tạo "chỉ ngày" hoặc "chỉ thời gian": Các đối tượng `Date` luôn mang cả hai.
- Tháng được tính từ zero (vâng, tháng 1 là tháng zero).
- Các ngày trong tuần trong `getDay()` cũng được tính từ 0 (đó là Chủ nhật).
- `Date` auto-corrects khi các thành phần ngoài phạm vi (out-of-range) được đặt. Tốt cho việc thêm/trừ ngày/tháng/giờ.
- Date có thể được trừ, và sự khác biệt của chúng tính bằng mili giây. Đó là bởi vì một `Date` trở thành timestamp khi được chuyển đổi thành một số.
- Sử dụng `Date.now()` để lấy dấu thời gian hiện tại nhanh.

Lưu ý rằng không giống như nhiều hệ thống khác, dấu thời gian trong JavaScript là tính bằng mili giây chứ không phải tính bằng giây.

Ngoài ra, đôi khi chúng ta cần đo thời gian chính xác hơn. Bản thân JavaScript không có cách đo thời gian tính bằng micro giây (1 phần triệu giây), nhưng hầu hết các môi trường đều cung cấp nó. Chẳng hạn, trình duyệt có [performance.now()](https://developer.mozilla.org/en-US/docs/Web/API/Performance/now) cung cấp số mili giây từ khi bắt đầu tải trang với độ chính xác micro giây (3 chữ số sau the point):

```js
alert(`Loading started ${performance.now()}ms ago`);
// Something like: "Loading started 34731.26000000001ms ago"
// .26 is microseconds (260 microseconds)
// more than 3 digits after the decimal point are precision errors, but only the first 3 are correct
```

Node.JS có mô đun `microtime` và các cách khác. Về mặt kỹ thuật, bất kỳ thiết bị và environment nào cũng cho phép lấy độ chính xác cao hơn, đó chỉ là không có trong `Date`.
