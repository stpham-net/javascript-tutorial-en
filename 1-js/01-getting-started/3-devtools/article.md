# Developer console

Mã dễ bị lỗi. Bạn hoàn toàn có thể mắc lỗi ... Ồ, tôi đang nói về cái gì vậy? Bạn *hoàn toàn* sẽ mắc lỗi, ít nhất là nếu bạn là một con người, không phải là [robot](https://en.wikipedia.org/wiki/Bender_(Futurama)).

Nhưng trong trình duyệt, người dùng không thấy lỗi theo mặc định. Vì vậy, nếu có lỗi xảy ra trong kịch bản, chúng ta sẽ không thấy những gì bị hỏng và không thể sửa nó.

Để xem lỗi và nhận được nhiều thông tin hữu ích khác về tập lệnh (scripts), "công cụ dành cho nhà phát triển (developer tools)" đã được nhúng trong trình duyệt.

Hầu hết các nhà phát triển nghiêng về Chrome hoặc Firefox để phát triển vì những trình duyệt đó có các công cụ dành cho nhà phát triển tốt nhất. Các trình duyệt khác cũng cung cấp các công cụ dành cho nhà phát triển, đôi khi có các tính năng đặc biệt, nhưng thường để "bắt kịp" với Chrome hoặc Firefox. Vì vậy, hầu hết các nhà phát triển đều có trình duyệt "yêu thích" và chuyển sang các trình duyệt khác nếu một sự cố cụ thể là do trình duyệt.

Developer tools rất mạnh mẽ; chúng có nhiều tính năng. Để bắt đầu, chúng tôi sẽ tìm hiểu cách mở chúng, xem xét các lỗi và chạy các lệnh JavaScript.

## Google Chrome

Mở trang [bug.html](bug.html).

Có một lỗi trong mã JavaScript trên đó. Nó bị ẩn khỏi mắt của khách truy cập thông thường, vì vậy hãy mở các công cụ dành cho nhà phát triển để xem.

Nhấn phím `F12` hoặc, nếu bạn đang dùng Mac, thì phím `Cmd+Opt+J`.

Các developer tool sẽ mở trên tab Console theo mặc định.

Nó trông giống như thế này:

![chrome](chrome.png)

Giao diện chính xác của các công cụ dành cho nhà phát triển tùy thuộc vào phiên bản Chrome của bạn. Nó thay đổi theo thời gian nhưng nên tương tự.

- Ở đây chúng ta có thể thấy thông báo lỗi màu đỏ. Trong trường hợp này, tập lệnh chứa lệnh "lalala" không xác định.
- Ở bên phải, có một liên kết có thể nhấp vào nguồn `bug.html:12` với số dòng xảy ra lỗi.

Bên dưới thông báo lỗi, có một biểu tượng `>` màu xanh. Nó đánh dấu một "dòng lệnh" nơi chúng ta có thể gõ các lệnh JavaScript. Nhấn phím `Enter` để chạy chúng (`Shift+Enter` để nhập lệnh đa dòng).

Bây giờ chúng ta có thể thấy lỗi và đó là đủ để bắt đầu. Chúng tôi sẽ quay lại công cụ dành cho nhà phát triển sau và đề cập đến việc gỡ lỗi sâu hơn trong chương **debugging-chrome**.


## Firefox, Edge và những người khác

Hầu hết các trình duyệt khác sử dụng `F12` để mở các công cụ dành cho nhà phát triển.

Giao diện của chúng khá giống nhau. Khi bạn biết cách sử dụng một trong những công cụ này (bạn có thể bắt đầu với Chrome), bạn có thể dễ dàng chuyển sang công cụ khác.

## Safari

Safari (trình duyệt của Mac, không được Windows/Linux hỗ trợ) có một chút đặc biệt ở đây. Chúng ta cần kích hoạt "Develop menu" trước.

Mở Preferences và đi đến khung "Advanced". Có một hộp kiểm ở phía dưới:

![safari](safari.png)

Bây giờ `Cmd+Opt+C` có thể chuyển đổi sang console. Ngoài ra, lưu ý rằng mục menu mới trên cùng có tên "Develop" đã xuất hiện. Nó có nhiều lệnh và tùy chọn.

## Tóm lược

- Công cụ dành cho nhà phát triển cho phép chúng tôi xem lỗi, chạy lệnh, kiểm tra biến và nhiều hơn nữa.
- Chúng có thể được mở bằng `F12` cho hầu hết các trình duyệt trên Windows. Chrome cho Mac cần `Cmd+Opt+J`, Safari: `Cmd+Opt+C` (cần bật trước).

Bây giờ chúng tôi có môi trường sẵn sàng. Trong phần tiếp theo, chúng ta sẽ tìm hiểu về JavaScript.
