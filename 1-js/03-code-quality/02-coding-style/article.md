# Coding Style

Mã của chúng ta phải sạch và dễ đọc nhất có thể.

Đó thực sự là nghệ thuật lập trình -- để thực hiện một nhiệm vụ phức tạp và mã hóa nó theo cách vừa chính xác vừa có thể đọc được.

## Syntax

Dưới đây là một cheatsheet với một số quy tắc được đề xuất (xem bên dưới để biết thêm chi tiết):

![](code-style.png)

Bây giờ hãy thảo luận về các quy tắc và lý do cho chúng một cách chi tiết.

<br>

> ---

**📌 Irony Detected**

Không có gì được đặt trong đá ở đây. Đây là những sở thích phong cách, không phải giáo điều tôn giáo.

> ---

<br>

### Dấu ngoặc nhọn

Trong hầu hết các dự án JavaScript, dấu ngoặc nhọn được viết theo kiểu "Ai Cập" với dấu ngoặc mở trên cùng dòng với từ khóa tương ứng - không phải trên một dòng mới. Cũng cần có một khoảng trắng trước dấu ngoặc mở, như thế này:

```js
      if (condition) {
        // do this
        // ...and that
        // ...and that
      }
```

Cấu trúc một dòng là một trường hợp góc cạnh quan trọng. Chúng ta có nên sử dụng dấu ngoặc ở tất cả? Nếu có thì ở đâu?

Dưới đây là các biến thể chú thích để bạn có thể tự đánh giá khả năng đọc của chúng:

![](figure-bracket-style.png)

Tóm tắt:
- Đối với mã rất ngắn, một dòng là chấp nhận được. Ví dụ: `if (cond) return null`.
- Nhưng một dòng riêng cho mỗi câu trong ngoặc thường dễ đọc hơn.

### Độ dài của dòng (Line Length)

Không ai thích đọc một dòng mã ngang dài. Cách tốt nhất là tách chúng ra và giới hạn độ dài của các dòng của bạn.

Độ dài dòng tối đa phải được thỏa thuận ở cấp độ đội. Nó thường là 80 hoặc 120 ký tự.

### thụt lề (Indents)

Có hai loại thụt lề:

- **Thụt lề ngang: 2 hoặc 4 khoảng trắng.**

    Một thụt lề ngang được thực hiện bằng cách sử dụng 2 hoặc 4 khoảng trắng hoặc biểu tượng "Tab". Chọn cái nào là một cuộc chiến thánh cũ. Khoảng trắng ngày nay phổ biến hơn.

    Một lợi thế của khoảng trắng so với tab là khoảng trắng cho phép cấu hình thụt lề linh hoạt hơn biểu tượng "Tab".

    Chẳng hạn, chúng ta có thể căn chỉnh các đối số với dấu ngoặc mở, như thế này:

    ```js
    show(parameters,
         aligned, // 5 spaces padding at the left  
         one,
         after,
         another
      ) {
      // ...
    }
    ```

- **Thụt lề dọc: các dòng trống để chia mã thành các khối logic.**

    Ngay cả một function duy nhất thường có thể được chia thành các khối logic. Trong ví dụ dưới đây, việc khởi tạo các biến, vòng lặp chính và trả về kết quả được phân chia theo chiều dọc:

    ```js
    function pow(x, n) {
      let result = 1;
      //              <--
      for (let i = 0; i < n; i++) {
        result *= x;
      }
      //              <--
      return result;
    }
    ```

    Chèn một dòng mới bổ sung ở nơi nó có thể giúp mã dễ đọc hơn. Không nên có nhiều hơn chín dòng mã mà không có thụt lề dọc.

### Semicolons

Một dấu chấm phẩy nên có mặt sau mỗi câu, ngay cả khi nó có thể bị bỏ qua.

Có những ngôn ngữ mà dấu chấm phẩy thực sự là tùy chọn và nó hiếm khi được sử dụng. Tuy nhiên, trong JavaScript, có những trường hợp ngắt dòng không được hiểu là dấu chấm phẩy, khiến mã dễ bị lỗi.

Khi bạn trở nên trưởng thành hơn với tư cách là một lập trình viên, bạn có thể chọn một kiểu không có dấu chấm phẩy như [StandardJS](https://standardjs.com/). Cho đến khi đó, tốt nhất là sử dụng dấu chấm phẩy để tránh những cạm bẫy có thể xảy ra.

### Các cấp độ lồng (Nesting Levels)

Cố gắng tránh mã lồng nhau quá nhiều cấp độ sâu.

Đôi khi, nên sử dụng chỉ thị `continue` trong một vòng lặp để tránh việc lồng thêm.

Ví dụ, thay vì thêm một điều kiện `if` lồng nhau như thế này:

```js
      for (let i = 0; i < 10; i++) {
        if (cond) {
          ... // <- one more nesting level
        }
      }
```

Chúng ta có thể viết:

```js
      for (let i = 0; i < 10; i++) {
        if (!cond) continue;
        ...  // <- no extra nesting level
      }
```

Một điều tương tự có thể được thực hiện với `if/else` và `return`.

Ví dụ, hai cấu trúc dưới đây là giống hệt nhau.

Lựa chọn 1:

```js
      function pow(x, n) {
        if (n < 0) {
          alert("Negative 'n' not supported");
        } else {
          let result = 1;

          for (let i = 0; i < n; i++) {
            result *= x;
          }

          return result;
        }  
      }
```

Lựa chọn 2:

```js
      function pow(x, n) {
        if (n < 0) {
          alert("Negative 'n' not supported");
          return;
        }

        let result = 1;

        for (let i = 0; i < n; i++) {
          result *= x;
        }

        return result;
      }
```

Cái thứ hai dễ đọc hơn vì "trường hợp góc cạnh" của `n < 0` được xử lý sớm. Sau khi kiểm tra xong, chúng ta có thể chuyển sang luồng mã "chính" mà không cần thêm lồng.

## Function Placement

Nếu bạn đang viết một số hàm "helper" và mã sử dụng chúng, có ba cách để tổ chức các hàm.

1. Các hàm được khai báo ở trên mã sử dụng chúng:

    ```js
    // function declarations
    function createElement() {
      ...
    }

    function setHandler(elem) {
      ...
    }

    function walkAround() {
      ...
    }

    // the code which uses them
    let elem = createElement();
    setHandler(elem);
    walkAround();
    ```
    
2. Mã trước, sau đó là các function

    ```js
    // the code which uses the functions
    let elem = createElement();
    setHandler(elem);
    walkAround();

    // --- helper functions ---
    function createElement() {
      ...
    }

    function setHandler(elem) {
      ...
    }

    function walkAround() {
      ...
    }
    ```
    
3. Hỗn hợp (Mixed): một function được khai báo khi nó được sử dụng lần đầu tiên.

Hầu hết thời gian, biến thể thứ hai được ưa thích.

Đó là bởi vì khi đọc mã, trước tiên chúng ta muốn biết *nó làm gì*. Nếu mã trước, thì nó cung cấp thông tin đó. Sau đó, có lẽ chúng ta sẽ không cần phải đọc các function, đặc biệt nếu tên của chúng là mô tả về những gì chúng thực sự làm.

## Style Guides

Một style guide chứa các quy tắc chung về "cách viết" mã, ví dụ sử dụng dấu ngoặc kép nào, bao nhiêu khoảng trắng để thụt lề, nơi đặt ngắt dòng, v.v ... Rất nhiều điều nhỏ.

Khi tất cả các thành viên của một nhóm sử dụng cùng một style guide, mã có xu hướng trông đồng nhất, bất kể thành viên nào trong nhóm đã viết nó.

Tất nhiên, một nhóm luôn có thể viết style guide riêng của họ. Hầu hết thời gian, không cần. Có rất nhiều lựa chọn đã được thử và đúng để lựa chọn, vì vậy việc áp dụng một trong những lựa chọn này thường là đặt cược tốt nhất của bạn.

Một số lựa chọn phổ biến:

- [Google JavaScript Style Guide](https://google.github.io/styleguide/javascriptguide.xml)
- [Airbnb JavaScript Style Guide](https://github.com/airbnb/javascript)
- [Idiomatic.JS](https://github.com/rwaldron/idiomatic.js)
- [StandardJS](https://standardjs.com/)
- (plus many more)

Nếu bạn là một nhà phát triển mới, hãy bắt đầu với cheatsheet ở đầu chương này. Khi bạn đã thành thạo bạn có thể duyệt các style guide khác để chọn các nguyên tắc chung và quyết định xem bạn thích cái nào nhất.

## Automated Linters

Linters là các công cụ có thể tự động kiểm tra style mã của bạn và đưa ra các đề xuất để tái cấu trúc.

Điều tuyệt vời ở chúng là style-checking cũng có thể tìm thấy một số lỗi, như lỗi chính tả trong tên biến hoặc hàm. Do tính năng này, việc cài đặt một linter được khuyến nghị ngay cả khi bạn không muốn dính vào một "code style" cụ thể.

Dưới đây là các công cụ linting nổi tiếng nhất:

- [JSLint](http://www.jslint.com/) -- one of the first linters.
- [JSHint](http://www.jshint.com/) -- more settings than JSLint.
- [ESLint](http://eslint.org/) -- probably the newest one.

Tất cả đều có thể làm được việc. Tác giả sử dụng [ESLint](http://eslint.org/).

Hầu hết các linters được tích hợp với nhiều trình soạn thảo phổ biến: chỉ cần kích hoạt plugin trong trình chỉnh sửa và định cấu hình style.

Ví dụ, đối với ESLint, bạn nên làm như sau:

1. Cài đặt [Node.JS](https://nodejs.org/).
2. Cài đặt ESLint bằng lệnh `npm install -g eslint` (npm là trình cài đặt gói JavaScript).
3. Tạo một tệp cấu hình có tên `.eslintrc` trong thư mục gốc của dự án JavaScript của bạn (trong thư mục chứa tất cả các tệp của bạn).
4. Cài đặt/kích hoạt plugin cho trình soạn thảo của bạn tích hợp với ESLint. Phần lớn các editor có một cái.

Đây là một ví dụ về tệp `.eslintrc`:

```js
      {
        "extends": "eslint:recommended",
        "env": {
          "browser": true,
          "node": true,
          "es6": true
        },
        "rules": {
          "no-console": 0,
        },
        "indent": 2
      }
```

Ở đây, lệnh `"extends"` biểu thị rằng cấu hình được dựa trên tập cài đặt "eslint:recommended". Sau đó, chúng ta chỉ định riêng của chúng ta.

Cũng có thể tải xuống các bộ quy tắc style từ web và mở rộng chúng. Xem <http://eslint.org/docs/user-guide/getting-started> để biết thêm chi tiết về cài đặt.

Ngoài ra, một số IDE nhất định có linting tích hợp, tiện lợi nhưng không thể tùy chỉnh như ESLint.

## Tóm lược

Tất cả các quy tắc cú pháp được mô tả trong chương này (và trong các style guide được tham chiếu) nhằm mục đích tăng khả năng đọc mã của bạn, nhưng tất cả chúng đều gây tranh cãi.

Khi chúng ta nghĩ về việc viết mã "tốt hơn", các câu hỏi chúng ta nên đặt ra là "Điều gì làm cho mã dễ đọc hơn và dễ hiểu hơn?" và "Điều gì có thể giúp chúng ta tránh lỗi?" Đây là những điều chính cần ghi nhớ khi lựa chọn và thảo luận về các code style.

Đọc các hướng dẫn style phổ biến sẽ cho phép bạn cập nhật những ý tưởng mới nhất về xu hướng code style và thực tiễn tốt nhất.
