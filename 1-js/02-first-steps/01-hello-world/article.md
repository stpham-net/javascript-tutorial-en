# Hello, world!

Hướng dẫn mà bạn đang đọc là về JavaScript cốt lõi, độc lập với nền tảng. Sắp tới, bạn sẽ học Node.JS và các nền tảng khác sử dụng nó.

Nhưng, chúng ta cần một môi trường làm việc để chạy các tập lệnh của chúng ta và, chỉ vì cuốn sách này trực tuyến, trình duyệt là một lựa chọn tốt. Chúng ta sẽ giữ số lượng lệnh cụ thể của trình duyệt (như `alert`) ở mức tối thiểu để bạn không dành thời gian cho chúng nếu bạn dự định tập trung vào một môi trường khác như Node.JS. Mặt khác, chi tiết trình duyệt được giải thích chi tiết trong [phần tiếp theo](/ui) của hướng dẫn.

Vì vậy, trước tiên, hãy xem cách đính kèm tập lệnh vào trang web. Đối với các môi trường phía máy chủ, bạn có thể thực thi nó bằng một lệnh như `"node my.js"` cho Node.js


## Thẻ "script"

Các chương trình JavaScript có thể được chèn vào bất kỳ phần nào của tài liệu HTML với sự trợ giúp của thẻ `<script>`.

Ví dụ:

```html
      <!DOCTYPE HTML>
      <html>

      <body>

        <p>Before the script...</p>

        <script>
          alert('Hello, world!');
        </script>

        <p>...After the script.</p>

      </body>

      </html>
```


Thẻ `<script>` chứa mã JavaScript được tự động thực thi khi trình duyệt bắt gặp thẻ.


## Đánh dấu hiện đại

Thẻ `<script>` có một vài thuộc tính (attributes) hiếm khi được sử dụng ngày nay, nhưng chúng ta có thể tìm thấy chúng trong mã cũ:

**The `type` attribute: <code>&lt;script <u>type</u>=...&gt;</code>**

HTML4 tiêu chuẩn cũ yêu cầu một tập lệnh phải có một loại. Thông thường nó là `type="text/javascript"`. Nó không còn cần thiết nữa. Ngoài ra, tiêu chuẩn hiện đại đã thay đổi hoàn toàn ý nghĩa của thuộc tính này. Bây giờ nó có thể được sử dụng cho các mô-đun Javascript. Nhưng đó là một chủ đề nâng cao; chúng ta sẽ nói về các mô-đun sau trong phần khác của hướng dẫn. 

**The `language` attribute: <code>&lt;script <u>language</u>=...&gt;</code>**

Thuộc tính này có nghĩa là để hiển thị ngôn ngữ của kịch bản. Thuộc tính này không còn có ý nghĩa, bởi vì JavaScript là ngôn ngữ mặc định. Không cần sử dụng nó.

**Comments trước và sau tập lệnh.**

Trong những cuốn sách và hướng dẫn thực sự cổ xưa, người ta có thể tìm thấy những comment bên trong `<script>`, như thế này:

```
      <script type="text/javascript"><!--
          ...
      //--></script>
```

Thủ thuật này không được sử dụng trong JavaScript hiện đại. Những comment này được sử dụng để ẩn mã JavaScript khỏi các trình duyệt cũ không biết về thẻ `<script>`. Vì các trình duyệt được phát hành trong 15 năm qua không có vấn đề này, loại comment này có thể giúp bạn xác định mã thực sự cũ.

## External scripts

Nếu chúng ta có nhiều mã JavaScript, chúng ta có thể đặt nó vào một tệp riêng.

Tệp script được đính kèm với HTML với thuộc tính `src`:

```html
      <script src="/path/to/script.js"></script>
```

Ở đây `/path/to/script.js` là một đường dẫn tuyệt đối đến tệp có script (từ site root).

Cũng có thể cung cấp một đường dẫn liên quan đến trang hiện tại. Chẳng hạn, `src="script.js"` có nghĩa là một tệp `"script.js"` trong thư mục hiện tại.

Chúng ta cũng có thể cung cấp một URL đầy đủ. Ví dụ:

```html
      <script src="https://cdnjs.cloudflare.com/ajax/libs/lodash.js/3.2.0/lodash.js"></script>
```

Để đính kèm một số tập lệnh, sử dụng nhiều thẻ:


```html
      <script src="/js/script1.js"></script>
      <script src="/js/script2.js"></script>
      …
```


🎐 **Xin lưu ý:**

Theo quy định, chỉ các tập lệnh đơn giản nhất được đưa vào HTML. Những cái phức tạp hơn nằm trong các tập tin riêng biệt.

Lợi ích của một tệp riêng là trình duyệt sẽ tải xuống và sau đó lưu nó vào [bộ đệm](https://en.wikipedia.org/wiki/Web_cache).

Sau này, các trang khác muốn có cùng một tập lệnh sẽ lấy nó từ bộ đệm thay vì tải xuống. Vì vậy, tập tin thực sự chỉ được tải xuống một lần.

Điều đó giúp tiết kiệm lưu lượng và làm cho các trang nhanh hơn.


⚠️ **Nếu `src` được đặt, nội dung tập lệnh sẽ bị bỏ qua.**

Một thẻ `<script>` duy nhất không thể có cả thuộc tính `src` và mã bên trong.

Điều này sẽ không hoạt động:


```html
      <script src="file.js">
        alert(1); // the content is ignored, because src is set
      </script>
```

Chúng ta phải chọn: đó là một `<script src="…">` bên ngoài hoặc một `<script>` thông thường có mã.

Ví dụ trên có thể được chia thành hai tập lệnh để làm việc:

```html
      <script src="file.js"></script>
      <script>
        alert(1);
      </script>
```


## Tóm lược

- Chúng ta có thể sử dụng thẻ `<script>` để thêm mã JavaScript vào trang.
- Không yêu cầu thuộc tính `type` và `language`.
- Một script trong tệp bên ngoài có thể được chèn bằng `<script src="path/to/script.js"></script>`.


Có nhiều hơn nữa để tìm hiểu về các browser script và sự tương tác của chúng với trang web. Nhưng hãy nhớ rằng phần này của hướng dẫn được dành cho ngôn ngữ JavaScript, vì vậy chúng ta không nên lạc hướng khỏi nó. Chúng ta sẽ sử dụng trình duyệt như một cách để chạy JavaScript, rất thuận tiện cho việc đọc trực tuyến, nhưng vẫn chỉ là một trong nhiều cách.
