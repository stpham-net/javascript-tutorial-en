# Comments

Như chúng ta đã biết từ chương **structure**, các comment có thể là một dòng: bắt đầu bằng `//` và nhiều dòng: `/ * ... */`.

Chúng ta thường sử dụng chúng để mô tả cách thức và lý do tại sao mã hoạt động.

Từ cái nhìn đầu tiên, commenting có thể là hiển nhiên, nhưng người mới trong lập trình thường hiểu sai.

## Bad comments

Người mới có xu hướng sử dụng các comment để giải thích "những gì đang diễn ra trong mã". Như thế này:

```js
      // This code will do this thing (...) and that thing (...)
      // ...and who knows what else...
      very;
      complex;
      code;
```

Nhưng trong mã tốt, số lượng các comment "giải thích" như vậy nên tối thiểu. Nghiêm túc mà nói, mã nên dễ hiểu nếu không có chúng.

Có một quy tắc tuyệt vời về điều đó: "nếu mã không rõ ràng đến mức nó yêu cầu một nhận xét, thì có lẽ nó nên được viết lại".

### Công thức: Đưa các function ra ngoài

Đôi khi, có ích khi thay thế một đoạn mã bằng một hàm, như ở đây:

```js
      function showPrimes(n) {
        nextPrime:
        for (let i = 2; i < n; i++) {

          // check if i is a prime number
          for (let j = 2; j < i; j++) {
            if (i % j == 0) continue nextPrime;
          }

          alert(i);
        }
      }
```

Biến thể tốt hơn, với chức năng bao gồm `isPrime`:

```js
      function showPrimes(n) {

        for (let i = 2; i < n; i++) {
          if (!isPrime(i)) continue;

          alert(i);  
        }
      }

      function isPrime(n) {
        for (let i = 2; i < n; i++) {
          if (n % i == 0) return false;
        }

        return true;
      }
```

Bây giờ chúng ta có thể hiểu mã dễ dàng. Các function chính bản thân nó trở thành comment. Mã như vậy được gọi là *tự mô tả (self-descriptive)*.

### Recipe: tạo các function

Và nếu chúng ta có một "bảng mã" dài như thế này:

```js
      // here we add whiskey
      for(let i = 0; i < 10; i++) {
        let drop = getWhiskey();
        smell(drop);
        add(drop, glass);
      }

      // here we add juice
      for(let t = 0; t < 3; t++) {
        let tomato = getTomato();
        examine(tomato);
        let juice = press(tomato);
        add(juice, glass);
      }

      // ...
```

Sau đó, nó có thể là một biến thể tốt hơn để cấu trúc lại nó thành các function như:

```js
      addWhiskey(glass);
      addJuice(glass);

      function addWhiskey(container) {
        for(let i = 0; i < 10; i++) {
          let drop = getWhiskey();
          //...
        }
      }

      function addJuice(container) {
        for(let t = 0; t < 3; t++) {
          let tomato = getTomato();
          //...
        }
      }
```

Một lần nữa, các function tự cho biết những gì đang xảy ra. Không có gì để comment. Và cấu trúc mã cũng tốt hơn khi phân chia. Rõ ràng mọi function làm gì, những gì nó cần và những gì nó trả về.

Trong thực tế, chúng ta hoàn toàn không thể tránh những comment "giải thích". Có các thuật toán phức tạp. Và có những "tinh chỉnh" thông minh cho mục đích tối ưu hóa. Nhưng nhìn chung chúng ta nên cố gắng giữ mã đơn giản và tự mô tả.

## Good comments

Vì vậy, comment giải thích thường là xấu. Những comment nào là tốt?

**Mô tả kiến trúc (Describe the architecture)** 

Cung cấp tổng quan cấp cao về các thành phần, cách chúng tương tác, luồng điều khiển trong các tình huống khác nhau ... Nói tóm lại -- cái nhìn của con chim (the bird's eye view) về mã. Có một ngôn ngữ sơ đồ đặc biệt [UML](http://wikipedia.org/wiki/Unified_Modeling_Language) cho các sơ đồ kiến trúc cấp cao. Chắc chắn đáng học tập.

**Tài liệu sử dụng một function** 

Có một cú pháp đặc biệt [JSDoc](http://en.wikipedia.org/wiki/JSDoc) để ghi lại một hàm: cách sử dụng, tham số, giá trị được trả về.

Ví dụ:

```js
      /**
       * Returns x raised to the n-th power.
       *
       * @param {number} x The number to raise.
       * @param {number} n The power, must be a natural number.
       * @return {number} x raised to the n-th power.
       */
      function pow(x, n) {
        ...
      }
```

Các comment như vậy cho phép chúng ta hiểu mục đích của function và sử dụng nó đúng cách mà không cần tìm trong mã của nó.

Nhân tiện, nhiều editor như [WebStorm](https://www.jetbrains.com/webstorm/) cũng có thể hiểu chúng và sử dụng chúng để cung cấp autocomplete và một chút kiểm tra mã tự động (automatic code-checking).

Ngoài ra, có những công cụ như [JSDoc 3](https://github.com/jsdoc3/jsdoc) có thể tạo tài liệu HTML từ các comment. Bạn có thể đọc thêm thông tin về JSDoc tại <http://usejsdoc.org/>.

**Tại sao nhiệm vụ được giải quyết theo cách này?** 

Những gì được viết là quan trọng. Nhưng những gì *không* được viết có thể còn quan trọng hơn để hiểu những gì đang diễn ra. Tại sao nhiệm vụ được giải quyết chính xác theo cách này? Mã không đưa ra câu trả lời.

Nếu có nhiều cách để giải quyết nhiệm vụ, tại sao lại có cách này? Đặc biệt là khi nó không phải là một trong những cách rõ ràng nhất.

Không có các comment như vậy tình huống sau đây là có thể:

1. Bạn (hoặc đồng nghiệp của bạn) mở mã được viết cách đây một thời gian và thấy rằng đó là "tối ưu".
2. Bạn nghĩ: "Lúc đó tôi ngu ngốc đến mức nào, và tôi thông minh hơn bao nhiêu", và viết lại bằng cách sử dụng biến thể "rõ ràng và đúng đắn hơn".
3. ...Sự thôi thúc để viết lại là tốt. Nhưng trong quá trình bạn thấy rằng giải pháp "rõ ràng hơn" thực sự còn thiếu. Bạn thậm chí còn lờ mờ nhớ tại sao, bởi vì bạn đã thử nó từ lâu rồi. Bạn trở lại biến thể chính xác, nhưng thời gian đã bị lãng phí.

Các comment giải thích giải pháp là rất quan trọng. Họ giúp tiếp tục phát triển đúng cách.

**Bất kỳ tính năng tinh tế của mã? Chúng được sử dụng ở đâu?** 

Nếu mã có bất cứ điều gì tinh tế và phản trực giác, nó chắc chắn đáng để commenting.

## Tóm lược

Một dấu hiệu quan trọng của một nhà phát triển tốt là các comment: sự hiện diện của chúng và thậm chí sự vắng mặt của họ.

Comment tốt cho phép chúng ta duy trì mã tốt, quay lại mã sau khi trì hoãn và sử dụng mã hiệu quả hơn.

**Comment this:**

- Kiến trúc tổng thể, cái nhìn cao cấp (high-level view).
- Cách dùng function (Function usage).
- Giải pháp quan trọng, đặc biệt là khi không rõ ràng ngay lập tức.

**Avoid comments:**

- Điều đó cho biết "cách mã hoạt động" và "những gì nó làm".
- Chỉ đặt chúng nếu không thể làm cho mã đơn giản và tự mô tả.

Các comment cũng được sử dụng cho các công cụ tạo tài liệu tự động như JSDoc3: họ đọc chúng và tạo tài liệu HTML (hoặc tài liệu ở định dạng khác).
