# Các component và props

Các component giúp ta chia giao diện ra thành các phần tử đủ nhỏ, độc lập, có thể tái sử dụng lại và hướng suy nghĩ này giúp việc xây dựng giao diện chỉ đơn giản là ghép các phần ấy này lại với nhau. Trong bài này sẽ giúp bạn làm quen với ý tưởng này. Bạn có thể tìm hiểu sâu hơn về các [component API - Application Programming Interface tại đây](http://google.com).

Về mặt khái niệm, các component giống như các hàm trong Javascript. Chúng có "props" để nhận vào các giá trị tuỳ chọn và trả ra các React element đã được cấu hình để hiển thị lên màn hình.

## Hàm và các lớp component
Cách đơn giản nhất để định nghĩa một component thông qua một hàm trong Javascript:

```js
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}
```

Hàm trên là một component tiêu chuẩn của React bởi nó sở hữu duy nhất đối tượng đầu vào "props" (hay còn được hiểu là các thuộc tính) chưa dữ liệu và trả ra một React element. Các component còn có tên gọi khác là "function component" bởi chúng là các [hàm litteral trong Javascript](https://stackoverflow.com/questions/12314905/exact-meaning-of-function-literal-in-javascript).

Bạn có thể sử dụng cú pháp [`class trong ES6`](https://developer.mozilla.org/vi/docs/Web/JavaScript/Reference/Classes) để định nghĩa một component:

```js
class Welcome extends React.Component {
  render() {
    return <h1>Hello, {this.props.name}</h1>;
  }
}
```

Hai component trên đều hợp lệ để sử dụng trong React.

Việc sử dụng `class` có những tính năng bổ sung mà chúng ta sẽ tìm hiểu thêm trong [phần kế tiếp](https://reactjs.org/docs/state-and-lifecycle.html). Trong những phần tiếp theo trong bài này và một số bài khác chúng ta sẽ tạm sử dụng việc khai báo function component để có sự thống nhất xuyên suốt trong quá trình học.

## Hiển thị một component
Trước đây, ta chỉ thấy các React element đại diện cho các thẻ DOM:

```js
const element = <div />;
```

Tuy nhiên, các element cũng đại diện cho các component do chính người dùng định nghĩa ra:

```js
const element = <Welcome name="Sara" />;
```

Khi React nhận thấy element được miêu tả trong component do người dùng tự định nghĩa, nó sẽ chuyển đổi các thuộc tính JSX trong component thành một object. Ta có thể thấy nó chính là "props".

Ví dụ, đoạn mã sau đây sẽ hiển thị ra "Hello, Sara" lên màn hình trình duyệt:

```js
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}

const element = <Welcome name="Sara" />;
ReactDOM.render(
  element,
  document.getElementById('root')
);
```
**[Chạy thử trên CodePen](https://reactjs.org/redirect-to-codepen/components-and-props/rendering-a-component)**

Cùng xem xét cái gì đã diễn ra trong ví dụ trên:
    1. Đầu tiên hàm `ReactDOM.render()` được gọi cùng với element `<Welcome name="Sara" />`.
    2. React tìm đến component `Welcome` sở hữu props với định dạng `{name: Sara}`.
    3. Và component `Welcome` lúc này trả về một element `<h1>Hello, Sara</h1>`.
    4. React DOM lúc này sẽ cập nhật lại DOM để khớp với kết quả được trả ra từ component `Welcome` là `<h1>Hello, Sara</h1>`.

*
    **Lưu ý: Tên của một component luôn luôn được viết hoa.**
    React coi các component là các thẻ DOM khi chúng có tên ở dạng viết thường. Ví dụ, `<div />` đại diện cho một thẻ `div` trong HTML, nhưng với `<Welcome />` thì sẽ đại diện cho một component và React sẽ tìm đến nó để yêu cầu việc trả ra các element mà nó sở hữu bên trong.<br><br>Bạn có thể đọc thêm về quy ước này [tại đây](http://google.com)

## Xây dựng một component
Các component có thể sở hữu các component khác bên trong. Điều đó cho phép chúng ta sử dụng các component theo hướng trừu tượng hoá ở bất kì cấp độ nào. Một button, form, dialog hay screen: trong ứng dụng React, tất cả đều được xây dựng và hành xử tuân theo các component.

Ví dụ, ta có thể khởi tạo một component `App` để tận dụng việc hiển thị component `Welcome` nhiều lần:

```js
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}

function App() {
  return (
    <div>
      <Welcome name="Sara" />
      <Welcome name="Cahal" />
      <Welcome name="Edite" />
    </div>
  );
}

ReactDOM.render(
  <App />,
  document.getElementById('root')
);
```
**[Chạy thử trên CodePen](https://reactjs.org/redirect-to-codepen/components-and-props/composing-components)**

Điển hình thay, các ứng dụng React phiên bản mới có một component là `App`, nó chính là một component gốc được tạo ra nhằm quản lý các diễn biến trong các component khác. Tuy nhiên, nếu bạn tích hợp React vào trong một ứng dụng đã có từ trước, bạn sẽ phải bắt đầu theo hướng xây dựng từ dưới lên với việc phải chia nhỏ các component như là `Button` và dần dần sửa đổi đến các phần gốc của ứng dụng.

## Chia tách component
Đừng ngần ngại khi chia nhỏ các component.

Hãy xem xét ví dụ về component `Comment` dưới đây:

```js
function Comment(props) {
  return (
    <div className="Comment">
      <div className="UserInfo">
        <img className="Avatar"
          src={props.author.avatarUrl}
          alt={props.author.name}
        />
        <div className="UserInfo-name">
          {props.author.name}
        </div>
      </div>
      <div className="Comment-text">
        {props.text}
      </div>
      <div className="Comment-date">
        {formatDate(props.date)}
      </div>
    </div>
  );
}
```
**[Chạy thử trên CodePen](https://reactjs.org/redirect-to-codepen/components-and-props/extracting-components)**

Component `Comment` nhận vào các đối số như `author` (kiểu đối tượng), `text` (kiểu chuỗi) và `date` (kiểu ngày tháng) thông qua props, và component này đáp ứng giao diện bình luận cho một trang mạng xã hội.

Component này vẫn còn có thể chia tách và tối ưu những phần tử được lồng bên trong nó. Hãy cùng tối ưu lại component này:

Đầu tiên, ta sẽ tách phần ảnh đại diện ra thành một component có tên `Avatar`:

```js
function Avatar(props) {
  return (
    <img className="Avatar"
      src={props.user.avatarUrl}
      alt={props.user.name}
    />

  );
}
```

Khi chia tách component `Avatar` sẽ không quan tâm đến việc nó sẽ được đẩy vào và hiển thi thông qua component `Comment` ra sao. Đó là lý do tại sao chúng ta sử dụng props và đổi tên biến thành một trường có ý nghĩa chung chung như: `user` hơn là `author`.

Khi đặt tên các thuộc tính được gói trong props ta nên đánh giá theo định hướng của component thay vì đặt vào ngữ cảnh chi tiết khi sử dụng.

Giờ thì có thể thấy component `Comment` đã tối ưu hơn một chút:

```js
function Comment(props) {
  return (
    <div className="Comment">
      <div className="UserInfo">
        <Avatar user={props.author} />
        <div className="UserInfo-name">
          {props.author.name}
        </div>
      </div>
      <div className="Comment-text">
        {props.text}
      </div>
      <div className="Comment-date">
        {formatDate(props.date)}
      </div>
    </div>
  );
}
```

Kế tiếp, ta sẽ chia tách nốt phần thông tin người dùng trở thành một component mới có tên `UserInfo` và đồng thời sẽ gói `Avatar` vào trong:

```js
function UserInfo(props) {
  return (
    <div className="UserInfo">
      <Avatar user={props.user} />
      <div className="UserInfo-name">
        {props.user.name}
      </div>
    </div>
  );
}
```

Cuối cùng ta thu được kết quả là một component `Comment` đã tối ưu hơn rất nhiều:

```js
function Comment(props) {
  return (
    <div className="Comment">
      <UserInfo user={props.author} />
      <div className="Comment-text">
        {props.text}
      </div>
      <div className="Comment-date">
        {formatDate(props.date)}
      </div>
    </div>
  );
}
```
**[Chạy thử trên CodePen](https://reactjs.org/redirect-to-codepen/components-and-props/extracting-components-continued)**

Việc phân tách các component luôn là công việc khó khăn nhất, nhưng đổi lại chúng ta sở hữu kết quả là có thể tái sử dụng các component trong một ứng dụng lớn. Một nguyên tắc nhỏ là nếu một phần của giao diện bạn xây dựng được sử dụng một vài lần như (`Button`, `Panel`, `Avatar`), hoặc phức tạp hơn nữa là các component do người dùng tạo ra như (`App`, `FeedStory`, `Comment`), và từ đó nó sẽ trở thành các component hữu dụng có thể tái sử dụng lại nhiều lần.

## Props chỉ đọc
Liệu bạn có thể định nghĩa một component tuân theo chuẩn [function hoặc class](http://google.com) mà nó sở hữu một props hữu hạn và không bao giờ thay đổi. Hãy xem ví dụ về hàm `sum` như sau:

```js
function sum(a, b) {
  return a + b;
}
```

Những hàm như trên được gọi là ["pure function"](https://en.wikipedia.org/wiki/Pure_function) bởi chúng không hề thay đổi các giá trị đầu vào, và luôn luôn trả về một kết quả dựa trên một tập đầu vào định sẵn.

Ngược lại, dạng hàm đối nghịch với pure function sẽ có sự thay đổi về tham số đầu vào giống như sau:

```js
function withdraw(account, amount) {
  account.total -= amount;
}
```

React rất linh hoạt nhưng nó cũng có một nguyên tắc bất di bất dịch là:

**Toàn bộ các component trong React phải hành xử như một pure function với sự thống nhất trong props của chúng.**

Nguyên tắc trên là điều dễ hiểu, bởi các giao diện luôn thay đổi và linh hoạt qua thời gian. Vậy trong phần [kết tiếp](https://github.com/vnknowledge2014/react_docs/blob/master/State%20and%20Lifecycle.md), ta sẽ cùng nhau tiếp cận một khái niệm mới là "state". State cho phép các component có thể uyển chuyển trong việc thay đổi các giá trị đầu ra theo thời gian để đáp ứng các yêu cầu xuất phát từ phía người dùng, trong các ứng dụng sử dụng mạng để luôn chuyển dữ liệu hay bất kì thứ gì khác mà không vi phạm tới nguyên tắc chúng ta thấy ở trên.
