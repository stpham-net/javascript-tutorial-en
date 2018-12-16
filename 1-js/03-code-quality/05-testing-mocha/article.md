# Kiểm tra tự động với mocha

Kiểm tra tự động sẽ được sử dụng trong các nhiệm vụ tiếp theo.

Nó thực sự là một phần của "mức giáo dục tối thiểu" của nhà phát triển.

## Tại sao chúng ta cần kiểm tra?

Khi chúng ta viết một hàm, chúng ta thường có thể tưởng tượng nó nên làm gì: tham số nào cho kết quả nào.

Trong quá trình phát triển, chúng ta có thể kiểm tra function bằng cách chạy nó và so sánh kết quả với dự kiến. Ví dụ, chúng ta có thể làm điều đó trong console.

Nếu có gì đó không đúng - sau đó chúng ta sửa mã, chạy lại, kiểm tra kết quả -- và cứ thế cho đến khi nó hoạt động.

Nhưng "chạy lại (re-runs)" thủ công như vậy là không hoàn hảo.

**Khi kiểm tra mã bằng cách chạy lại thủ công, thật dễ dàng bỏ lỡ điều gì đó.**

Chẳng hạn, chúng ta đang tạo một hàm `f`. Đã viết một số mã, kiểm tra: `f(1)` hoạt động, nhưng `f(2)` không hoạt động. Chúng ta sửa mã và bây giờ `f(2)` hoạt động. Trông xong chưa? Nhưng chúng ta đã quên kiểm tra lại `f(1)`. Điều đó có thể dẫn đến một lỗi.

Điều đó rất điển hình. Khi chúng ta phát triển một cái gì đó, chúng ta ghi nhớ rất nhiều trường hợp sử dụng có thể. Nhưng thật khó để mong đợi một lập trình viên kiểm tra tất cả chúng theo cách thủ công sau mỗi thay đổi. Vì vậy, nó trở nên dễ dàng để sửa chữa một điều và phá vỡ một điều khác.

**Kiểm thử tự động có nghĩa là các kiểm tra được viết riêng, là mã phụ bổ xung thêm cho mã chính. Chúng có thể được thực hiện dễ dàng và kiểm tra tất cả các trường hợp sử dụng chính.**

## Phát triển hướng hành vi (BDD)

Chúng ta hãy sử dụng một kỹ thuật có tên [Phát triển hướng hành vi](http://en.wikipedia.org/wiki/Behavior-driven_development) hoặc nói ngắn gọn là BDD. Cách tiếp cận đó được sử dụng trong số nhiều dự án. BDD không chỉ là về kiểm thử. Còn nhiều hơn nữa.

**BDD là ba điều trong một: kiểm tra VÀ tài liệu VÀ ví dụ.**

Đủ từ. Hãy xem ví dụ.

## Phát triển "pow": thông số kỹ thuật (the spec)

Giả sử chúng ta muốn tạo một hàm `pow(x, n)` làm tăng `x` thành một số nguyên `n`. Chúng ta giả sử rằng `n ≥ 0`.

Nhiệm vụ đó chỉ là một ví dụ: có toán tử `**` trong JavaScript có thể làm điều đó, nhưng ở đây chúng ta tập trung vào luồng phát triển có thể được áp dụng cho các tác vụ phức tạp hơn.

Trước khi tạo mã của `pow`, chúng ta có thể tưởng tượng hàm nên làm gì và mô tả nó.

Mô tả như vậy được gọi là *đặc tả* hoặc nói ngắn gọn là một thông số kỹ thuật (a spec) và trông như thế này:

```js
      describe("pow", function() {

        it("raises to n-th power", function() {
          assert.equal(pow(2, 3), 8);
        });

      });
```

Một thông số kỹ thuật (spec) có ba khối xây dựng chính mà bạn có thể thấy ở trên:

**`describe("title", function() { ... })`**

Chúng ta đang mô tả chức năng gì. Sử dụng để nhóm "workers" -- các khối `it`. Trong trường hợp của chúng ta, chúng ta mô tả chức năng `pow`.

**`it("title", function() { ... })`**

Trong title của `it` chúng ta *theo cách có thể đọc được của con người* mô tả trường hợp sử dụng cụ thể và đối số thứ hai là một hàm kiểm tra nó.

**`assert.equal(value1, value2)`**

Mã bên trong khối `it`, nếu việc triển khai là chính xác, sẽ thực thi không có lỗi.

Các hàm `assert.*` Được sử dụng để kiểm tra xem `pow` có hoạt động như mong đợi không. Ngay tại đây, chúng ta đang sử dụng một trong số chúng -- `assert.equal`, nó so sánh các đối số và đưa ra lỗi nếu chúng không bằng nhau. Ở đây nó kiểm tra xem kết quả của `pow(2, 3)` bằng `8`.

Có nhiều loại so sánh và kiểm tra khác mà chúng ta sẽ xem thêm.

## Luồng phát triển

Dòng phát triển thường trông như thế này:

1. Một thông số ban đầu được viết, với các bài kiểm tra cho các chức năng cơ bản nhất.
2. Một triển khai ban đầu được tạo ra.
3. Để kiểm tra xem nó có hoạt động hay không, chúng ta chạy khung thử nghiệm [Mocha](http://mochajs.org/) (chi tiết sẽ sớm biết) để chạy thông số kỹ thuật (spec). Lỗi được hiển thị. Chúng ta sửa chữa cho đến khi mọi thứ hoạt động.
4. Bây giờ chúng ta có một triển khai ban đầu làm việc với các bài kiểm tra.
5. Chúng ta thêm nhiều trường hợp sử dụng vào spec, có thể chưa được hỗ trợ bởi việc triển khai. Các thử nghiệm bắt đầu thất bại.
6. Tới 3, cập nhật việc thực hiện cho đến khi các bài kiểm tra không có lỗi.
7. Lặp lại các bước 3-6 cho đến khi chức năng đã sẵn sàng.

Vì vậy, sự phát triển là *lặp lại*. Chúng ta viết spec, thực hiện nó, đảm bảo kiểm tra vượt qua, sau đó viết thêm các bài kiểm tra, đảm bảo chúng hoạt động, v.v ... Cuối cùng, chúng ta có cả việc thực hiện và kiểm tra cho nó.

Trong trường hợp của chúng ta, bước đầu tiên đã hoàn tất: chúng ta có một thông số ban đầu cho `pow`. Vì vậy, hãy thực hiện. Nhưng trước đó, hãy thực hiện một bước chạy "không (zero)" của spec, chỉ để thấy rằng các thử nghiệm đang hoạt động (tất cả chúng sẽ đều thất bại).

## The spec in action

Ở đây trong hướng dẫn, chúng ta sẽ sử dụng các thư viện JavaScript sau đây để kiểm tra:

- [Mocha](http://mochajs.org/) -- khung cốt lõi: nó cung cấp các chức năng kiểm tra phổ biến bao gồm `description` và `it` và chức năng chính chạy thử nghiệm.
- [Chai](http://chaijs.com) -- thư viện với nhiều xác nhận. Nó cho phép sử dụng rất nhiều xác nhận khác nhau, bây giờ chúng ta chỉ cần `assert.equal`.
- [Sinon](http://sinonjs.org/) - một thư viện để theo dõi các chức năng, mô phỏng các chức năng tích hợp và hơn thế nữa, chúng ta sẽ cần nó nhiều hơn sau này.

Các thư viện này phù hợp cho cả thử nghiệm trên trình duyệt và phía máy chủ. Ở đây chúng ta sẽ xem xét các biến thể trình duyệt.

Trang HTML đầy đủ với các khung này và `pow` spec:

```html
      <!DOCTYPE html>
      <html>
      <head>
        <!-- add mocha css, to show results -->
        <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/mocha/3.2.0/mocha.css">
        <!-- add mocha framework code -->
        <script src="https://cdnjs.cloudflare.com/ajax/libs/mocha/3.2.0/mocha.js"></script>
        <script>
          mocha.setup('bdd'); // minimal setup
        </script>
        <!-- add chai -->
        <script src="https://cdnjs.cloudflare.com/ajax/libs/chai/3.5.0/chai.js"></script>
        <script>
          // chai has a lot of stuff, let's make assert global
          let assert = chai.assert;
        </script>
      </head>

      <body>

        <script>
          function pow(x, n) {
            /* function code is to be written, empty now */
          }
        </script>

        <!-- the script with tests (describe, it...) -->
        <script src="test.js"></script>

        <!-- the element with id="mocha" will contain test results -->
        <div id="mocha"></div>

        <!-- run tests! -->
        <script>
          mocha.run();
        </script>
      </body>

      </html>
```

Trang này có thể được chia thành năm phần:

1. `<head>` -- thêm các thư viện và styles của bên thứ ba để kiểm tra.
2. Trong trường hợp của chúng ta, `<script>` với function cần kiểm tra -- với mã cho `pow`.
3. Các thử nghiệm -- trong trường hợp của chúng ta là một tập lệnh bên ngoài `test.js` có `describe("pow", ...)` từ phía trên.
4. Phần tử HTML `<div id="mocha">` sẽ được Mocha sử dụng để xuất kết quả.
5. Các bài kiểm tra được bắt đầu bằng lệnh `mocha.run()`.

Kết quả:

<http://plnkr.co/edit/Coz2HZNfrPtUhbE5oXTv?p=preview>

Đến bây giờ, thử nghiệm thất bại, có một lỗi. Điều đó hợp lý: chúng ta có một empty function code trong `pow`, vì vậy `pow(2,3)` trả về `undefined` thay vì `8`.

Trong tương lai, chúng ta hãy lưu ý rằng có những test-runners nâng cao, như [karma](https://karma-runner.github.io/) và những cái khác. Vì vậy, nói chung không phải là vấn đề để thiết lập nhiều thử nghiệm khác nhau.

## Triển khai ban đầu

Chúng ta hãy thực hiện đơn giản `pow`, để các bài kiểm tra vượt qua:

```js
      function pow() {
        return 8; // :) we cheat!
      }
```

Wow, bây giờ nó hoạt động!

<http://plnkr.co/edit/N0g8BSrn0BYt0YnL2xyt?p=preview>

## Cải thiện thông số kỹ thuật (Improving the spec)

Những gì chúng ta đã làm chắc chắn là một mánh gian lận. Hàm không hoạt động: một nỗ lực để tính toán `pow(3,4)` sẽ cho kết quả không chính xác, nhưng các bài kiểm tra đã vượt qua.

...Nhưng tình huống khá điển hình, nó xảy ra trong thực tế. Kiểm tra vượt qua, nhưng chức năng hoạt động sai. Spec của chúng ta là không hoàn hảo. Chúng ta cần thêm nhiều trường hợp sử dụng cho nó.

Hãy thêm một bài kiểm tra nữa để xem nếu `pow(3, 4) = 81`.

Chúng ta có thể chọn một trong hai cách để tổ chức kiểm tra tại đây:

1. Biến thể đầu tiên -- thêm một `assert` vào cùng `it`:

    ```js
    describe("pow", function() {

      it("raises to n-th power", function() {
        assert.equal(pow(2, 3), 8);
        assert.equal(pow(3, 4), 81);
      });

    });
    ```
    
2. Thứ hai - thực hiện hai bài kiểm tra:

    ```js
    describe("pow", function() {

      it("2 raised to power 3 is 8", function() {
        assert.equal(pow(2, 3), 8);
      });

      it("3 raised to power 3 is 27", function() {
        assert.equal(pow(3, 3), 27);
      });

    });
    ```

Sự khác biệt chính là khi `assert` gây ra lỗi, khối `it` ngay lập tức chấm dứt. Vì vậy, trong biến thể đầu tiên nếu `assert` đầu tiên thất bại, thì chúng ta sẽ không bao giờ thấy kết quả của` assert` thứ hai.

Làm cho các thử nghiệm riêng biệt là hữu ích để có thêm thông tin về những gì đang diễn ra, vì vậy biến thể thứ hai là tốt hơn.

Và bên cạnh đó, có thêm một quy tắc nữa là tốt để tuân theo.

**Một bài kiểm tra kiểm tra một điều.**

Nếu chúng ta xem xét test và thấy hai kiểm tra độc lập trong đó, tốt hơn là chia nó thành hai kiểm tra đơn giản hơn.

Vì vậy, hãy tiếp tục với biến thể thứ hai.

Kết quả:

<http://plnkr.co/edit/8OHMbIy9NpwZC7F08vpn?p=preview>

Như chúng ta có thể mong đợi, thử nghiệm thứ hai đã thất bại. Chắc chắn, hàm của chúng ta luôn trả về `8`, trong khi `assert` mong đợi `27`.

## Cải thiện việc thực hiện

Hãy viết một cái gì đó thực tế hơn để các bài kiểm tra vượt qua:

```js
      function pow(x, n) {
        let result = 1;

        for (let i = 0; i < n; i++) {
          result *= x;
        }

        return result;
      }
```

Để chắc chắn rằng chức năng hoạt động tốt, hãy kiểm tra nó để biết thêm giá trị. Thay vì viết các khối `it` bằng tay, chúng ta có thể tạo chúng trong `for`:

```js
      describe("pow", function() {

        function makeTest(x) {
          let expected = x * x * x;
          it(`${x} in the power 3 is ${expected}`, function() {
            assert.equal(pow(x, 3), expected);
          });
        }

        for (let x = 1; x <= 5; x++) {
          makeTest(x);
        }

      });
```

Kết quả:

<http://plnkr.co/edit/hT6N6WZh57etjFpVrgxF?p=preview>

## Mô tả lồng nhau (Nested describe)

Chúng ta sẽ thêm nhiều bài kiểm tra hơn nữa. Nhưng trước đó, hãy lưu ý rằng hàm trợ giúp `makeTest` và `for` nên được nhóm lại với nhau. Chúng ta sẽ không cần `makeTest` trong các thử nghiệm khác, nó chỉ cần trong `for`: nhiệm vụ chung của chúng là kiểm tra xem `pow` tăng như thế nào.

Nhóm được thực hiện với một `describe` lồng nhau:

```js
      describe("pow", function() {

        describe("raises x to power n", function() {

          function makeTest(x) {
            let expected = x * x * x;
            it(`${x} in the power 3 is ${expected}`, function() {
              assert.equal(pow(x, 3), expected);
            });
          }

          for (let x = 1; x <= 5; x++) {
            makeTest(x);
          }

        });

        // ... more tests to follow here, both describe and it can be added
      });
```

`describe` lồng nhau định nghĩa một "nhóm con" mới của thử nghiệm. Trong đầu ra, chúng ta có thể thấy thụt lề có tiêu đề:

<http://plnkr.co/edit/niGy485DwHAz1rMgUKKQ?p=preview>

Trong tương lai, chúng ta có thể thêm nhiều `it` và `describe` ở cấp cao nhất với các hàm trợ giúp của riêng họ, họ sẽ không thấy `makeTest`.

<br>

> ---

**📌 `before/after` và `beforeEach/afterEach`**

Chúng ta có thể thiết lập các hàm `before/after` thực thi before/after khi chạy thử nghiệm và cả các hàm `beforeEach/afterEach` thực thi before/after *every* `it`.

Ví dụ:

```js
      describe("test", function() {

        before(() => alert("Testing started – before all tests"));
        after(() => alert("Testing finished – after all tests"));

        beforeEach(() => alert("Before a test – enter a test"));
        afterEach(() => alert("After a test – exit a test"));

        it('test 1', () => alert(1));
        it('test 2', () => alert(2));

      });
```

Trình tự chạy sẽ là:

```
      Testing started – before all tests (before)
      Before a test – enter a test (beforeEach)
      1
      After a test – exit a test   (afterEach)
      Before a test – enter a test (beforeEach)
      2
      After a test – exit a test   (afterEach)
      Testing finished – after all tests (after)
```

Mở ví dụ trong sandbox.:

<http://plnkr.co/edit/tYilLAivTy7nYUEN9mJS?p=preview>

Thông thường, `beforeEach/afterEach` (`before/each`) được sử dụng để thực hiện khởi tạo, zero out counters hoặc làm một cái gì đó khác giữa các thử nghiệm (hoặc nhóm thử nghiệm).

> ---

<br>

## Mở rộng thông số kỹ thuật (Extending the spec)

Chức năng cơ bản của `pow` đã hoàn tất. Lặp lại đầu tiên của sự phát triển được thực hiện. Khi chúng ta hoàn thành lễ kỷ niệm và uống rượu sâm banh -- hãy tiếp tục và cải thiện nó.

Như đã nói, hàm `pow(x, n)` có nghĩa là hoạt động với các giá trị nguyên dương `n`.

Để chỉ ra lỗi toán học, các hàm JavaScript thường trả về `NaN`. Chúng ta hãy làm tương tự với các giá trị không hợp lệ của `n`.

Trước tiên, hãy thêm hành vi vào spec(!):

```js
      describe("pow", function() {

        // ...

        it("for negative n the result is NaN", function() {
          assert.isNaN(pow(2, -1));
        });

        it("for non-integer n the result is NaN", function() {
          assert.isNaN(pow(2, 1.5));    
        });

      });
```

Kết quả với các thử nghiệm mới:

<http://plnkr.co/edit/yXhjMSLW2aZm5g211G70?p=preview>

Các thử nghiệm mới được thêm không thành công, vì việc triển khai của chúng ta không hỗ trợ chúng. Đó là cách BDD được thực hiện: đầu tiên chúng ta viết các bài kiểm tra thất bại, và sau đó thực hiện chúng.

> ---

**📌 Các khẳng định khác (Other assertions)**

Vui lòng lưu ý khẳng định `assert.isNaN`: nó kiểm tra `NaN`.

Có những khẳng định khác trong Chai, ví dụ:

- `assert.equal(value1, value2)` -- kiểm tra bằng nhau `value1 == value2`.
- `assert.strictEqual(value1, value2)` -- kiểm tra đẳng thức nghiêm ngặt (strict equality) `value1 === value2`.
- `assert.notEqual`, `assert.notStrictEqual` -- kiểm tra ngược lại với những cái ở trên.
- `assert.isTrue(value)` -- kiểm tra xem `value === true`
- `assert.isFalse(value)` - kiểm tra xem `value === false`
- ... danh sách đầy đủ có trong [tài liệu](http://chaijs.com/api/assert/)

> ---

<br>

Vì vậy, chúng ta nên thêm một vài dòng vào `pow`:

```js
      function pow(x, n) {
        if (n < 0) return NaN;
        if (Math.round(n) != n) return NaN;

        let result = 1;

        for (let i = 0; i < n; i++) {
          result *= x;
        }

        return result;
      }
```

Bây giờ nó hoạt động, tất cả các bài kiểm tra vượt qua:

<http://plnkr.co/edit/WitPVCCVX3cyxuYEbwpk?p=preview>

## Tóm lược

Trong BDD, spec đi trước, tiếp theo là thực hiện. Cuối cùng, chúng ta có cả thông số kỹ thuật (spec) và mã.

The spec có thể được sử dụng theo ba cách:

1. **Kiểm thử** đảm bảo rằng mã hoạt động chính xác.
2. **Tài liệu (Docs)** -- tiêu đề của `describe` và `it` cho biết function này làm gì.
3. **Ví dụ (Examples)** -- các thử nghiệm thực sự là các ví dụ hoạt động cho thấy cách sử dụng một chức năng.

Với spec, chúng ta có thể cải thiện một cách an toàn, thay đổi, thậm chí viết lại function từ đầu và đảm bảo nó vẫn hoạt động tốt.

Điều đó đặc biệt quan trọng trong các dự án lớn khi một function được sử dụng ở nhiều nơi. Khi chúng ta thay đổi một function như vậy, sẽ không có cách nào để kiểm tra thủ công nếu mọi nơi sử dụng function đó vẫn hoạt động tốt.

Không có kiểm thử, mọi người có hai cách:

1. Thực hiện thay đổi, không quan tâm vấn đề là gì. Và sau đó người dùng của chúng ta gặp lỗi và báo cáo chúng. Nếu chúng ta có thể đủ khả năng đó.
2. Hoặc mọi người trở nên sợ sửa đổi các function như vậy, nếu hình phạt cho lỗi là khắc nghiệt. Sau đó, nó trở nên cũ kỹ, phát triển quá mức với mạng nhện, không ai muốn vào đó và điều đó không tốt.

**Mã được kiểm tra tự động là trái với điều đó!**

Nếu dự án được bảo hiểm với các thử nghiệm, sẽ không có vấn đề như vậy. Chúng ta có thể chạy thử nghiệm và xem rất nhiều kiểm tra được thực hiện trong vài giây.

**Bên cạnh đó, một mã được kiểm tra tốt có kiến trúc tốt hơn.**

Đương nhiên, đó là vì nó dễ dàng hơn để thay đổi và cải thiện nó. Nhưng không chỉ có thế.

Để viết các bài kiểm tra, mã phải được tổ chức theo cách sao cho mọi hàm đều có một nhiệm vụ được mô tả rõ ràng, đầu vào và đầu ra được xác định rõ. Điều đó có nghĩa là một kiến trúc tốt ngay từ đầu.

Trong cuộc sống thực, điều đó đôi khi không dễ dàng. Đôi khi thật khó để viết một thông số trước mã thực tế, vì vẫn chưa rõ nó nên hoạt động như thế nào. Nhưng nhìn chung viết kiểm thử làm cho sự phát triển nhanh hơn và ổn định hơn.

## What now?

Cuối cùng xuyên suốt hướng dẫn, bạn sẽ gặp nhiều nhiệm vụ với các bài kiểm tra. Vì vậy, bạn sẽ thấy nhiều ví dụ thực tế hơn.

Viết kiểm thử đòi hỏi kiến thức JavaScript tốt. Nhưng chúng ta mới bắt đầu học nó. Vì vậy, để giải quyết mọi thứ, hiện tại bạn không bắt buộc phải viết kiểm thử, nhưng bạn đã có thể đọc chúng ngay cả khi chúng phức tạp hơn một chút so với trong chương này.
