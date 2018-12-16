
# Polyfill

Ngôn ngữ JavaScript phát triển đều đặn. Các đề xuất mới cho ngôn ngữ xuất hiện thường xuyên, chúng được phân tích và, nếu được coi là xứng đáng, được thêm vào danh sách tại <https://tc39.github.io/ecma262/> và sau đó chuyển sang [specification](http://www.ecma-international.org/publications/standards/Ecma-262.htm).

Các nhóm đằng sau các công cụ JavaScript có ý tưởng riêng của họ về những gì cần triển khai trước. Họ có thể quyết định thực hiện các đề xuất trong dự thảo và hoãn lại những điều đã có trong thông số kỹ thuật (spec), bởi vì chúng ít thú vị hơn hoặc chỉ khó thực hiện hơn.

Vì vậy, nó khá phổ biến đối với một engine chỉ thực hiện một phần của tiêu chuẩn.

Một trang tốt để xem trạng thái hỗ trợ hiện tại cho các tính của năng ngôn ngữ là <https://kangax.github.io/compat-table/es6/> (nó thật lớn, chúng ta còn nhiều điều phải nghiên cứu).

## Babel

Khi chúng ta sử dụng các tính năng hiện đại của ngôn ngữ, một số engine có thể không hỗ trợ mã đó. Như đã nói, không phải tất cả các tính năng được thực hiện ở khắp mọi nơi.

Ở đây Babel đến giải cứu.

[Babel](https://babeljs.io) là một [transpiler](https://en.wikipedia.org/wiki/Source-to-source_compiler). Nó viết lại mã JavaScript hiện đại thành tiêu chuẩn trước đó.

Thật ra, có hai phần trong Babel:

1. Đầu tiên, chương trình transpiler, viết lại mã. Nhà phát triển chạy nó trên máy tính của riêng họ. Nó viết lại mã thành tiêu chuẩn cũ hơn. Và sau đó mã được gửi đến trang web cho người dùng. Hệ thống xây dựng dự án hiện đại như [webpack](http://webpack.github.io/) hoặc [brunch](http://brunch.io/) cung cấp phương tiện để chạy transpiler tự động trên mỗi thay đổi mã, do đó không mất thêm bất kì thời gian cho việc đó từ phía chúng ta.

2. Thứ hai, polyfill.

    Bộ chuyển mã viết lại mã, vì vậy các tính năng cú pháp được đề cập. Nhưng đối với các chức năng mới, chúng ta cần viết một tập lệnh đặc biệt thực hiện chúng. JavaScript là một ngôn ngữ rất năng động, các tập lệnh có thể không chỉ thêm các hàm mới mà còn sửa đổi các hàm dựng sẵn, để chúng hoạt động theo tiêu chuẩn hiện đại.

    Có một thuật ngữ "polyfill" cho các tập lệnh "điền vào" khoảng trống và thêm các triển khai bị thiếu.

    Hai polyfill thú vị là:
    - [babel polyfill](https://babeljs.io/docs/usage/polyfill/) hỗ trợ rất nhiều, nhưng lớn.
    - Dịch vụ [polyfill.io](http://polyfill.io) cho phép tải/xây dựng (load/construct) các polyfill theo yêu cầu, tùy thuộc vào các tính năng chúng ta cần.

Vì vậy, chúng ta cần thiết lập bộ chuyển mã và thêm polyfill cho các engine cũ để hỗ trợ các tính năng hiện đại.

Nếu chúng ta hướng tới các engine hiện đại và không sử dụng các tính năng ngoại trừ các tính năng được hỗ trợ ở mọi nơi, thì chúng ta không cần sử dụng Babel.

Lưu ý rằng khi sản xuất (production) chúng ta có thể sử dụng Babel để dịch mã thành phù hợp với các trình duyệt ít gần đây hơn, do đó sẽ không có giới hạn như vậy, mã sẽ chạy ở mọi nơi.
