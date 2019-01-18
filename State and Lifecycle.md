# State và vòng đời của chúng

Ở phần này ta sẽ cùng nhau tìm hiểu về khái niệm của state và vòng đời của chúng trong một React component. Bạn có thể tìm hiểu chi tiết hơn [tại đây](http://google.com).

Hãy cùng nhau xem lại ví dụ về chiếc đồng hồ [trong phần bài học trước](https://github.com/vnknowledge2014/react_docs/blob/master/Rendering%20Elements.md). Trong bài [Hiển thị các phần tử trong React](https://github.com/vnknowledge2014/react_docs/blob/master/Rendering%20Elements.md), ta mới chỉ học được cách thức để cập nhật lại giao diện thương hướng một chiều là gọi hàm `ReactDOM.render()` để thay đổi kết quả hiển thị trên màn hình:

```js
function tick() {
  const element = (
    <div>
      <h1>Hello, world!</h1>
      <h2>It is {new Date().toLocaleTimeString()}.</h2>
    </div>
  );
  ReactDOM.render(
    element,
    document.getElementById('root')
  );
}

setInterval(tick, 1000);
```
**[Chạy thử trên CodePen](http://codepen.io/gaearon/pen/gwoJZk?editors=0010)**

Trong phần này, ta sẽ học cách đóng gói và tái sử dụng một các thực thụ thông qua ví dụ về `Clock` component ở phía trên. Component này sẽ tự cấu hình thời gian theo máy tính của chúng ta và cập nhật lại chính nó theo mỗi giây.

Ta sẽ học cách đóng gói component `Clock` như sau:

```js
function Clock(props) {
  return (
    <div>
      <h1>Hello, world!</h1>
      <h2>It is {props.date.toLocaleTimeString()}.</h2>
    </div>
  );
}

function tick() {
  ReactDOM.render(
    <Clock date={new Date()} />,
    document.getElementById('root')
  );
}

setInterval(tick, 1000);
```
**[Chạy thử trên CodePen](http://codepen.io/gaearon/pen/dpdoYR?editors=0010)**

Kết quả phía trên vẫn còn sót một yêu cầu đó là `Clock` sẽ tự cấu hình bộ đếm thời gian và cập nhật lại giao diện hiển thị theo mỗi giây.

Vậy ý tưởng ở đây là ta chỉ cần viết component này một lần và tự nó phải cập nhật lại giá trị bên trong của chính mình:

```js
ReactDOM.render(
  <Clock />,
  document.getElementById('root')
);
```

Để thực hiện được điều đó, ta cần phải đưa "state" vào trong component `Clock`.

State cũng tựa như props, nhưng có điều nó là dạng biến đóng và hoàn toàn phụ thuộc vào component sở hữu nó.

Trong bài [Các Component và props](https://github.com/vnknowledge2014/react_docs/blob/master/Components%20and%20Props.md) thì việc khai báo hay định nghĩa một component theo chuẩn ES6 sử dụng `class` đã có nói tới việc rằng cách này có được hỗ trợ thêm một số tính năng. Vậy việc khai báo một state cục bộ chính là một tính năng chỉ có thể có được khi sử dụng `class`.

## Chuyển đổi từ chuẩn function sang class
Sẽ có 5 bước để chúng ta tiến hành việc chuyển đổi component `Clock` từ chuẩn function sang class:
    1. Khởi tạo một [class theo ES6](https://developer.mozilla.org/vi/docs/Web/JavaScript/Reference/Classes), với tên class ứng với tên component cũ và thêm vào cú pháp `extends React.Component`.
    2. Thêm vào một phương thước rỗng có tên `render()`.
    3. Sao chép các dòng bên dưới hàm `render()` trong component cũ và chuyển chúng vào trong phương thức `render()` mới nằm trong bước số 2.
    4. Thay thế `props` bằng `this.props` bên trong phần nội dung bên trong hàm `render()` của class `Clock`.
    5. Xoá bỏ toàn bộ phần component `Clock` cũ theo chuẩn function.

```js
class Clock extends React.Component {
  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>It is {this.props.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}
```
**[Chay thử trên CodePen](http://codepen.io/gaearon/pen/zKRGpo?editors=0010)**

Giờ sau khi tái cấu trúc theo chuẩn class thì lúc này phương thước `render` sẽ phải được gọi lại mỗi lần khi việc cập nhật lại dữ liệu xảy ra nhưng phải đảm bảo việc hiển thị trong component `<Clock />` sẽ vẫn chỉ nằm trong cùng một DOM, đồng thời việc khởi tạo class `Clock` chỉ diễn ra duy nhất một lần khi sử dụng. Lúc này ta sẽ cần dùng tới state cũng như các phương thức giúp quản trị vòng đời của nó.

## Sử dụng State cục bộ trong class

Ta sẽ thay thế `date` từ một props trở thành state trong 3 bước:

1. Thay thế `this.props.date` thành `this.state.date` bên trong phương thức `render`:
    
```js
class Clock extends React.Component {
    render() {
        return (
            <div>
                <h1>Hello, world!</h1>
                <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
            </div>
        );
    }
}
```

2. Sử dụng [`class constructor`](https://developer.mozilla.org/vi/docs/Web/JavaScript/Reference/Classes#Constructor) để khởi tạo việc gán giá trị cho state với `this.state`:

```js
class Clock extends React.Component {
    constructor(props) {
        super(props);
        this.state = {date: new Date()};
    }
    render() {
        return (
        <div>
            <h1>Hello, world!</h1>
            <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
        </div>
        );
    }
}
```
Lưu ý rằng ta có thể sử dụng `props` thông qua phương thức constructor:

```js
constructor(props) {
    super(props);
    this.state = {date: new Date()};
}
```

Các class component sẽ luôn gọi tới phương thức constructor với `props` được truyền vào.

3. Loại bỏ props `date` bên trong `<Clock />`:

```js
ReactDOM.render(
  <Clock />,
  document.getElementById('root')
);
```

Ta sẽ đưa mã nguồn của bộ đếm thời gian trở lại chính component của nó.

Kết quả ta thu được như sau:

```js
class Clock extends React.Component {
  constructor(props) {
    super(props);
    this.state = {date: new Date()};
  }

  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}

ReactDOM.render(
  <Clock />,
  document.getElementById('root')
);
```
**[Chạy thử trên CodePen](http://codepen.io/gaearon/pen/KgQpJd?editors=0010)**

Kế tiếp, ta sẽ làm cho `Clock` có thể tự cấu hình bộ đếm thời gian và tự cập nhật lại dữ liệu cho chính mình sau mỗi giây.

## Sử dụng các phương thức quản lý vòng đời trong class
Trong ứng dụng có nhiều component, việc quản lý vòng đời rất quan trọng khi giúp giải phóng các tài nguyên bị chiếm dụng bởi các component khi chúng kết thúc vòng đời.

Ta muốn việc [cấu hình hiển thị giá trị của bộ đếm thời gian](https://developer.mozilla.org/vi/docs/Web/API/WindowOrWorkerGlobalScope/setInterval) cho `Clock` vào bất cứ khi nào nó được gọi để trả ra kết quả trong DOM lần đầu tiên. Để làm điều đó ta có những phương thức được gọi là "mounting" trong React.

Đồng thời với việc trên, ta cũng muốn việc [loại bỏ giá trị hiển thị trong bộ đếm thời gian](https://developer.mozilla.org/vi/docs/Web/API/WindowTimers/clearInterval) vào bất kì khi nào trong DOM trực thuộc `Clock`. Trong React các phương thức để làm việc này gọi chung là "unmounting".

Ta có thể sử dụng các phương thức đặc biệt trong component theo chuẩn class để khởi chạy các đoạn mã dùng đến việc "mount" và "unmount":

```js
class Clock extends React.Component {
  constructor(props) {
    super(props);
    this.state = {date: new Date()};
  }

  componentDidMount() {

  }

  componentWillUnmount() {

  }

  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}
```

Các phương thức này được gọi là "lifecycle method - hay dịch nôm là các phương thức quản lý vòng đời".

Với phương thức `componentDidMount()` thì những gì nằm trong nó sẽ chạy sau khi component đã được hiển thị thành dạng DOM. Và đây chính là nơi thuận tiện để ta sử dụng bộ đếm thời gian:

```js
  componentDidMount() {
    this.timerID = setInterval(
      () => this.tick(),
      1000
    );
  }
```

Cũng cần lưu ý về cách ta sử lưu lại timerID bằng `this`.

Trong khi `this.props` tự cài đặt bởi React và `this.sate` có một ý nghĩa đặc biệt thì ta có thể tự do thêm vào các trường bổ sung vào class nếu cần phải lưu trữ thứ gì đó không thuộc vào luồng dữ liệu như timerID.

Ta sẽ loại bỏ bộ đếm thời gian bằng phương thức `componentWillUnMount()`:

```js
  componentWillUnmount() {
    clearInterval(this.timerID);
  }
```

Cuối cùng, ta triển khai phương thức `tick()` mà component `Clock` sẽ chạy mỗi giây.

Nó sẽ sử dụng `this.setState()` để cập nhật theo tiến trình cho state thuộc component:

```js
class Clock extends React.Component {
  constructor(props) {
    super(props);
    this.state = {date: new Date()};
  }

  componentDidMount() {
    this.timerID = setInterval(
      () => this.tick(),
      1000
    );
  }

  componentWillUnmount() {
    clearInterval(this.timerID);
  }

  tick() {
    this.setState({
      date: new Date()
    });
  }

  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}

ReactDOM.render(
  <Clock />,
  document.getElementById('root')
);
```
**[Chạy thử trên CodePen](http://codepen.io/gaearon/pen/amqdNA?editors=0010)**

Vậy là đồng hồ sẽ được cập nhật giá trị mới mỗi giây.

Cùng tóm tắt nhanh những điều diễn ra theo thứ tự mà các phương thức được gọi:
    1. Khi `<Clock />` được hàm `ReactDOM.render()` kích hoạt, React sẽ gọi tới constructor của component `Clock`. Khi đó `Clock` cần hiển thị ra màn hình thời gian tại thời điểm hiện tại, nó sẽ khởi tạo `this.state` với một đối tượng bao gồm thời gian hiện tại. Ta sẽ cập nhật lại state sau.
    2. React gọi phương thức `render` trong component `Clock`. Đây chính là cách mà React theo dõi những thứ được hiển thị trên màn hình. Lúc này React cập nhật DOM để khớp với giá trị được đẩy ra từ `Clock`.
    3. Khi giá trị của `Clock` được thêm vào DOM, React gọi tới `componentDidMount()`. Lúc này bên trong component `Clock` sẽ yêu cầu trình duyệt trả lại giá trị của bộ đếm thời gian thông qua phương thức `tick()` mỗi giây.
    4. Mỗi giây trình duyệt sẽ gọi tới phương thức `tick()`. Bên trong component `Clock` đưa ra tiến trình cập nhật giao diện thông qua `setState()` với một đối tượng ghi nhận thời gian hiện tại. Nhờ có `setState()` mà React biết được lúc nào state thay đổi, để gọi lại phương thức `render()` để cập nhật lại giá trị được hiển thị trên màn hình. Vậy đi vào chi tiết của ví dụ thì `this.state.date` trong phương thức `render()` lúc này đã thay đổi và thời gian hiển thị được cập nhật. React cập nhật thay đổi này vào DOM.
    5. Nếu `Clock` đã bị xoá khỏi DOM, React sẽ gọi tới phương thức `componentWillUnmount()` để dừng bộ đếm thời gian lại.

## Sử dụng state đúng cách
Có ba điều ta cần biết về `setState()`.

### Không chỉnh sửa state trực tiếp
Ví dụ sau đây sẽ cho thấy component sẽ không được cập nhật lại việc hiển thị:

```js
// Wrong
this.state.comment = "Hello";
```

Thay vào đó hãy dùng `setState()`:

```js
// Correct
this.setState({comment: 'Hello'});
```

Đó là nới duy nhất ta có thể gán `this.state` như trong constructor.

### Cập nhật state có thể không đồng bộ
React có thể gom nhiều `setState()` để cập nhật trong một lần nhằm đạt được hiệu xuất tốt.

`this.props` và `this.state` có thể được cập nhật bất đồng bộ, chính vì thế mà không nên dựa vào các giá trị để tính toán cho state kế tiếp.

Ví dụ, đoạn mã này có thể sẽ không hoạt đống đúng khi cập nhật biến counter:

```js
// Wrong
this.setState({
  counter: this.state.counter + this.props.increment,
});
```

Để sửa lại đoạn mã trên ta nên sử dụng hai lần `setState()` để nhận vào một hàm hơn là một đối tượng. Hàm này sẽ trả ra state trước và đóng vai là tham số đầu tiên trong hàm, cùng thời điểm đó props sẽ cập nhật, lúc này props là tham số thứ hai trong hàm:

```js
// Correct
this.setState((state, props) => ({
  counter: state.counter + props.increment
}));
```

Ngoài ra thì có thể sử dụng [arrow function](https://developer.mozilla.org/vi/docs/Web/JavaScript/Reference/Functions/Arrow_functions), nhưng nó cũng hoạt động với các hàm chính quy:

```js
// Correct
this.setState(function(state, props) {
  return {
    counter: state.counter + props.increment
  };
});
```

### Hợp nhất state
Khi gọi `setState()`, React hợp nhất đối tượng ta cung cấp vào state hiện tại.

Ví dụ, state của ta có thể bao gồm vài biến đọc lập:

```js
constructor(props) {
    super(props);
    this.state = {
      posts: [],
      comments: []
    };
  }
```

Lúc này ta có thể cập nhật chúng đọc lập bằng cách sử dụng `setState()`:

```js
componentDidMount() {
    fetchPosts().then(response => {
      this.setState({
        posts: response.posts
      });
    });

    fetchComments().then(response => {
      this.setState({
        comments: response.comments
      });
    });
  }
```

Việc hợp nhất này là dạng nông, vậy `setState({comments})` giữ nguyên `this.state.posts`, nhưng lại thay thế `this.state.comments`.

## Ngược dòng dữ liệu
Cả component cha và con đều không thể biết liệu một component nào là dạng có state - stateful hay không có state - stateless, và chúng không quan tâm liệu component đó được viết theo chuẩn function hay class.

Đó là lý do tại sao state được đóng gói và chỉ có phạm vi bên trong component sở hữu nó. Nó không thể được truy cập bởi bất kì component nào khác ngoài component nó thuộc về.

Một component có thể chọn cách truyền state xuống component con thông qua props:

```js
<h2>It is {this.state.date.toLocaleTimeString()}.</h2>
```

Cách này cũng hoạt động tốt với các component do người dùng tự định nghĩa:

```js
<FormattedDate date={this.state.date} />
```

Component `FormattedDate` sẽ nhận `date` vào props của nó và không cần biết thứ đó đến từ state của `Clock`, từ props của `Clock`, hay được gõ vào:

```js
function FormattedDate(props) {
  return <h2>It is {props.date.toLocaleTimeString()}.</h2>;
}
```
**[Chạy thử trên CodePen](http://codepen.io/gaearon/pen/zKRqNB?editors=0010)**

Luồng dữ liệu dạng này thường được gọi là "top-down" hoặc "unidirectional". Bất kỳ state nào luôn được sở hữu bởi một component cụ thể và bất kỳ dữ liệu hoặc giao diện người dùng nào có sử dụng state đó thì chỉ có thể ảnh hưởng đến các thành phần "phía dưới" nó.

Hãy tưởng tượng cây component như một thác nước props, mỗi state của component giống như một nguồn nước chảy vào một điểm tuỳ ý nhưng lại không thực sự chảy xuống.

Để thể hiện toàn bộ các component là hoàn toàn độc lập, ta có thể khởi tạo một component `App` để hiển thị ra 3 component `Clock`:

```js
function App() {
  return (
    <div>
      <Clock />
      <Clock />
      <Clock />
    </div>
  );
}

ReactDOM.render(
  <App />,
  document.getElementById('root')
);
```
**[Chạy thử trên CodePen](http://codepen.io/gaearon/pen/vXdGmd?editors=0010)**

Mỗi `Clock` được thiết lập một bộ đếm thời gian riêng và cập nhật cũng hoàn toàn đọc lập.

Trong các ứng dụng React, thì liệu một component stateful hay stateless có thể được xem xét chi tiết việc khi component ấy thực thi có thay đổi theo thời gian. Ta có thể sử dung stateless component bên trong một stateful component và ngược lại.