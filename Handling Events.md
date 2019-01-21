# Xử lý sự kiện trong React

Xử lý các sự kiện với các element trong React rất giống với việc xử lý các sự kiện trên các DOM element. Có một số khác biệt về cú pháp:

    * Các sự kiện trong React đều được đặt tên theo chuẩn **camelCase** thay vì viết thường.
    * Với JSX ta truyền vào một hàm để bắt sự kiện thay vì truyền vào một chuỗi.

Ví dụ, với HTML:

```js
<button onclick="activateLasers()">
  Activate Lasers
</button>
```

Với React sẽ có chút khác biệt:

```js
<button onClick={activateLasers}>
  Activate Lasers
</button>
```

Một sự khác biệt nữa là ta không thể trả về giá trị `false` cho việc loại bỏ các hành vi mặc định trong React, thay vào đó ta sử dụng `preventDefault` để làm việc này. Ví dụ, với HTML để loại bỏ hành vi mở một trang mới khi ta click vào đường dẫn sẽ có dạng như sau:

```js
<a href="#" onclick="console.log('The link was clicked.'); return false">
  Click me
</a>
```

Nhưng trong React, nó sẽ có dạng sau:

```js
function ActionLink() {
  function handleClick(e) {
    e.preventDefault();
    console.log('The link was clicked.');
  }

  return (
    <a href="#" onClick={handleClick}>
      Click me
    </a>
  );
}
```

Trong ví dụ trên, `e` là biến chỉ các sự kiện được truyền vào. Việc định nghĩa này được React tuân thủ theo chuẩn [W3C](https://www.w3.org/TR/DOM-Level-3-Events/), vậy là không còn phải lo lắng về việc tương thích đa trình duyệt. Để tìm hiểu kĩ hơn về biến sự kiện hãy tuy cập đường dẫn này [SyntheticEvent](https://reactjs.org/docs/events.html).

Khi sử dụng React ta hoàn không cần phải dùng đến `addEventListener` để theo dõi sử thay đổi của các DOM element sau khi khởi tạo. Thay và đó, chỉ cần cung cấp một listener khi element được khởi tạo lần đầu.

Khi định nghĩa một component sử dụng [ES6 class](https://developer.mozilla.org/vi/docs/Web/JavaScript/Reference/Classes), thì mỗi sự kiện chính là một phương thức được khai báo trong class. Ví dụ, component `Toggle` sẽ hiển thị một button để người dùng chỉ định trạng thái của nó giữa "ON" và "OFF" thông qua việc sử dụng state:

```js
class Toggle extends React.Component {
  constructor(props) {
    super(props);
    this.state = {isToggleOn: true};

    // This binding is necessary to make `this` work in the callback
    this.handleClick = this.handleClick.bind(this);
  }

  handleClick() {
    this.setState(state => ({
      isToggleOn: !state.isToggleOn
    }));
  }

  render() {
    return (
      <button onClick={this.handleClick}>
        {this.state.isToggleOn ? 'ON' : 'OFF'}
      </button>
    );
  }
}

ReactDOM.render(
  <Toggle />,
  document.getElementById('root')
);
```
**[Chạy thử trên CodePen](http://codepen.io/gaearon/pen/xEmzGg?editors=0010)**

Hãy cẩn trọng khi sử dụng `this` với các hàm callback trong JSX. Trong Javascript, các phương thức trong class sẽ mặc định không được [bind](https://developer.mozilla.org/vi/docs/Web/JavaScript/Reference/Global_objects/Function/bind). Nếu lỡ quên sử dụng bind cho `this.handleClick` và truyền vào `onClick`, thì `this` sẽ trả về `undefined` khi hàm được gọi.

Nó không phải là hành vi do React quy định, mà do [Javascript](https://www.smashingmagazine.com/2014/01/understanding-javascript-function-prototype-bind/) quy định về việc các hàm hoạt động như thế nào. Nói chung là nếu việc tham chiếu tới phương thức mà thiếu cặp ngoặc tròn `()` phía sau đó giống như `onClick={this.handleClick}`, thì phải sử dụng bind đối với phương thức ấy.

Nếu việc dùng `bind` gây ra sự phiền toái, thì có hai cách để xử lý. Nếu ta sử dụng cú pháp thử nghiệm [public class](https://babeljs.io/docs/plugins/transform-class-properties/), thì việc gọi bind chính xác các hàm callback sẽ được diễn ra:

```js
class LoggingButton extends React.Component {
  // This syntax ensures `this` is bound within handleClick.
  // Warning: this is *experimental* syntax.
  handleClick = () => {
    console.log('this is:', this);
  }

  render() {
    return (
      <button onClick={this.handleClick}>
        Click me
      </button>
    );
  }
}

```

Cú pháp này được áp dụng mặc định trên [Create React App](https://github.com/facebookincubator/create-react-app).

Nếu không muốn sử dụng cú pháp trên, thì có thể sử dụng [arrow function](https://developer.mozilla.org/vi/docs/Web/JavaScript/Reference/Functions/Arrow_functions):

```js
class LoggingButton extends React.Component {
  handleClick() {
    console.log('this is:', this);
  }

  render() {
    // This syntax ensures `this` is bound within handleClick
    return (
      <button onClick={(e) => this.handleClick(e)}>
        Click me
      </button>
    );
  }
}
```

Có một vấn đề với cú pháp dạng này là hàm callback sẽ được khởi tạo mỗi thời điểm khác nhau khi `LoggingButton` được hiển thị. Trong hầu hết các trường hợp, thì mọi thứ đều ổn. Tuy nhiên, nếu ta truyền callback như một prop xuống các component thấp hơn, thì trường hợp bị lặp hay bị hiển thị thừa component sẽ xảy ra. Cuối cùng thì việc sử dụng bind trong constructor hoặc sử dụng cú pháp thử nghiệm ở trên vẫn luôn được khuyến khích để không còn phải bận tâm về vấn đề liên quan đến hiệu năng này.

## Truyền đối số vào hàm xử lý sự kiện
Bên trong một vòng lặp, nó thường muốn truyền một loạt các tham số tới một hàm xử lý sự kiện. Ví dụ, nếu `id` là một hàng các ID, thì sẽ hoạt động như sau:

```js
<button onClick={(e) => this.deleteRow(id, e)}>Delete Row</button>
<button onClick={this.deleteRow.bind(this, id)}>Delete Row</button>
```

Hai dòng mã ở trên đều giống nhau, bởi việc dùng [arrow function](https://developer.mozilla.org/vi/docs/Web/JavaScript/Reference/Functions/Arrow_functions) và [Function.prototype.bind](https://developer.mozilla.org/vi/docs/Web/JavaScript/Reference/Global_objects/Function/bind) đều không có sự khác biệt.

Trong hai trường hợp, thì `e` là đại diện cho các sự kiện trong React được truyền vào như tham số thứ hai ngay sau ID. Với arrow function, thì việc truyền tham số cho thấy sự rõ ràng, minh bạch. Nhưng đối với `bind` thì bất kì đối số tiếp theo là gì thì đều được truyền đi tự động.

