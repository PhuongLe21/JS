## I. Javascript
1. use strict: Giúp code nghiêm ngặt hơn, warning error ví dụ như biến sử dụng trước khi khai báo
2. const, let, var:
* const, let: no reassign, block scope.
* var: cho phép gán lại biến, và global scope hoăc nếu khai báo trong function thì là function scope, ngoài ra nó còn có cơ chế hoisting
3. Hoisting: Là cơ chế mà cho phép biến hoặc hàm được khai báo lên đâù scope, áp dung cho 2 case
* Biến dùng từ khóa var
* Hàm khai báo theo kiểu function declaration  
Có 2 kiểu khai báo hàm là:
```
function doSomething() { // Function declaration

}

const doSomething = function() { // Function expression

}
```
4. Closure: Ví dụ là hàm A trả về hàm B => Là cơ chế cho phép inner function có thể access biến của outer function ngay cả khi outer function đã chạy xong:
```
function A () {
  const name = 'Huong'
  return function () {
    console.log(name)
  }
}

(1) const person = A()
(2) person() // Huong
Măc dù sau câu lênh 1 hàm A đã chạy xong, đáng lẽ biến name được giải phóng rồi, nhưng câu lệnh 2 vẫn access đc biến A, đó là nhờ cơ chế closure 
```
5. Handle asynchronous
There are 3 popular ways: Callback, Promise, async/await
* Callback: là fn A, đc truyền như là đối số của fn B và trong B gọi A
* Promise:
  - Sinh ra để xử lý callback hell bằng cách dùng **.then** nhiều lần, trong *then* phải return 1 promise thì nó mời chờ còn nếu không nó sẽ không chờ
  - Khi có nhiều *then* thì chỉ cần 1 *then* trong đó gây lỗi, thì nó sẽ nhảy vào catch và các *then* còn lại sẽ không được chạy
  - finally luôn chạy sau khi cùng kể cả khi bị reject hay fulfilled
  - 3 trạng thái của promise: fulfilled, pending, reject
```
Callback
  fn B(){
    console.log('I am a callback, I will be passed as a argument to A')
  }
  fn A (B) {
    console.log('Hello')
    B()
}

Promise
  const myPromise = new Promise(function(resolve, reject){
    // handle to resolve or reject sth
  })
```
Promise.all để xử lý nhiều promise mà kết quả của chúng k phụ thuộc nhau, nhưng có thể khi các promise đó chạy xong thì cần làm gì đó

6. Localstorage, SessionStorage, Cookie
- SessionStorage lưu data ở browser nhưng bị mất đi nếu đóng tab hoặc trình duyệt còn localStorage thì không
- Cookie??: sẽ được gửi kèm lên với các request

7. Arrow function:
- Viết ngắn gọn hơn normal fn
- **this** trong arrow fn sẽ k phải là context của fn đó mà là của fn gần nhất

8. bind, call, apply
- bind return new fn, cái gì truyền vào bind thì *this* trong fn đó sẽ nhận giá trị đc truyền vào
- call(first, value1, value2 ...): first sẽ là *this*, những cái sau là các đối số của hàm đó
- apply(first, array or array-like object): như call nhưng tham số thứ 2 là array


## React
1. Virtual Dom: React tạo virtual Dom, khi state change thì nó sẽ cập nhật virtual DOM, sau đó nó so sánh với previous virtual Dom (quá trình này goi là diffing), sau đó nó mới cập nhật real DOM (có thể thông qua thư viện ReactDOM)
2. JSX
- Cho phép viết code giống HTML trong react (ví dụ như cái phần code render trong component đó không phải là HTML hay JS, mà là JSX)
- Cần 1 thư viện như babel để biên dich code JSX
- Nếu không có JSX thì sẽ viết kiểu như sau, rất loằng ngoằng mà tốn time
```
React.createElement(
  'div',
  {className: 'sidebar'}
)
```
3. Class component và functional component?
- Functional component chỉ render giao diện, và có thể có props truyền vào, không có state và life cycle như class, chính vì vậy các phiên bản mới của React thì có bổ sung thêm các hooks để giải quyết vấn đề này

4. Làm sao để giới hạn type của props truyền vào? Ví dụ như props đó yêu cầu required
- Sử dụng PropTypes trong thư viện *prop-types*, hoặc typescript

5. Uncontrolled component và controlled component
- Controlled component là component mà có các form như input, textarea được quản lý bời state, nghĩa là sẽ có state là value của form đó, mỗi khi user gõ vào thì sẽ có hàm set lại state và giá tri form sẽ được cập nhaatj
- Uncontrolled thì không, cho nên muốn access giá trị mà user nhập vào có thể dùng reference
```
function ControlledComponent {
  const [name, setName] = useState(')
  const updateInputValue = () {
    // set name again
  }

  return (
    <input value={name} onChange={updateInputValue}/>
  )
}
```

6. Life cycle in React
- Có nhiều lifecycle nhưng thực tế thì chỉ dùng chủ yếu các life cycle sau:
  - Mounting: constructor, componentDidMount
  - Updating: componentDidUpdate
  - Unmounting: componentWillUnmount

- Bên hooks thì mình useEffect có thể  thực hiện được 3 cái là componentDidMount (truyền mảng rỗng), componentDidUpdate(không truyền depedency), componentWillUnmount(return 1 fn)

7. React hooks
- 1 số hook chính
  + useState
  + useEffect
  + useCallback
  + useMemo
  + useRef
```
const [name, setName] = useState('Hong')
useEffect(fn, [deps])
useEffect(() => {
  // doSth
  /*
    deps is empty => didMount
    deps not defined => didUpdate
    truyen cac deps thi chi can 1 trong cac deps change thi fn trong useEffect se run again
  */  
  return fn // componentWillUnmount,
}, [])

const cb = useCallback(fn, deps) // deps change thi fn run again
const a = 7
const memo = useMemo(() => return a, deps)
=> useCallback(fn, deps) tuong tu useMemo(() => fn, deps)
```
useCallback va useMemo thường kết hợp với React.memo để tăng performance. React.memo là 1 hoc sẽ bọc 1 component khác và chỉ khi props thay đổi component đc bọc bởi React.memo mới chạy laij
```
// file OtherComponent.js
function OtherComponent ({ onClick }) {
  console.log('Vi co React.memo nen onClick phai change thi fn nay moi chay lai')
}
export default React.memo(OtherComponent)
---------------------------------------------------------------------------------
file A.js
function A () {
  const [name, setName] = useState('')
  const handleClick = () => useCallback(fn, []) // do hàm hoặc các biến reference sẽ tao mới mỗi khi component này chạy lại nên dùng useCallback
  return (
    <>
      <input type="text" value={name} onChange={handleChangename} />
      <OtherComponent onClick={handleClick}>
    </>
  )
}
```
