# Giới thiệu về JavaScript

Chúng ta hãy xem những gì đặc biệt về JavaScript, những gì chúng ta có thể đạt được với nó và những công nghệ khác chơi tốt với nó.

## JavaScript là gì?

*JavaScript* ban đầu được tạo để *"làm cho các trang web sống động"*.

Các chương trình trong ngôn ngữ này được gọi là *script*. Chúng có thể được viết ngay trong HTML của trang web và được thực thi tự động khi tải trang.

Các kịch bản được cung cấp và thực hiện dưới dạng văn bản thuần túy. Họ không cần chuẩn bị đặc biệt hoặc biên soạn để chạy.

Ở khía cạnh này, JavaScript rất khác với một ngôn ngữ khác gọi là [Java](https://en.wikipedia.org/wiki/Java_(programming_language)).


> Tại sao "Java" Script?
> Khi JavaScript được tạo, ban đầu nó có một tên khác: "LiveScript". Nhưng Java rất phổ biến vào thời điểm đó, vì vậy người ta đã quyết định rằng việc định vị một ngôn ngữ mới là "em trai" của Java sẽ giúp ích.
> 
> Nhưng khi phát triển, JavaScript đã trở thành một ngôn ngữ hoàn toàn độc lập với đặc tả riêng có tên là [ECMAScript] (http://en.wikipedia.org/wiki/ECMAScript) và bây giờ nó không liên quan gì đến Java.


Ngày nay, JavaScript có thể thực thi không chỉ trên trình duyệt mà còn trên máy chủ hoặc trên bất kỳ thiết bị nào có chương trình đặc biệt gọi là [JavaScript engine] (https://en.wikipedia.org/wiki/JavaScript_engine).

Trình duyệt có một embedded engine đôi khi được gọi là "JavaScript virtual machine".

Các động cơ khác nhau có "codenames" khác nhau. Ví dụ:

- [V8](https://en.wikipedia.org/wiki/V8_(JavaScript_engine)) - trong Chrome và Opera.
- [SpiderMonkey](https://en.wikipedia.org/wiki/SpiderMonkey) - trong Firefox.
- ...Có các tên mã khác như "Trident" và "Chakra" cho các phiên bản khác nhau của IE, "ChakraCore" cho Microsoft Edge, "Nitro" và "SquirrelFish" cho Safari, v.v.

Các thuật ngữ trên rất tốt để nhớ vì chúng được sử dụng trong các bài viết dành cho nhà phát triển trên internet. Chúng tôi cũng sẽ sử dụng chúng. Chẳng hạn, nếu "một tính năng X được hỗ trợ bởi V8", thì nó có thể hoạt động trong Chrome và Opera.


> Engine hoạt động như thế nào?
> 
> Engine rất phức tạp. Nhưng những điều cơ bản là dễ hiểu.
> 
> 1. Engine (embedded nếu đó là trình duyệt) đọc ("phân tích cú pháp (parses)") tập lệnh.
> 2. Sau đó, nó chuyển đổi ("biên dịch (compiles)") tập lệnh sang ngôn ngữ máy (machine language).
> 3. Và sau đó mã máy (machine code) chạy, khá nhanh.
> 
> Engine áp dụng tối ưu hóa ở mỗi bước của quy trình (process). Nó thậm chí còn xem (watches) tập lệnh được biên dịch (compiled script) khi nó chạy, phân tích dữ liệu chảy qua nó và áp dụng tối ưu hóa cho machine code dựa trên kiến thức đó. Khi xong, các script chạy khá nhanh.


## JavaScript trong trình duyệt có thể làm gì?

JavaScript hiện đại là ngôn ngữ lập trình "an toàn". Nó không cung cấp quyền truy cập cấp thấp vào bộ nhớ hoặc CPU, vì ban đầu nó được tạo cho các trình duyệt không yêu cầu.

Khả năng của Javascript phụ thuộc rất nhiều vào môi trường mà nó đang chạy. Chẳng hạn, [Node.JS](https://wikipedia.org/wiki/Node.js) hỗ trợ các function cho phép JavaScript đọc/ghi các tệp tùy ý, thực hiện các network request, v.v.

JavaScript trong trình duyệt có thể thực hiện mọi thứ liên quan đến thao tác trang web, tương tác với người dùng và máy chủ web.

Chẳng hạn, JavaScript trong trình duyệt có thể:

- Thêm HTML mới vào trang, thay đổi nội dung hiện có, sửa đổi styles.
- Phản ứng với hành động của người dùng, chạy khi nhấp chuột, di chuyển con trỏ, nhấn phím.
- Gửi requests qua mạng đến các máy chủ từ xa, tải xuống và tải lên các tệp (cái gọi là [AJAX](https://en.wikipedia.org/wiki/Ajax_(programming)) và [COMET](https://en.wikipedia.org/wiki/Comet_(programming)) technologies).
- Get và set cookie, đặt câu hỏi cho khách truy cập, hiển thị tin nhắn.
- Ghi nhớ dữ liệu ở phía máy khách ("local storage").

## JavaScript trong trình duyệt KHÔNG THỂ làm gì?

Khả năng của JavaScript trong trình duyệt bị giới hạn vì sự an toàn của người dùng. Mục đích là để ngăn chặn một trang web xấu truy cập thông tin cá nhân hoặc làm hại dữ liệu của người dùng.

Ví dụ về các hạn chế đó bao gồm:

- JavaScript trên trang web không được đọc/ghi các tệp tùy ý trên đĩa cứng, sao chép chúng hoặc thực thi chương trình. Nó không có quyền truy cập trực tiếp vào các chức năng hệ thống hệ điều hành.

    Các trình duyệt hiện đại cho phép nó hoạt động với các tệp, nhưng quyền truy cập bị hạn chế và chỉ được cung cấp nếu người dùng thực hiện một số hành động nhất định, như "thả" tệp vào cửa sổ trình duyệt hoặc chọn tệp qua thẻ `<input>`.

    Có nhiều cách để tương tác với camera/microphone và các thiết bị khác, nhưng chúng cần có sự cho phép rõ ràng của người dùng. Vì vậy, một trang hỗ trợ JavaScript có thể không lén lút kích hoạt web-camera, quan sát môi trường xung quanh và gửi thông tin đến [NSA](https://en.wikipedia.org/wiki/National_Security_Agency).
- Các tab/cửa sổ khác nhau thường không biết về nhau. Đôi khi, chúng có thể, ví dụ khi một cửa sổ sử dụng JavaScript để mở cửa sổ khác. Nhưng ngay cả trong trường hợp này, JavaScript từ một trang có thể không truy cập được vào những trang khác nếu chúng đến từ các trang web khác nhau (từ một tên miền, giao thức hoặc cổng khác nhau).

    Điều này được gọi là "Chính sách nguồn gốc tương tự (Same Origin Policy)". Để giải quyết vấn đề đó, *cả hai trang* phải chứa mã JavaScript đặc biệt xử lý trao đổi dữ liệu.

    Hạn chế này, một lần nữa, là vì sự an toàn của người dùng. Một trang từ `http://anysite.com` mà người dùng đã mở không được truy cập vào tab trình duyệt khác có URL`http://gmail.com` và đánh cắp thông tin từ đó.
- JavaScript có thể dễ dàng giao tiếp qua mạng đến máy chủ nơi trang hiện tại (current page) đến. Nhưng khả năng nhận dữ liệu từ các web/tên miền khác bị tê liệt. Mặc dù có thể, nó yêu cầu thỏa thuận rõ ràng (được thể hiện bằng các HTTP headers) từ phía remote. Một lần nữa, đó là một giới hạn an toàn.

![](limitations@2x.png)

Các giới hạn như vậy không tồn tại nếu JavaScript được sử dụng bên ngoài trình duyệt, ví dụ như trên máy chủ. Các trình duyệt hiện đại cũng cho phép plugin/extensions mở rộng có thể yêu cầu quyền mở rộng (extended permissions).

## Điều gì làm cho JavaScript trở nên độc đáo?

Có ít nhất *ba* điều tuyệt vời về JavaScript:


> so sánh
> + Tích hợp đầy đủ với HTML/CSS.
> + Những điều đơn giản được thực hiện đơn giản.
> + Hỗ trợ bởi tất cả các trình duyệt chính và được bật theo mặc định.


Javascript là công nghệ trình duyệt duy nhất kết hợp ba thứ này.

Đó là điều làm cho JavaScript trở nên độc đáo. Đó là lý do tại sao nó là công cụ phổ biến nhất để tạo giao diện trình duyệt.

Trong khi lên kế hoạch tìm hiểu một công nghệ mới, việc kiểm tra các quan điểm của nó là có lợi. Vì vậy, hãy chuyển sang các xu hướng hiện đại ảnh hưởng đến nó, bao gồm các ngôn ngữ mới và khả năng của trình duyệt.


## Ngôn ngữ "trên" JavaScript

Cú pháp của JavaScript không phù hợp với nhu cầu của tất cả mọi người. Những người khác nhau muốn các tính năng khác nhau.

Điều đó được mong đợi, bởi vì các dự án và yêu cầu là khác nhau đối với mọi người.

Vì vậy, gần đây rất nhiều ngôn ngữ mới xuất hiện, được *transpiled* (chuyển đổi) thành JavaScript trước khi chúng chạy trong trình duyệt.

Các công cụ hiện đại làm cho việc dịch mã trở nên rất nhanh và minh bạch, thực sự cho phép các nhà phát triển viết mã bằng ngôn ngữ khác và tự động chuyển đổi nó "dưới mui xe".

Ví dụ về các ngôn ngữ như vậy:

- [CoffeeScript](http://coffeescript.org/) là "syntactic sugar" cho JavaScript. Nó giới thiệu cú pháp ngắn hơn, cho phép chúng tôi viết mã rõ ràng và chính xác hơn. Thông thường, Ruby devs thích nó.
- [TypeScript](http://www.typescriptlang.org/) tập trung vào việc thêm "gõ dữ liệu nghiêm ngặt (strict data typing)" để đơn giản hóa việc phát triển và hỗ trợ các hệ thống phức tạp. Nó được phát triển bởi Microsoft.
- [Dart](https://www.dartlang.org/) là ngôn ngữ độc lập có công cụ riêng chạy trong môi trường không có trình duyệt (như ứng dụng di động). Ban đầu, nó được Google cung cấp để thay thế cho JavaScript, nhưng đến bây giờ, các trình duyệt yêu cầu nó được dịch mã sang JavaScript giống như các trình duyệt ở trên.

Có nhiều. Tất nhiên, ngay cả khi chúng ta sử dụng một trong những ngôn ngữ này, chúng ta cũng nên biết JavaScript để thực sự hiểu những gì chúng ta đang làm.

## Tóm lược

- JavaScript ban đầu được tạo như một ngôn ngữ chỉ dành cho trình duyệt, nhưng hiện tại cũng được sử dụng trong nhiều môi trường khác.
- Ngày nay, JavaScript có một vị trí độc tôn là "ngôn ngữ trình duyệt" được sử dụng rộng rãi nhất với sự tích hợp đầy đủ với HTML/CSS.
- Có nhiều ngôn ngữ được "dịch mã (transpiled)" sang JavaScript và cung cấp một số tính năng nhất định. Bạn nên xem qua chúng, ít nhất là một thời gian ngắn, sau khi thành thạo JavaScript.
