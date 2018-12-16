# Thu gom dữ liệu rác (Garbage collection)

Quản lý bộ nhớ trong JavaScript được thực hiện tự động và vô hình đối với chúng ta. Chúng ta tạo ra nguyên thủy, đối tượng, chức năng ... Tất cả đều cần có bộ nhớ.

Điều gì xảy ra khi một cái gì đó không cần thiết nữa? Làm thế nào để JavaScript engine phát hiện ra nó và dọn sạch nó?

## Khả năng tiếp cận (Reachability)

Khái niệm chính về quản lý bộ nhớ trong JavaScript là *khả năng tiếp cận (reachability)*.

Nói một cách đơn giản, giá trị "có thể tiếp cận" là những giá trị có thể truy cập hoặc sử dụng được bằng cách nào đó. Chúng được đảm bảo được lưu trữ trong bộ nhớ.

1. Có một bộ cơ sở của các giá trị vốn có thể tiếp cận, không thể xóa được vì những lý do rõ ràng.

    Ví dụ:

    - Các biến cục bộ (Local variables) và tham số của hàm hiện tại.
    - Biến và tham số cho các function khác trên chuỗi các cuộc gọi lồng nhau hiện tại.
    - Các biến toàn cầu (Global variables).
    - (có một số khác, nội bộ cũng tốt)

    Các giá trị này được gọi là *roots*.

2. Bất kỳ giá trị nào khác được coi là có thể tiếp cận nếu có thể tiếp cận từ root bằng một tham chiếu hoặc bởi một chuỗi các tham chiếu (chain of references).

    Chẳng hạn, nếu có một đối tượng trong một biến cục bộ và đối tượng đó có một thuộc tính tham chiếu đến một đối tượng khác, thì đối tượng đó được coi là có thể tiếp cận. Và những cái mà nó tham chiếu cũng có thể tiếp cận. Ví dụ chi tiết để tìm hiểu.

Có một quy trình nền (background process) trong JavaScript engine được gọi là [trình thu gom dữ liệu rác (garbage collector)](https://en.wikipedia.org/wiki/Garbage_collection_(computer_science)). Nó giám sát tất cả các đối tượng và loại bỏ những cái đã trở nên không thể truy cập được.

## Một ví dụ đơn giản

Đây là ví dụ đơn giản nhất:

```js
      // user has a reference to the object
      let user = {
        name: "John"
      };
```

![](memory-user-john.png)

Ở đây mũi tên mô tả một tham chiếu đối tượng. Biến toàn cục `"user"` tham chiếu đến đối tượng `{name: "John"}` (chúng ta sẽ gọi nó là John cho đơn giản). Thuộc tính `"name"` của John lưu trữ nguyên thủy, do đó, nó được vẽ bên trong đối tượng.

Nếu giá trị của `user` bị ghi đè, tham chiếu sẽ bị mất:

```js
      user = null;
```

![](memory-user-john-lost.png)

Bây giờ John trở nên không thể truy cập. Không có cách nào để truy cập nó, không có tham chiếu đến nó. Trình thu gom dữ liệu vô nghĩa sẽ loại bỏ dữ liệu và giải phóng bộ nhớ.

## Hai tham chiếu (Two references)

Bây giờ hãy tưởng tượng chúng ta đã sao chép tham chiếu từ `user` sang `admin`:

```js
      // user has a reference to the object
      let user = {
        name: "John"
      };

      let admin = user;
```

![](memory-user-john-admin.png)

Bây giờ nếu chúng ta làm như này:

```js
      user = null;
```

...Sau đó, đối tượng vẫn có thể truy cập thông qua biến toàn cục `admin`, vì vậy nó nằm trong bộ nhớ. Nếu chúng ta cũng ghi đè lên `admin`, thì nó có thể bị xóa.

## Đối tượng liên kết với nhau (Interlinked objects)

Bây giờ là một ví dụ phức tạp hơn. Gia đình:

```js
      function marry(man, woman) {
        woman.husband = man;
        man.wife = woman;

        return {
          father: man,
          mother: woman
        }
      }

      let family = marry({
        name: "John"
      }, {
        name: "Ann"
      });
```

Hàm `marry` "kết hôn" hai đối tượng bằng cách cho chúng tham chiếu với nhau và trả về một đối tượng mới chứa cả hai.

Kết quả cấu trúc bộ nhớ:

![](family.png)

Đến bây giờ, tất cả các đối tượng có thể truy cập.

Bây giờ hãy xóa hai tham chiếu:

```js
      delete family.father;
      delete family.mother.husband;
```

![](family-delete-refs.png)

Chỉ xóa một trong hai tham chiếu này là không đủ, bởi vì tất cả các đối tượng vẫn có thể truy cập được.

Nhưng nếu chúng ta xóa cả hai, thì chúng ta có thể thấy rằng John không còn tham chiếu đến nữa:

![](family-no-father.png)

Các tham chiếu đi không quan trọng. Chỉ những tham chiếu đến mới có thể làm cho một đối tượng có thể truy cập. Vì vậy, John hiện không thể truy cập được và sẽ bị xóa khỏi bộ nhớ với tất cả dữ liệu của nó cũng không thể truy cập được.

Sau khi thu gom dữ liệu rác (garbage collection):

![](family-no-father-2.png)

## Đảo không thể tiếp cận (Unreachable island)

Cũng có thể là toàn bộ đảo của các đối tượng được liên kết với nhau trở nên không thể truy cập được và bị xóa khỏi bộ nhớ.

Đối tượng nguồn giống như trên. Rồi:

```js
      family = null;
```

Hình ảnh trong bộ nhớ trở thành:

![](family-no-family.png)

Ví dụ này cho thấy tầm quan trọng của khả năng tiếp cận.

Rõ ràng là John và Ann vẫn được liên kết, cả hai đều có các tham chiếu đến. Nhưng điều đó là không đủ.

Đối tượng `"family"` trước đây đã bị hủy liên kết từ root, không còn tham chiếu đến nó nữa, vì vậy toàn bộ hòn đảo trở nên không thể truy cập được và sẽ bị xóa.

## Thuật toán nội bộ (Internal algorithms)

Thuật toán thu gom dữ liệu rác cơ bản được gọi là "mark-and-sweep".

Các bước "thu gom dữ liệu rác" sau đây thường xuyên được thực hiện:

- Trình thu gom dữ liệu vô nghĩa lấy roots và "đánh dấu (marks)" (ghi nhớ) chúng.
- Sau đó, nó truy cập và "đánh dấu (marks)" tất cả các tham chiếu từ chúng.
- Sau đó, nó truy cập các đối tượng được đánh dấu và đánh dấu tham chiếu của *chúng*. Tất cả các đối tượng đã truy cập được ghi nhớ, để không truy cập cùng một đối tượng hai lần trong tương lai.
- ...Và như vậy cho đến khi có các tham chiếu không tới cái gì cả (có thể tiếp cận từ gốc).
- Tất cả các đối tượng ngoại trừ những đối tượng được đánh dấu được loại bỏ.

Ví dụ, hãy để cấu trúc đối tượng của chúng ta trông như thế này:

![](garbage-collection-1.png)

Chúng ta có thể thấy rõ một "hòn đảo không thể đến được" ở phía bên phải. Bây giờ hãy xem cách trình thu gom dữ liệu vô nghĩa "đánh dấu và quét (mark-and-sweep)" xử lý nó.

Bước đầu tiên đánh dấu roots:

![](garbage-collection-2.png)

Sau đó, các tham chiếu của chúng được đánh dấu:

![](garbage-collection-3.png)

...Và các tham chiếu của chúng, trong khi có thể:

![](garbage-collection-4.png)

Bây giờ các đối tượng không thể truy cập trong process được coi là không thể tiếp cận và sẽ bị xóa:

![](garbage-collection-5.png)

Đó là khái niệm về cách thức hoạt động của thu gom dữ liệu rác.

Các JavaScript engine áp dụng nhiều tối ưu hóa để làm cho nó chạy nhanh hơn và không ảnh hưởng đến việc thực thi.

Một số tối ưu hóa:

- **Bộ sưu tập thế hệ (Generational collection)** -- các đối tượng được chia thành hai bộ: "bộ mới (new ones)" và "bộ cũ (old ones)". Nhiều đối tượng xuất hiện, làm công việc của chúng và chết nhanh chóng, chúng có thể được dọn dẹp mạnh mẽ. Những cái sót đủ lâu, trở nên "già" và được kiểm tra ít thường xuyên hơn.

- **Bộ sưu tập tăng dần (Incremental collection)** -- nếu có nhiều đối tượng và chúng ta cố gắng đi qua và đánh dấu toàn bộ đối tượng một lượt, có thể mất một khoảng thời gian và đưa ra sự chậm trễ có thể nhìn thấy trong quá trình thực thi. Vì vậy, engine cố gắng chia bộ sưu tập dữ liệu rác thành từng mảnh. Sau đó, các mảnh được thực hiện từng cái một, riêng biệt. Điều đó đòi hỏi một số sổ sách bổ sung giữa chúng để theo dõi các thay đổi, nhưng chúng ta có nhiều sự chậm trễ nhỏ thay vì lớn.

- **Bộ sưu tập thời gian nhàn rỗi (Idle-time collection)** - trình thu gom dữ liệu rác chỉ cố chạy trong khi CPU không hoạt động, để giảm hiệu ứng có thể xảy ra đối với việc thực thi.

Có những tối ưu hóa và hương vị khác của thuật toán thu gom dữ liệu rác. Nhiều như tôi muốn mô tả chúng ở đây, tôi phải chờ, bởi vì các engine khác nhau thực hiện các kỹ thuật và chỉnh sửa khác nhau. Và, điều thậm chí còn quan trọng hơn, mọi thứ thay đổi khi engine phát triển, vì vậy tiến sâu hơn "trước", mà không có nhu cầu thực sự có lẽ không đáng. Trừ khi, tất nhiên, đó là một vấn đề quan tâm thuần túy, sau đó sẽ có một số liên kết cho bạn dưới đây.

## Tóm lược

Những điều chính cần biết:

- Thu gom dữ liệu rác rác được thực hiện tự động. Chúng ta không thể ép buộc hoặc ngăn chặn nó.
- Các đối tượng được giữ lại trong bộ nhớ trong khi chúng có thể tiếp cận.
- Được tham chiếu không giống như có thể tiếp cận (từ root): một gói các đối tượng được liên kết với nhau có thể trở nên không thể tiếp cận được.

Các engine hiện đại thực hiện các thuật toán tiên tiến của thu gom dữ liệu rác.

Một cuốn sách chung "Cẩm nang thu gom dữ liệu rác: Nghệ thuật quản lý bộ nhớ tự động" (R. Jones et al) bao gồm một số trong số đó.

Nếu bạn đã quen thuộc với lập trình cấp thấp (low-level programming), thông tin chi tiết hơn về trình thu gom dữ liệu rác của V8 có trong bài viết [Chuyến tham quan V8: Thu gom dữ liệu rác](http://jayconrod.com/posts/55/a-tour-of-v8-garbage-collection).

[Blog V8](http://v8project.blogspot.com/) cũng xuất bản các bài viết về các thay đổi trong quản lý bộ nhớ theo thời gian. Đương nhiên, để tìm hiểu thu gom dữ liệu rác, bạn nên chuẩn bị tốt hơn bằng cách tìm hiểu về nội bộ V8 nói chung và đọc blog của [Vyacheslav Egorov](http://mrale.ph), người từng làm kỹ sư V8. Tôi đang nói: "V8", bởi vì nó được bao phủ tốt nhất với các bài viết trên internet. Đối với các engine khác, nhiều cách tiếp cận tương tự nhau, nhưng thu gom dữ liệu rác khác nhau ở nhiều khía cạnh.

Kiến thức chuyên sâu về các engine là tốt khi bạn cần tối ưu hóa ở mức độ thấp. Sẽ là khôn ngoan khi lập kế hoạch là bước tiếp theo sau khi bạn quen thuộc với ngôn ngữ này.  
