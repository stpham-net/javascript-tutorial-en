# Ninja code

> *Học mà không suy nghĩ thì vô ích; nghĩ mà không học thì nguy hiểm. - Khổng Tử*

Các ninja lập trình trong quá khứ đã sử dụng các thủ thuật này để mài giũa tâm trí của những người duy trì mã.

Các chuyên gia đánh giá mã tìm kiếm chúng trong các nhiệm vụ thử nghiệm.

Các nhà phát triển chưa có kinh nghiệm đôi khi sử dụng chúng thậm chí còn tốt hơn các ninja lập trình viên.

Đọc chúng cẩn thận và tìm ra bạn là ai - một ninja, một người mới, hoặc có thể là một nhà phê bình mã?

<br>

> ---

**📌 Irony Detected**

Nhiều người cố gắng đi theo con đường ninja. Ít thành công.

> ---

<br>

## Ngắn gọn là linh hồn của trí thông minh

Làm cho mã càng ngắn càng tốt. Cho thấy bạn thông minh như thế nào.

Hãy để các tính năng ngôn ngữ tinh tế hướng dẫn bạn.

Chẳng hạn, hãy xem toán tử ternary `'?'`:

```js
      // taken from a well-known javascript library
      i = i ? i < 0 ? Math.max(0, len + i) : i : 0;
```

Thật tuyệt phải không? Nếu bạn viết như vậy, một nhà phát triển bắt gặp dòng này và cố gắng hiểu giá trị của `i` là gì sẽ có một thời gian vui vẻ. Sau đó đến với bạn, tìm kiếm một câu trả lời.

Nói với họ rằng ngắn hơn luôn luôn tốt hơn. Họ bắt đầu đi vào con đường của ninja.

## Biến một chữ cái

> *Người Dao ẩn mình trong không lời. Chỉ có Dao là bắt đầu tốt và hoàn thành tốt. - Laozi (Tao Te Ching)*

Một cách khác để mã nhanh hơn là sử dụng tên biến một chữ cái ở mọi nơi. Giống như `a`, `b` hoặc `c`.

Một biến ngắn biến mất trong mã giống như một ninja thực sự trong rừng. Không ai có thể tìm thấy nó bằng cách sử dụng "tìm kiếm" của trình soạn thảo. Và ngay cả khi ai đó làm như vậy, họ sẽ không thể "giải mã" ý nghĩa của tên `a` hoặc `b`.

...Nhưng có một ngoại lệ. Một ninja thực sự sẽ không bao giờ sử dụng `i` làm bộ đếm trong vòng lặp `"for"`. Bất cứ nơi nào, nhưng không phải ở đây. Nhìn xung quanh, có nhiều chữ kỳ lạ hơn. Chẳng hạn, `x` hoặc `y`.

Một biến kỳ lạ như một bộ đếm vòng lặp đặc biệt thú vị nếu thân vòng lặp mất 1-2 trang (làm cho nó dài hơn nếu bạn có thể). Sau đó, nếu ai đó nhìn sâu vào bên trong vòng lặp, họ sẽ không thể nhanh chóng nhận ra rằng biến có tên `x` là bộ đếm vòng lặp.

## Sử dụng chữ viết tắt

Nếu quy tắc nhóm cấm sử dụng tên một chữ cái và mơ hồ - rút ngắn chúng, hãy viết tắt.

Như thế này:

- `list` -> `lst`.
- `userAgent` -> `ua`.
- `browser` -> `brsr`.
- ...etc

Chỉ người có trực giác thực sự tốt mới có thể hiểu được những cái tên như vậy. Hãy cố gắng rút ngắn mọi thứ. Chỉ một người xứng đáng mới có thể duy trì sự phát triển của mã của bạn.

## Cao vút. Hãy trừu tượng.

> *Quảng trường lớn không có góc*
> 
> *Tàu lớn hoàn thành cuối cùng,*
> 
> *Ghi chú tuyệt vời là âm thanh hiếm,*
> 
> *Hình ảnh tuyệt vời không có hình thức.*
> 
> *- Laozi (Tao Te Ching)*

Trong khi chọn một tên cố gắng sử dụng từ trừu tượng nhất. Giống như `obj`, `data`, `value`, `item`, `elem` v.v.

- **Tên lý tưởng cho một biến là `data`.** Sử dụng nó ở mọi nơi bạn có thể. Thật vậy, mỗi biến giữ *dữ liệu*, phải không?

    ...Nhưng phải làm gì nếu `data` đã được sử dụng? Hãy thử `value`, nó cũng phổ quát. Rốt cuộc, một biến cuối cùng nhận được một *value*.

- **Đặt tên một biến theo kiểu của nó: `str`, `num`...**

    Hãy thử chúng. Một đồng tu trẻ có thể tự hỏi - những cái tên như vậy có thực sự hữu ích cho một ninja không? Thực sự, chúng có!

    Chắc chắn, tên biến vẫn có nghĩa. Nó nói những gì bên trong biến: một chuỗi, một số hoặc một cái gì đó khác. Nhưng khi một người ngoài cố gắng hiểu mã, anh ta sẽ ngạc nhiên khi thấy rằng thực sự không có thông tin nào cả! Và cuối cùng sẽ thất bại trong việc thay đổi mã được suy nghĩ tốt của bạn.

    Kiểu giá trị dễ dàng tìm ra bằng cách gỡ lỗi. Nhưng ý nghĩa của biến là gì? Với string/number nào nó lưu trữ?

    Không có cách nào để tìm ra mà không có một thiền định tốt!

- **...Nhưng nếu không có nhiều tên như vậy thì sao?** Chỉ cần thêm một số: `data1, item2, elem5`...

## Kiểm tra chú ý

Chỉ có một lập trình viên thực sự chu đáo mới có thể hiểu mã của bạn. Nhưng làm thế nào để kiểm tra điều đó?

**Một trong những cách - sử dụng các tên biến tương tự, như `date` và `data`.**

Trộn chúng ở nơi bạn có thể.

Việc đọc nhanh mã như vậy trở nên không thể. Và khi có một lỗi đánh máy ... Ừm ... Chúng tôi bị mắc kẹt trong thời gian dài để uống trà.

## Từ đồng nghĩa thông minh

> *Việc khó nhất trong mọi việc là tìm con mèo đen trong buồng tối, đặc biệt nếu chẳng có con mèo nào cả. - Khổng Tử*

Sử dụng tên *tương tự* cho những thứ *giống nhau* làm cho cuộc sống thú vị hơn và thể hiện sự sáng tạo của bạn với công chúng.

Ví dụ, xem xét các tiền tố chức năng. Nếu một chức năng hiển thị một thông báo trên màn hình - hãy khởi động nó bằng `display…`, như `displayMessage`. Và sau đó nếu một chức năng khác hiển thị trên màn hình một cái gì đó khác, như tên người dùng, hãy khởi động nó bằng `show…` (như `showName`).

Khẳng định rằng có một sự khác biệt tinh tế giữa các chức năng như vậy, trong khi không có.

Tạo một hiệp ước với các ninja đồng đội: nếu John bắt đầu "hiển thị" các chức năng với `display...` trong mã của mình, thì Peter có thể sử dụng `render..` và Ann - `paint...`. Lưu ý rằng mã trở nên thú vị và đa dạng hơn nhiều.

...Và bây giờ là hat trick!

Đối với hai hàm có sự khác biệt quan trọng - sử dụng cùng một tiền tố!

Chẳng hạn, hàm `printPage(trang)` sẽ sử dụng máy in. Và hàm `printText(text)` sẽ đặt văn bản trên màn hình. Hãy để một người đọc xa lạ nghĩ tốt về chức năng có tên tương tự `printMessage`: "Nó đặt thông điệp ở đâu? Tới một máy in hoặc trên màn hình?". Để làm cho nó thực sự tỏa sáng, `printMessage(message)` nên xuất nó trong cửa sổ mới!

## Tên sử dụng lại

> *Một khi toàn bộ được chia, các phần cần tên.*
> 
> *Đã có đủ tên.*
> 
> *Người ta phải biết khi nào nên dừng lại.*
> 
> *- Laozi (Tao Te Ching)*

Thêm một biến mới chỉ khi thực sự cần thiết.

Thay vào đó, sử dụng lại tên hiện có. Chỉ cần viết các giá trị mới vào chúng.

Trong một hàm cố gắng chỉ sử dụng các biến được truyền dưới dạng tham số.

Điều đó sẽ khiến việc xác định chính xác những gì trong biến *bây giờ* rất khó khăn. Và nó cũng đến từ đâu. Một người có trực giác yếu sẽ phải phân tích từng dòng mã và theo dõi các thay đổi thông qua mỗi nhánh mã.

**Một biến thể tiên tiến của phương pháp này là để ngấm ngầm (!) thay thế giá trị bằng một cái gì đó giống nhau ở giữa một vòng lặp hoặc một hàm.**

Ví dụ:

```js
      function ninjaFunction(elem) {
        // 20 lines of code working with elem

        elem = clone(elem);

        // 20 more lines, now working with the clone of the elem!
      }
```

Một lập trình viên đồng nghiệp muốn làm việc với `elem` trong nửa sau của hàm sẽ ngạc nhiên ... Chỉ trong quá trình gỡ lỗi, sau khi kiểm tra mã họ sẽ phát hiện ra rằng anh ta đang làm việc với một bản sao!

Nhìn thấy trong mã thường xuyên. Hiệu quả chết người thậm chí chống lại một ninja có kinh nghiệm. 

## Underscores cho vui

Đặt dấu gạch dưới `_` và `__` trước tên biến. Giống như `_name` hoặc `__value`. Sẽ thật tuyệt nếu chỉ có bạn biết ý nghĩa của chúng. Hoặc, tốt hơn, thêm chúng chỉ để cho vui, không có ý nghĩa đặc biệt nào cả. Hoặc ý nghĩa khác nhau ở những nơi khác nhau.

Bạn giết hai con thỏ bằng một phát súng. Đầu tiên, mã trở nên dài hơn và ít đọc hơn, và thứ hai, một nhà phát triển đồng nghiệp có thể mất nhiều thời gian để cố gắng tìm hiểu ý nghĩa của dấu gạch dưới.

Một ninja thông minh đặt dấu gạch dưới tại một điểm mã và trốn chúng ở những nơi khác. Điều đó làm cho mã thậm chí còn dễ vỡ hơn và tăng xác suất xảy ra lỗi trong tương lai.

## Thể hiện tình yêu của bạn

Hãy để mọi người thấy thực thể của bạn tuyệt vời như thế nào! Những cái tên như `superEuity`, `megaFrame` và `beautifulItem` chắc chắn sẽ khai sáng cho người đọc.

Thật vậy, từ một tay, một cái gì đó được viết: `super..`, `mega..`, `nice..` Nhưng từ mặt khác - điều đó không mang lại chi tiết. Một người đọc có thể quyết định tìm kiếm một ý nghĩa ẩn và thiền trong một hoặc hai giờ.

## Chồng chéo các biến ngoài

> *Khi ở trong ánh sáng, không thể nhìn thấy bất cứ thứ gì trong bóng tối.*
> 
> *Khi ở trong bóng tối, có thể nhìn thấy mọi thứ trong ánh sáng.*
> 
> *- Guan Yin Zi*

Sử dụng cùng tên cho các biến trong và ngoài hàm. Như đơn giản. Không cần nỗ lực.

```js
      let user = authenticateUser();

      function render() {
        let user = anotherValue();
        ...
        ...many lines...
        ...
        ... // <-- a programmer wants to work with user here and...
        ...
      }
```

Một lập trình viên nhảy vào bên trong `render` có thể sẽ không nhận thấy rằng có một 'user' cục bộ đang che giấu cái bên ngoài.

Sau đó, anh ta sẽ cố gắng làm việc với `user` giả sử rằng đó là biến ngoài, kết quả của `authenticateUser()` ... Cái bẫy bị bung ra! Xin chào, trình gỡ lỗi ...

## Tác dụng phụ ở mọi nơi!

Có những chức năng trông giống như chúng không thay đổi bất cứ điều gì. Giống như `isReady()`, `checkPermission()`, `findTags()`... Họ được cho là thực hiện các tính toán, tìm và trả lại dữ liệu mà không thay đổi bất cứ điều gì bên ngoài chúng. Nói cách khác, không có "tác dụng phụ".

**Một mẹo thực sự hay là thêm một hành động "hữu ích" cho họ, bên cạnh nhiệm vụ chính.**

Một biểu hiện ngạc nhiên kinh ngạc trên khuôn mặt của đồng nghiệp của bạn khi họ thấy một chức năng có tên là `is..`, `check..` hoặc `find...` thay đổi điều gì đó - chắc chắn sẽ mở rộng ranh giới lý trí của bạn.

**Một cách khác để gây bất ngờ là trả về kết quả không chuẩn.**

Thể hiện suy nghĩ ban đầu của bạn! Hãy để cuộc gọi của `checkPermission` trả về không phải là `true/false`, mà là một đối tượng phức tạp với kết quả kiểm tra.

Những nhà phát triển cố gắng viết `if(checkPermission(..))`, sẽ tự hỏi tại sao nó không hoạt động. Nói với họ: "Đọc tài liệu!". Và đưa ra bài viết này.

## Chức năng mạnh mẽ!

> *Đạo lớn chảy khắp nơi,*
> 
> *cả bên trái và bên phải.*
> 
> *- Laozi (Tao Te Ching)*

Đừng giới hạn chức năng bởi những gì được viết trong tên của nó. Hãy rộng hơn.

Chẳng hạn, một hàm `validateEmail(email)` có thể (bên cạnh việc kiểm tra tính chính xác của email) hiển thị thông báo lỗi và yêu cầu nhập lại email.

Các hành động bổ sung không nên rõ ràng từ tên hàm. Một lập trình viên ninja thực thụ sẽ làm cho họ không rõ ràng từ mã.

**Kết hợp một số hành động thành một để bảo vệ mã của bạn khỏi việc sử dụng lại.**

Hãy tưởng tượng, một nhà phát triển khác chỉ muốn kiểm tra email chứ không xuất ra bất kỳ tin nhắn nào. Hàm của bạn `validateEmail(email)` mà cả hai sẽ không phù hợp với họ. Vì vậy, họ sẽ không phá vỡ thiền của bạn bằng cách hỏi bất cứ điều gì về nó.

## Tóm lược

Tất cả "những lời khuyên" ở trên là từ mã thực sự ... Đôi khi, được viết bởi các nhà phát triển có kinh nghiệm. Có thể thậm chí nhiều kinh nghiệm hơn bạn ;)

- Thực hiện theo một số trong số chúng, và mã của bạn sẽ trở nên đầy bất ngờ.
- Theo nhiều trong số chúng, và mã của bạn sẽ thực sự trở thành của bạn, không ai muốn thay đổi nó.
- Làm theo tất cả, và mã của bạn sẽ trở thành một bài học quý giá cho các nhà phát triển trẻ đang tìm kiếm sự giác ngộ.

