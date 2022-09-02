## 0. Câu hỏi:
Đoạn code sau output là gì?
``` javascript
console.log(petName)
var petName = 'Bear'
```
<b>A.</b> ReferenceError: petName is not defined
<b>B.</b> Bear
<b>C.</b> undefined
<b>D.</b> ReferenceError: Cannot access 'petName' before initialization
<details><summary><b>Answer</b></summary>
C
</details>
## 1. Hosting là gì?
Hosting là tính năng lưu trữ của Javascript, tính năng này với `từng bối cảnh thực thi` sẽ đưa tất cả các `khai báo` lên đầu đoạn mã trước khi thực thi đoạn code.
=> Điều này giúp cho chươnng trình sẽ không bị lỗi việc sử dụng biến trước khi khai báo.

## 2. Hosting nó trông như thế nào?
Đoạn code ở <b>`0. Câu hỏi`</b> là 1 ví dụ cơ bản về Hosting:
```javascript
console.log(petName)
var petName = 'Bear' // Được khai báo phía sau console.log
```
Với hosting, tất cả `khai báo` được đẩy lên đầu trước khi thực thi đoạn code. Ở đây đoạn code trước khi thực thi sẽ `gần tương tự` như đoạn code sau:
```javascript
var petName; // petName đã được khai báo ở đầu đoạn code
console.log(petName)
petName = 'Bear' // <==
```
Tính năng hosting có thể khai báo kiểu mất dạy như vậy. Tuy nhiên điều này có gì hay mà javascript lại sinh ra??? Cứ đi tiếp đến các trường hợp hosting và đưa ra kết luận.

## 3. Hosting trong các trường hợp
Ở đây cụ thể sẽ có 2 trường hợp lớn: <b>`variable`</b> và <b>`function`</b>
#### a. Với Variable
Với biến thì có 3 từ khoá: `var`, `let` và `const`
- Từ khoá <b>`var`</b>:
Khi sử dụng mà chưa khởi tạo giá trị sẽ trả về `undefined`
```javascript
console.log(petName) // undefined
var petName = 'Bear'
```
- Từ khoá <b>`let`</b> và <b>`const`</b>:
Khi sử dụng mà chưa khởi tạo giá trị sẽ trả về lỗi `ReferenceError: Cannot access 'petName' before initialization`
```javascript
console.log(petName) // ReferenceError: Cannot access 'petName' before initialization
let petName = 'Bear'
```

-> Lỗi này muốn báo là biến `petName` đã được khai báo trong `heap` nhưng chưa được khởi tạo giá trị

Vậy với `0. Câu hỏi` thì đáp án sẽ là <b>`C.`</b> `undefined`

#### b. Với function
Giống như với `variable`, thì `function` cũng sẽ được đẩy khai báo lên đầu đoạn code.

VD: 
```javascript
let x = 20,
    y = 10;

let result = add(x, y); // function add chưa được khai báo ở đây
console.log(result);

// function add được khai báo sau khi gọi
function add(a, b) {
    return a + b;
}
```

Ở ví dụ này, hosting sẽ đẩy khai báo `function add` lên đầu đoạn code. Lúc này đoạn code sẽ gần tương tự như sau:
```javascript
function add(a, b) { // function được đẩy lên đầu đoạn code
    return a + b;
}

let x = 20,
    y = 10;

let result = add(x, y);
console.log(result);

// function add(a, b) {
//     return a + b;
// }
```
Và kết quả sẽ là: `30`

##4. Các ví dụ khác
####a. Với biến có giá trị là 1 function
VD: 
```javascript
let x = 20,
    y = 10;

let result = add(x, y);
console.log(result);

var add = function(a, b) { // Biến add = function()
    return a + b;
}
```
Đúng như khải niệm thì hosting sẽ khai báo biến add lên đầu đoạn code. Đoạn code sẽ gần tương tự như:
```javascript
var add;

let x = 20,
    y = 10;

let result = add(x, y); //TypeError: add is not a function
console.log(result);

add = function(a, b) {
    return a + b;
}
```
Kết quả sẽ có lỗi:  `TypeError: add is not a function`

Kết quả sẽ tương tự nếu biến có giá trị là `arrow function`
```javascript
let x = 20,
    y = 10;

let result = add(x, y);
console.log(result);

var add = (a, b) => { // Biến add = arrow function
    return a + b;
}
```

####b. Hosting giữa các block
VD: ở đây sử dụng từ khoá let để phạm vi sử dụng là trong từng block 
```javascript
let count = 10;
{
    console.log(count);
    let count = 20;
}
```
Ở ví dụ này, để rạch ròi việc hosting sẽ hoạt động như nào trong `từng bối cảnh thực thi`.
Tất cả các ví dụ đã sử dụng ở các phần trước đều nằm ở `bối cảnh thực thi global`

`Mỗi block sẽ tạo ra các bối cảnh thực thi khác theo đó biến trong block con sẽ nhìn được biến global và không nhìn được sang các block cùng cấp khác => Nội dung này sẽ được đề cập cụ thể hơn ở bài Phạm vi sử dụng`

Theo đó, có 2 bối cảnh thực thi được tạo ra. 1 là global, và 2 là trong block. Hosting sẽ hoạt động trong cả 2 bối cảnh thực thi.
Đoạn code sẽ gần tương tự như sau:
```javascript
// Hosting trong bối cảnh thực thi của global
//(hiện tại không có gì thay đổi vì chỉ có khai báo 1 biến count ở let count = 10)
let count = 10;
{
    let count; // Hosting trong bối cảnh thực thi của block
    console.log(count); //ReferenceError: Cannot access 'count' before initialization
    count = 20;
}
```
Kết quả sẽ là `ReferenceError: Cannot access 'count' before initialization`

##5. Kết luận
Hosing về cơ bản sẽ là 1 tính năng không được hoàn hảo, vì đã phá vỡ nguyên tắc khi code là khai báo trước khi sử dụng. Tựu chung sẽ có 2 kết luận cá nhân:
1. Với variable: Gần như vô nghĩa vì nguyên tắc code cần khai báo biến trước khi sử dụng và hosting đã phá vỡ nguyên tắc đó. Cái được duy nhất là sẽ không bị lỗi chưa khai báo biến nếu có khởi tạo giá trị bên dưới (Hay xảy ra trong code PHP)
2. Với function thì sẽ khá hữu ích vì có thể đưa các khai báo function xuống cuối đoạn code để tránh việc lướt mãi mới đến đoạn code chính

<b></b>
```javascript
```
