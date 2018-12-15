# Gỡ lỗi trong Chrome

Trước khi viết mã phức tạp hơn, hãy nói về gỡ lỗi.

Tất cả các trình duyệt hiện đại và hầu hết các môi trường khác đều hỗ trợ "gỡ lỗi" -- một giao diện người dùng đặc biệt trong các công cụ dành cho nhà phát triển giúp việc tìm kiếm và sửa lỗi dễ dàng hơn nhiều.

Chúng tôi sẽ sử dụng Chrome ở đây, vì có lẽ nó là giàu tính năng nhất trong khía cạnh này.

## Cửa sổ "nguồn" (The "sources" pane)

Phiên bản Chrome của bạn có thể trông hơi khác một chút, nhưng vẫn rõ ràng những gì ở đó.

- Mở [example page](debugging/index.html) trong Chrome.
- Bật công cụ dành cho nhà phát triển với `F12` (Mac: `Cmd+Opt+I`).
- Chọn khung `sources`.

Đây là những gì bạn nên xem nếu bạn đang làm nó lần đầu tiên:

![](chrome-open-sources.png)

Nút toggler ![](button-1.png) mở tab với các tệp.

Hãy nhấp vào nó và chọn `index.html` và sau đó `hello.js` trong chế độ xem dạng cây. Đây là những gì sẽ hiển thị:

![](chrome-tab.png)

Ở đây chúng ta có thể thấy ba khu vực:

1. Vùng **Resources zone** liệt kê HTML, JavaScript, CSS và các tệp khác, bao gồm các hình ảnh được đính kèm vào trang. Tiện ích mở rộng Chrome cũng có thể xuất hiện ở đây.
2. Vùng **Source zone** hiển thị mã nguồn.
3. **Information and control zone** là để gỡ lỗi, chúng ta sẽ sớm khám phá nó.

Bây giờ bạn có thể nhấp vào cùng một toggler ![](button-2.png) một lần nữa để ẩn danh sách tài nguyên (resource) và cung cấp cho mã một số khoảng trống.

## Bảng điều khiển (Console)

Nếu chúng ta nhấn `Esc`, thì bảng điều khiển sẽ mở bên dưới. Chúng ta có thể gõ các lệnh ở đó và nhấn `Enter` để thực thi.

Sau khi một câu lệnh được thực thi, kết quả của nó được hiển thị bên dưới.

Ví dụ, ở đây `1+2` kết quả là `3` và `hello("debugger")` không trả về gì, vì vậy kết quả là `undefined`:

![](chrome-sources-console.png)

## Điểm dừng (Breakpoints)

Hãy xem xét những gì đang diễn ra trong mã của [example page](debugging/index.html). Trong `hello.js`, nhấp vào số dòng `4`. Vâng, ngay trên chữ số `4`, không phải trên mã.

Xin chúc mừng! Bạn đã thiết lập một điểm dừng. Vui lòng nhấp vào số cho dòng `8`.

Nó sẽ trông như thế này (màu xanh là nơi bạn nên nhấp):

![](chrome-sources-breakpoint.png)

Một *điểm dừng (breakpoint)* là một điểm mã trong đó trình gỡ lỗi sẽ tự động tạm dừng thực thi JavaScript.

Trong khi mã bị tạm dừng, chúng ta có thể kiểm tra các biến hiện tại, thực thi các lệnh trong bàn điều khiển, v.v. Nói cách khác, chúng ta có thể gỡ lỗi nó.

Chúng ta luôn có thể tìm thấy một danh sách các điểm dừng trong khung bên phải. Điều đó hữu ích khi chúng ta có nhiều điểm dừng trong các tệp khác nhau. Nó cho phép:
- Nhanh chóng nhảy đến điểm dừng trong mã (bằng cách nhấp vào nó trong khung bên phải).
- Tạm thời vô hiệu hóa điểm dừng bằng cách bỏ chọn nó.
- Xóa điểm dừng bằng cách nhấp chuột phải và chọn Xóa.
- ...Và như vậy.

<br>

> ---

**📌 Điểm dừng có điều kiện (Conditional breakpoints)**

*Nhấp chuột phải* vào số dòng cho phép tạo điểm dừng *có điều kiện*. Nó chỉ kích hoạt khi biểu thức đã cho là đúng.

Điều đó rất hữu ích khi chúng ta chỉ cần dừng lại cho một giá trị biến nhất định hoặc cho các tham số function nhất định.

> ---

<br>

## Lệnh gỡ lỗi (Debugger command)

Chúng ta cũng có thể tạm dừng mã bằng cách sử dụng lệnh `debugger`, như thế này:

```js
      function hello(name) {
        let phrase = `Hello, ${name}!`;

        debugger;  // <-- the debugger stops here

        say(phrase);
      }
```

Điều đó rất thuận tiện khi chúng ta ở trong trình chỉnh sửa mã và không muốn chuyển sang trình duyệt và tìm kiếm tập lệnh trong các công cụ dành cho nhà phát triển (developer tools) để đặt điểm dừng.

## Tạm dừng và nhìn xung quanh (Pause and look around)

Trong ví dụ của chúng tôi, `hello()` được gọi trong quá trình tải trang, vì vậy cách dễ nhất để kích hoạt trình gỡ lỗi là tải lại trang. Vì vậy, hãy nhấn `F5` (Windows, Linux) hoặc `Cmd+R` (Mac).

Khi điểm dừng được đặt, việc thực thi tạm dừng ở dòng thứ 4:

![](chrome-sources-debugger-pause.png)

Vui lòng mở các thông tin thả xuống (informational dropdowns) bên phải (được dán nhãn bằng mũi tên (labeled with arrows)). Chúng cho phép bạn kiểm tra trạng thái mã hiện tại:

1. **`Watch` -- hiển thị các giá trị hiện tại cho bất kỳ biểu thức nào.**

    Bạn có thể nhấp vào dấu cộng `+` và nhập một biểu thức. Trình gỡ lỗi sẽ hiển thị giá trị của nó bất cứ lúc nào, tự động tính toán lại nó trong quá trình thực thi.

2. **`Call Stack` -- hiển thị chuỗi cuộc gọi lồng nhau (nested calls chain).**

    Tại thời điểm hiện tại, trình gỡ lỗi đang ở trong cuộc gọi `hello()`, được gọi bởi một tập lệnh trong `index.html` (không có function nào ở đó, vì vậy nó được gọi là "anonymous").

    Nếu bạn bấm vào một mục ngăn xếp (stack item), trình gỡ lỗi nhảy đến mã tương ứng và tất cả các biến của nó cũng có thể được kiểm tra.
    
3. **`Scope` -- các biến hiện tại.**

    `Local` hiển thị các biến function cục bộ (local function variables). Bạn cũng có thể thấy các giá trị của chúng được tô sáng ngay trên source.

    `Global` có các biến global (bên ngoài bất kỳ các hàm).

    Ngoài ra còn có từ khóa `this` mà chúng ta chưa nghiên cứu, nhưng chúng ta sẽ sớm làm điều đó.

## Truy tìm thực thi (Tracing the execution)

Bây giờ là lúc để *theo dõi (trace)* kịch bản.

Có các nút cho nó ở trên cùng của khung bên phải. Hãy tiến hành với chúng.

**![](button-3.png) -- tiếp tục thực thi, phím nóng `F8`.**

Tiếp tục thực thi. Nếu không có điểm dừng bổ sung, thì việc thực thi sẽ tiếp tục và trình gỡ lỗi mất quyền kiểm soát.

Đây là những gì chúng ta có thể thấy sau khi nhấp vào nó:

![](chrome-sources-debugger-trace-1.png)

Việc thực thi đã được tiếp tục, đạt đến một điểm dừng khác bên trong `say()` và dừng lại ở đó. Hãy nhìn vào "ngăn xếp cuộc gọi (Call stack)" ở bên phải. Nó đã tăng thêm một cuộc gọi. Bây giờ chúng ta đang ở trong `say()`.

**![](button-4.png) -- thực hiện một bước (chạy lệnh tiếp theo), nhưng *không đi vào function*, phím nóng `F10`.**

Nếu chúng ta nhấp vào nó bây giờ, `alert` sẽ được hiển thị. Điều quan trọng là `alert` có thể là bất kỳ chức năng nào, việc thực thi "bước qua nó (steps over it)", bỏ qua các hàm bên trong.

**![](button-5.png) -- tạo một bước, phím nóng `F11`.**

Giống như cái trước, nhưng "bước vào" các hàm lồng nhau. Nhấp vào đây sẽ bước qua toàn bộ các hành động của script từng bước một.

**![](button-6.png) -- tiếp tục thực hiện cho đến khi kết thúc function hiện tại, phím nóng `Shift+F11`.**

Việc thực thi sẽ dừng lại ở dòng cuối cùng của hàm hiện tại. Thật tiện lợi khi chúng ta vô tình tham gia một cuộc gọi lồng nhau bằng cách sử dụng ![](button-5.png), nhưng nó không làm chúng ta quan tâm và chúng ta muốn tiếp tục kết thúc càng sớm càng tốt.

**![](button-7.png) -- bật/tắt tất cả các điểm dừng.**

Nút đó không di chuyển thực thi. Chỉ là một loạt bật/tắt cho các điểm dừng.

**![](button-8.png) -- bật/tắt tự động tạm dừng trong trường hợp có lỗi.**

Khi được bật và các công cụ dành cho nhà phát triển (developer tools) được mở, một lỗi tập lệnh sẽ tự động tạm dừng thực thi. Sau đó chúng ta có thể phân tích các biến để xem những gì đã sai. Vì vậy, nếu tập lệnh của chúng ta bị lỗi, chúng ta có thể mở trình gỡ lỗi, bật tùy chọn này và tải lại trang để xem nó chết ở đâu và bối cảnh tại thời điểm đó.

> ---

**📌 Continue to here**

Nhấp chuột phải vào một dòng mã sẽ mở menu ngữ cảnh với một tùy chọn tuyệt vời có tên "Continue to here".

Điều đó thật tiện lợi khi chúng ta muốn tiến lên nhiều bước về phía trước, nhưng chúng ta quá lười biếng để thiết lập một điểm dừng.

> ---

<br>

## Ghi nhật ký (Logging)

Để xuất cái gì đó ra console, có function `console.log`.

Chẳng hạn, điều này xuất các giá trị từ `0` đến `4` sang console:

```js
      // open console to see
      for (let i = 0; i < 5; i++) {
        console.log("value", i);
      }
```

Người dùng thông thường không thấy đầu ra đó, nó nằm trong bảng điều khiển. Để xem nó, hãy mở tab Console của các công cụ dành cho nhà phát triển hoặc nhấn `Esc` trong khi ở một tab khác: để mở giao diện điều khiển ở phía dưới.

Nếu chúng ta có đủ logging trong mã của chúng ta, thì chúng ta có thể thấy những gì đang diễn ra từ các bản ghi mà không cần trình gỡ lỗi.

## Tóm lược

Như chúng ta có thể thấy, có ba cách chính để tạm dừng một tập lệnh:
1. Một điểm dừng (breakpoint).
2. Các câu lệnh `debugger'.
3. Một lỗi (nếu công cụ dev đang mở và nút ![](button-8.png) đang "bật")

Sau đó, chúng ta có thể kiểm tra các biến và bước tiếp (step on) để xem thực thi sai ở đâu.

Có nhiều tùy chọn hơn trong các công cụ dành cho nhà phát triển hơn được đề cập ở đây. Hướng dẫn đầy đủ có tại <https://developers.google.com/web/tools/chrome-devtools>.

Thông tin từ chương này là đủ để bắt đầu gỡ lỗi, nhưng sau đó, đặc biệt nếu bạn dùng nhiều công cụ trình duyệt, vui lòng đến đó và xem qua các khả năng nâng cao hơn của các công cụ dành cho nhà phát triển.

Ồ, và bạn cũng có thể nhấp vào nhiều nơi khác nhau của các dev tools và chỉ cần xem những gì hiển thị. Đó có lẽ là con đường nhanh nhất để học các công cụ dev. Đừng quên nhấp chuột phải cũng tốt!
1 result is available, use up and down arrow keys to navigate.
