# Giới thiệu về JSX

Hãy xem xét biến được khai báo dưới đây:

```js
const element = <h1>Hello, world</h1>;
```

Cú pháp trên không phải là một `string` hay `html`.

Thứ chúng ta thấy ở đoạn mã trên chính là JSX, nó là một phiên bản mở rộng của Javascript. JSX được sử dụng để dựng giao diện trong React, nó làm chúng ta nhớ về một dạng ngôn ngữ mẫu - template language, nhưng có điều là JSX sở hữu toàn bộ tính năng của Javascript.

JSX giúp chúng ta xây dựng các phần tử trong React. Chúng ta sẽ tìm hiểu việc hiển thị các phần tử thành dạng DOM trong [phần kế tiếp](http://google.com). Tiếp theo đây chúng ta sẽ tìm hiểu kiến thức cơ bản về JSX.

## Tại sao lại sử dụng JSX ?
React đã nắm bắt được việc hiển thị logic kết hợp cùng với giao diện như: làm cách nào các sự kiện được xử lý, cách các trạng thái thay đổi theo thời gian và cách dữ liệu được chuẩn bị cho việc hiển thị.

Thay vì sử dụng các công nghệ để chia tách giữa logic và xử lý hiển thị nội dung ra các tập tin khác nhau, thì React hướng tới việc sử dung một nguyên tắc thiết kế gọi tắt là SoC - [Separation of concerns](https://en.wikipedia.org/wiki/Separation_of_concerns), nguyên tắc này giúp định hướng việc tập trung vào chia tách các thành phần riêng biệt hướng tới hạn chế việc ràng buộc lẫn nhau và chúng được gọi là các `component`. Chúng ta sẽ quay lại chủ đề này trong [phần sau](http://google.com), nhưng nếu bạn vẫn cảm thấy lăn tăn về việc kết hợp nội dụng hiển thị với mã Javascript thì [video này sẽ thuyết phục bạn](https://www.youtube.com/watch?v=x7cQ3mrcKaY).

React [không yêu cầu sử dụng JSX](http://google.com), nhưng hầu hết mọi người đều thấy nó hữu ích và giúp ta tiếp cận với cái nhìn trực quan hơn khi làm việc với giao diện bên trong mã Javascript. Nó cho phép React hiển thị các thông báo lỗi và các cảnh báo tương đối hữu dụng. 

Vậy hãy cùng nhau tìm hiểu rõ hơn về JSX trong react.

## Nhúng biểu thức vào trong JSX
Trong ví dụ ngay bên dưới, chúng ta khai báo ra một biến `name` và sẽ sử dụng nó thông qua việc bọc biến đó vào trong cặp ngoặc nhọn bên trong JSX:

```js
const name = 'Josh Perez';
const element = <h1>Hello, {name}</h1>;

ReactDOM.render(
    element,
    document.getElementById('root')
);
```

Bạn có thể sử dụng bất kì biểu thức nào trong Javascript được bảo bởi cặp ngoặc nhọn bên trong JSX. Ví dụ, `2 + 2`, `user.firstName`, hoặc `formatName(user)` tất cả đều là các biểu thức hợp lệ trong Javascript.

Phần ví dụ tiếp dưới đây chúng ta sẽ nhúng kết quả được trả về từ hàm `formatName(user)` vào thẻ `<h1>`.

```js
function formatName(user) {
  return user.firstName + ' ' + user.lastName;
}

const user = {
  firstName: 'Harper',
  lastName: 'Perez'
};

const element = (
  <h1>
    Hello, {formatName(user)}!
  </h1>
);

ReactDOM.render(
  element,
  document.getElementById('root')
);
```
**[Chạy thử trên CodePen](https://reactjs.org/redirect-to-codepen/introducing-jsx)**

Chúng ta chia JSX thành nhiều dòng để dễ dàng theo dõi. Mặc dù không cần thiết, nhưng khi thực hiện điều này, chúng ta được khuyến cáo là nên gói JSX trong cặp ngoặc đơn để tránh những rắc rối của việc [tự động thêm dấu chấm phẩy `;` gây ra lỗi](https://stackoverflow.com/questions/2846283/what-are-the-rules-for-javascripts-automatic-semicolon-insertion-asi).

## JSX cũng là một biểu thức
Sau khi biên dịch, các biểu thức JSX trở thành các lệnh gọi hàm Javascript thông thường và đánh giá các đối tượng.

Điều này có nghĩa là bạn có thể sử dụng JSX bên trong các câu lệnh `if` và `for`, hay việc gán biến, hoặc nó cũng có thể trở thành một đối số và trả ra kết quả là một JSX từ một hàm:

```js
function getGreeting(user) {
  if (user) {
    return <h1>Hello, {formatName(user)}!</h1>;
  }
  return <h1>Hello, Stranger.</h1>;
}
```

## Chỉ định thuộc tính với JSX
Bạn có thể sử dụng dấu ngoặc kép để chỉ định chuỗi ký tự là thuộc tính:

```js
const element = <div tabIndex="0"></div>;
```

Bạn cũng có thể sử dụng dấu ngoặc nhọn để nhúng một biểu thức JavaScript vào trong một thuộc tính:

```js
const element = <img src={user.avatarUrl}></img>;
```

Không đặt dấu ngoặc kép quanh dấu ngoặc nhọn khi nhúng biểu thức Javascript vào trong thuộc tính. Hãy nên sử dụng dấu ngoặc kép (cho giá trị `string`) hoặc dấu ngoặc nhọn (cho biểu thức), nhưng không sử dụng cả hai trong cùng một thuộc tính.

*
    **Cảnh báo:**
    JSX gần Javascipt hơn HTML, React DOM sử dụng quy ước đặt tên thuộc tính **camelCase** thay vì tên thuộc tính HTML. Ví dụ, `class` trở thành [`className`](https://developer.mozilla.org/en-US/docs/Web/API/Element/className) trong JSX và `tabindex` trở thành [`tabIndex`](https://developer.mozilla.org/en-US/docs/Web/API/HTMLElement/tabIndex).


## Làm rõ thành phần con trong JSX
Nếu một thẻ rỗng, bạn có thẻ đóng nó ngay lập tức `/>` như trong XML:

```js
const element = <img src={user.avatarUrl} />;
```

Thẻ JSX có thể bao hàm các thẻ con:

```js
const element = (
  <div>
    <h1>Hello!</h1>
    <h2>Good to see you here.</h2>
  </div>
);
```

## JSX ngăn ngừa Injection Attacks
Việc nhúng các giá trị đầu vào của người dùng trong JSX là thực sự an toàn:

```js
const title = response.potentiallyMaliciousInput;
// This is safe:
const element = <h1>{title}</h1>;
```

Theo mặc định, React DOM sẽ [giấu đi](https://stackoverflow.com/questions/7381974/which-characters-need-to-be-escaped-on-html) mọi giá trị được nhúng trong JSX trước khi hiển thị chúng. Do đó, nó đảm bảo rằng bạn không bao giờ có thể nhồi bất cứ thứ gì mà không được khai báo rõ ràng trong ứng dụng của bạn. Tất cả mọi thứ được chuyển đổi thành một chuỗi trước khi được hiển thị. Điều này giúp ngăn chặn các cuộc tấn công [XSS (cross-site-scripting)](https://en.wikipedia.org/wiki/Cross-site_scripting).

## JSX đại diện cho các đối tượng
Babel biên dịch mã JSX thành JS và dùng hàm `React.createElement()` để chạy việc hiển thị.

Hai ví dụ này giống hệt nhau:

```js
const element = (
  <h1 className="greeting">
    Hello, world!
  </h1>
);
```

```js
const element = React.createElement(
  'h1',
  {className: 'greeting'},
  'Hello, world!'
);
```

`React.createElement()` thực hiện một vài kiểm tra giúp mã bạn viết tránh gặp lỗi nhưng về cơ bản thì nó tạo ra một đối tượng kiểu như sau:

```js
// Note: this structure is simplified
const element = {
  type: 'h1',
  props: {
    className: 'greeting',
    children: 'Hello, world!'
  }
};
```

Các đối tượng này được gọi là "React element". Bạn có thể nghĩa về chúng như những mô tả về các thứ bạn muốn hiển thị trên màn hình. React đọc các đối tượng này và sử dụng chúng để xây nên các DOM cũng như cập nhật chúng.

Chúng ta sẽ khám phá việc hiển thị các "React element" thành DOM trong phần sau.


*
    **Mẹo:**
    Khuyến cáo bạn nên sử dụng ["Babel"](https://babeljs.io/docs/en/editors/) cho trình soạn thảo mã nguồn với ES6 và JSX giúp đánh dấu - nhấn mạnh vào cú pháp để dễ nhận biết hay tiện cho việc đọc mã nguồn được rõ ràng hơn. 