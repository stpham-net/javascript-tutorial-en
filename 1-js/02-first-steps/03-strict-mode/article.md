# Chế độ hiện đại, "use strict"

Trong một thời gian dài JavaScript đã phát triển mà không có vấn đề về tương thích. Các tính năng mới đã được thêm vào ngôn ngữ, nhưng chức năng cũ không thay đổi.

Điều đó có lợi ích của việc không bao giờ phá vỡ mã hiện có. Nhưng nhược điểm là bất kỳ sai lầm hoặc quyết định không hoàn hảo nào được tạo bởi các nhà sáng tạo JavaScript đã bị mắc kẹt trong ngôn ngữ mãi mãi.

Cho đến năm 2009, ECMAScript 5 (ES5) đã xuất hiện. Nó đã thêm các tính năng mới cho ngôn ngữ và sửa đổi một số tính năng hiện có. Để giữ cho mã cũ hoạt động, hầu hết các sửa đổi được tắt theo mặc định. Người ta cần kích hoạt chúng một cách rõ ràng bằng một lệnh đặc biệt `"use strict"`.

## "use strict"

Lệnh này trông giống như một chuỗi: `"use strict"` hoặc `'use strict'`. Khi nó nằm ở trên cùng của tập lệnh, thì toàn bộ tập lệnh hoạt động theo cách "hiện đại" ("modern" way).

Ví dụ:

```js
      "use strict";

      // this code works the modern way
      ...
```

Chúng ta sẽ tìm hiểu các chức năng (một cách để nhóm các lệnh) sớm.

Nhìn tiếp và hãy lưu ý rằng `"use strict"` có thể được đặt ở đầu một hàm (hầu hết các loại hàm) thay vì toàn bộ tập lệnh. Sau đó, chế độ nghiêm ngặt chỉ được kích hoạt trong chức năng đó. Nhưng thông thường mọi người sử dụng nó cho toàn bộ kịch bản.

<br>

> ---

**📌  Đảm bảo rằng "use strict" là ở trên đầu**

Vui lòng đảm bảo rằng `"use strict"` ở trên cùng của tập lệnh, nếu không chế độ nghiêm ngặt có thể không được bật.

Không có chế độ nghiêm ngặt (strict mode) ở đây:

```js
      alert("some code");
      // "use strict" below is ignored, must be on the top

      "use strict";

      // strict mode is not activated
```

Chỉ các comment có thể xuất hiện ở trên `"use strict"`.

> ---

<br>
<br>

> ---

**📌 Không có cách nào để hủy bỏ `use strict`**

Không có chỉ thị `"no use strict"` hoặc tương tự, sẽ trả lại hành vi cũ.

Khi chúng ta vào chế độ nghiêm ngặt (strict mode), sẽ không có sự trở lại.

> ---

<br>

## Always "use strict"

Sự khác biệt của `"use strict"` so với chế độ "default" vẫn còn được đề cập.

Trong các chương tiếp theo, khi chúng ta tìm hiểu các tính năng ngôn ngữ, chúng ta sẽ ghi chú về sự khác biệt của chế độ nghiêm ngặt và mặc định. May mắn thay, không có quá nhiều. Và chúng thực sự làm cho cuộc sống của chúng ta tốt hơn.

Tại thời điểm này, đủ để biết về nó nói chung là:

1. Lệnh `"use strict"` chuyển động cơ (engine) sang chế độ "hiện đại (modern)", thay đổi hành vi của một số tính năng tích hợp (built-in features). Chúng ta sẽ thấy chi tiết khi chúng ta nghiên cứu.
2. Chế độ nghiêm ngặt (strict mode) được kích hoạt bởi `"use strict"` ở trên cùng. Ngoài ra, có một số tính năng ngôn ngữ như "lớp" và "mô-đun" cho phép chế độ nghiêm ngặt tự động.
3. Chế độ nghiêm ngặt được hỗ trợ bởi tất cả các trình duyệt hiện đại.
4. Chúng ta luôn khuyến nghị bắt đầu các tập lệnh với `"use strict"`. Tất cả các ví dụ trong hướng dẫn này đều cho là như vậy, trừ khi (rất hiếm khi) được chỉ định khác.
No search results.
