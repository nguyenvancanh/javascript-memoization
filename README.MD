# Memoization trong Javascrip

**1. Memoization là gì?**
- Memoization là một kỹ thuật tối ưu hóa giúp tăng tốc các ứng dụng bằng cách lưu trữ kết quả của các lệnh gọi hàm tiêu tốn nhiều tài nguyên (thời gian và bộ nhớ) do tính toán phức tạp vào trong bộ nhớ cache, và nhanh chóng lấy kết quả ra khi cần thiết.
=> Đơn giản là: khi một hàm tiêu tốn quá nhiều thời gian và bộ nhớ được gọi làn đầu tiên, kết quả của nó sẽ được lưu trữ lại trong bộ nhớ cache, khi hàm đó được gọi lại từ lần thứ 2 trở đi, thì kết quả sẽ được đọc từ cache ra chứ không phải gửi request lên server vào tiến hành tính toán lại từng bước như lần đầu tiên.

**2. Memoization vô cùng quan trọng**

Hãy xem ví dụ sau để thấy được tầm quan trọng của memoization, 

- Bạn đang ngồi đọc một cuốn tiểu thuyết mới và có số lượng trang khá lớn ở công viên, thì có một người lạ đi qua và hỏi bạn tên cuốn tiểu thuyết và tác giả của nó là gì? Bạn phải lật lại trang bìa và trả lời câu hỏi của người đó. Nhưng càng ngày lại càng có nhiều người đi qua và càng có nhiều người hỏi bạn câu hỏi đó. Bạn là người tốt bụng, và luôn sẵn sàng trả lời bất kỳ ai. Vì vậy, bạn tiếp tục dừng lại rồi lật lại trang bìa của cuốn tiểu thuyết và trả lời từng người một. Câu hỏi đặt ra ở đây là:

```
Bạn sẽ tiếp tục lật lại trang bìa và trả lời câu hỏi của mọi người, hoặc bạn sẽ bắt đầu cung cấp dữ liệu từ bộ nhớ, giúp bạn tiết kiệm thời gian hơn?
```
Đương nhiên là trong thực tế, chúng ta sẽ xử dụng bộ nhớ của mình là cung cấp ngay thông tin mà đã được lưu lại trong bộ nhớ của chúng ta, Memoization cũng vậy, khi một method được cung cấp đầu vào (input), nó sẽ tiến hành xử lý các bước, lưu vào bộ nhớ trước khi trả về kết quả. Khi cùng một đầu vào được truyền tới hàm đó trong tương lai, nó chỉ đơn giản lấy kết quả được lưu trước đó và trả về chứ không cần phải tính toán lại các bước trên.

**Memozation hoạt động như thế nào?**

Memozation trong javascript được xây dựng chủ yếu dựa trên 2 khái niệm:

- Đóng kín (Closures)
- Higher Order Functions(returning functions from functions)

 Để hiểu thế nào là Closures, các bạn có thể theo dõi ví dụ sau:

```
function foo(a) {
  var b = a + 2;
  function bar(c) {
    console.log(a, b, c);
}
  bar(b * 2);
}

foo(3); // 3, 5, 10
```
Đoạn code này bao gồm: 

- Một global scope khai báo duy nhất một method đó là foo
- Trong method foo, định nghĩa a, b và method bar()
- Trong method bar lại định nghĩa c

Nhìn vào đoạn code trên , chúng ta nhận thấy method bar() có quyên truy cập vào biến a, b vì nó nằm trong method foo(). Lưu ý rằng, chúng ta lưu trữ thành công method bar() trong môi trường của nó. Vì vậy, ta nói rằng method bar() là đóng trong foo()

Vậy còn **returning functions from functions**

- Các hàm hoạt động dựa trên hàm khác, bằng cách lấy chúng làm tham số hoặc trả về giá trị là hầm đó được gọi là higher-order functions. Xem ví dụ sau:

```
function foo(){
  var a = 2;

  function bar() {
    console.log(a);
  }
  return bar;
}
var baz = foo();
baz();//2
```
Về định nghĩa thì chỉ có vậy, bây giờ chúng ta sẽ thử thực hành một số bài tập cơ bản xem sao.

**Tính số tiếp theo của Dãy Fibonaci**

```
Dãy fibonaci là dãy số được bắt đầu bởi số 0 hoặc số 1, hoặc 2 số 1 và các số tiếp theo phải tuân theo quy tắc, số đứng sau bằng tổng 2 số đứng trước cộng lại
```

```
0, 1, 1, 2, 3, 5, 8, 13, 21, 34, 55, 89, 144,...
```
hoặc

```
1, 1, 2, 3, 5, 8, 13, 21, 34, 55, 89, 144, …
```
Bài tập là tính số tiếp theo của dãy Fibonaci. Nếu chỉ dựa vào định nghĩa của dãy Fibonaci thì chúng ta sẽ có đoạn code sau:

```
function fibonacci(n) {
    if (n <= 1) {
        return 1
    }
    return fibonacci(n - 1) + fibonacci(n - 2);
}
```

Đơn giản, ngắn gọn và vô cùng chính xác. Nhưng vấn đề ở đay ra, khi n lớn, thì chúng ta phải gọi lại liên tục tới function của số fibonaci trước đó (n -1), cho tới khi chúng ta gọi tới được method tính dãy fibonaci của n = 1, hoặc n = 0 (những số mà chúng ta đã biết chính xác giá trị của nó). Điều này gọi là tính toán dư thừa, và Memozation sẽ loại bỏ hoàn toàn vấn đề này

