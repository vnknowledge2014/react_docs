# Hiển thị các phần tử trong React

Các phần tử hay còn được gọi là các `element` là các thành phần nhỏ nhất trong ứng dụng React.

Một `element` sẽ giống như ví dụ dưới đây:

```js
const element = <h1>Hello, world</h1>;
```

Không giống như các `DOM element` trong trình duyệt, Các `element` trong react là [plain object](https://stackoverflow.com/questions/52453407/the-different-between-object-and-plain-object-in-javascript), và chính vì lẽ đó nó cũng tốn ít chi phí khi khởi tạo. React DOM sẽ lo việc cập nhật các DOM sao cho khớp với các `element`.

*
    *Lưu ý:*
    Người ta có thể nhầm lẫn các element với một khái niệm được biết đến rộng hơn của "components". Chúng ta sẽ được biết rõ hơn về các component trong [bài học sau](https://github.com/vnknowledge2014/react_docs/blob/master/Components%20and%20Props.md). Các element là các component được "tạo ra", và phần bên dưới đây sẽ giúp bạn làm rõ các vấn đề bạn còn đang thắc mắc.

## Việc hiển thị một phần tử bên trong DOM
Có một thẻ `<div>` nằm đâu đó trong tập tin HTML của bạn:

```js
<div id="root"></div>
```

Bên trong thẻ `<div>` phía trên có id là `root` bởi mọi thứ bên trong thẻ này sẽ được quản lý bởi React DOM.

Các ứng dụng xây dựng bằng React thường có chỉ có một `root` DOM. Nếu bạn tích hợp React bên trong một ứng dụng đã tồn tại từ trước, thì có lẽ bạn cần phải cô lập và chia tách nhiều root DOM theo như cách bạn muốn.

Việc hiển thị một React element bên trong root DOM sẽ được xử lý thông qua `ReactDOM.render()`:

```js
const element = <h1>Hello, world</h1>;
ReactDOM.render(element, document.getElementById('root'));
```
**[Chạy thử trên CodePen](https://reactjs.org/redirect-to-codepen/rendering-elements/render-an-element)**

Kết quả ta có là một thông điệp "Hello, world" được hiển thị trên màn hình trình duyệt.

## Cập nhật một element đã được hiển thị
Các react element được xếp vào dạng [immutable](https://en.wikipedia.org/wiki/Immutable_object) - có nghĩa là không thể thay đổi. Bạn chỉ có thể khởi tạo một lần cho một element, và không thể thay đổi các phần tử con hoặc các thuộc tính. Một element giống như khung hình trong một bộ phim: nó đại diện cho giao diện tại một thời điểm nhất định.

Với những hiểu biết của chúng ta, thì chỉ có duy nhất một cách để cập nhật giao diện thông qua việc tạo mới một element và đẩy nó vào `ReactDOM.render()`.

Chúng ta sẽ xem xét ví dụ sau đây:

```js
function tick() {
  const element = (
    <div>
      <h1>Hello, world!</h1>
      <h2>It is {new Date().toLocaleTimeString()}.</h2>
    </div>
  );
  ReactDOM.render(element, document.getElementById('root'));
}

setInterval(tick, 1000);
```
**[Chạy thử trên CodePen](https://reactjs.org/redirect-to-codepen/rendering-elements/update-rendered-element)**

Ở ví dụ trên hàm `ReactDOM.render()` sẽ được gọi lại mỗi giây thông qua hàm callback `setInterval()`.

*
    **Lưu ý:**
    Trong thực tế, hầu hết các ứng dụng react chỉ gọi `ReactDOM.render()` một lần duy nhất. Trong phần tiếp theo chúng ta sẽ học về việc làm thế nào để đóng gói mã nguồn bên trong các [stateful component](https://github.com/vnknowledge2014/react_docs/blob/master/State%20and%20Lifecycle.md).<br/><br/>
    Bạn không nên bỏ qua các phần tiếp sau đây bởi nó sẽ giúp bạn hiểu và làm rõ các vấn đề liên quan trong những bài học kế tiếp.

## React chỉ cập nhật khi thực sự cần thiết
React DOM được so sánh với element và các element con ở phía lần hiển thị trước, và chỉ cập nhật DOM khi thực sự cần thiết để đưa nó về trạng thái thích hợp.

Bạn có thể kiểm chứng thông qua [ví dụ về đồng hồ ở phía trên](https://reactjs.org/redirect-to-codepen/rendering-elements/update-rendered-element) và mở công cụ dành cho nhà phát triển trên trình duyệt để theo dõi việc cập nhật được diễn ra như thế nào:

![](https://reactjs.org/granular-dom-updates-c158617ed7cc0eac8f58330e49e48224.gif)

Mặc dù việc khởi tạo một element được mô hình hoá trên toàn bộ cây giao diện theo mỗi giây, nhưng chỉ duy nhất chuỗi hiển thị thời gian được cập nhật sự thay đổi thông qua React DOM.

Với kinh nghiệm của đội ngũ phát triển React, thì việc nên suy nghĩ về cách giao diện nên được xem xét việc thay đổi theo từng thời điểm hơn là việc quan tâm đến cách thay đổi nó theo thời gian để loại bỏ toàn bộ các lớp lỗi.