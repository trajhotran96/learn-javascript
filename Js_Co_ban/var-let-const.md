## 0. Câu hỏi
Output sẽ là gì?

```javascript
const petName = 'Monkey'
{
    const petName = 'Bear'
}
console.log(petName)
```
- A: `Monkey`
- B: `Bear`
- C: `SyntaxError: Identifier 'petName' has already been declared`
- D: `ReferenceError: Cannot access 'petName' before initialization`
<details><summary><b>Answer</b></summary>
A: Monkey
</details>

## 1. Từ khóa `var`, `let` và `const` là gì?
Các từ khóa `var`, `let` và `const` đều là các từ khóa phục vụ cho việc khai báo biến trong javascript. Trong đó `let` và `const` được bổ sung ở ES6.
Vậy tại sao có `var` rồi lại cần thêm `let` và `const`?

## 2. Các từ khóa `var`, `let` và `const` trông như thế nào?

```javascript
var petName = 'Bear'
let petAge = '2' 
const petSex = 'Female'
```

## 3. Phân biệt
### a. Từ khóa `var`
Nếu như `var` được sinh ra để có thể khai báo đa dạng các kiểu dữ liệu như number, string, bool... Vậy tại sao lại bổ sung thêm `let` và `const` để làm gì?

Phạm vi của biến `var`:
- Trường hợp khai báo trong function thì phạm vi sử dụng là function.
- Các trường hợp khác, khi được khai báo thì biến `var` sẽ có phạm vi là `global`.

Cùng đi vào ví dụ để hiểu hơn về `var` và vấn đề của `var`.

<b> Trường hợp khai báo trong function để chứng minh phạm vi sử dụng biến `var` là `function scope`</b>
VD:

```javascript
function getName() {
    var petName = 'Bear';
    return petName;
}
console.log(petName) // ReferenceError: petName is not defined
```
Biến `var petName` được khai báo trong `function getName`. Phạm vi sử dụng `petName` sẽ là trong function => Dẫn đến `console.log(petName)` bên ngoài function sẽ lỗi chưa khai báo biến `petName`.

<b>Trường hợp khai báo khác để chứng minh phạm vi sử dụng biến `var` là `global scope`</b>

VD: 

```javascript
{
    var petName = 'Bear'
}
console.log(petName) // Bear
```
Kết quả sẽ là `Bear` mặc dù `petName` được khai báo bên trong một block.

<b>Sự vô tình trong code với `var`</b>

Giả sử trong trường hợp code rất dài, và `vô tình` ta khởi tạo tên biến trong một block nào đó `trùng tên` với biến bên ngoài block. Điều gì sẽ xảy ra?
VD: 

```javascript
var petName = 'Monkey'
//code rất dài
if (true) {
    //code rất dài
    var petName = 'Bear' // sự vô tình
    //code rất dài
}
console.log(petName) // Bear
```
Kết quả sẽ là `Bear` chứ không phải `Monkey`

Rõ ràng chẳng ai lại khai báo mới biến `petName` khi chỉ muốn thay đổi giá trị biến `petName`cả.
Lý do ở đây là việc phạm vi sử dụng của `var` là `global`. Dẫn đến việc `var petName = 'Bear'` nằm trong block nhưng sẽ thay thế giá trị `petName = 'Monkey'` được khởi tạo bên ngoài. Đây là lỗi không mong muốn và muốn debug thì rất khó khăn.

Để giải quyết vấn đề này thì ES6 bổ sung thêm `let` và `const`.
### b. Từ khóa `let`
Một điều mà có thể khiến `let` hoặc `const` giải quyết được vấn đề trên của `var` đó là `phạm vi sử dụng là block`.

<b>Chứng minh phạm vi biến là block scope</b>

VD:

```javascript
{
    let petName = 'Bear'
}
console.log(petName) // ReferenceError: petName is not defined
```
Biến `petName` được khai báo bên trong block, nên sẽ không sử dụng được ở bên ngoài block. `console.log(petName)` gọi bên ngoài block sẽ không biết `petName` ở đâu và có lỗi `petName is not defined`.

<b>Giải quyết vấn đề của `var`</b>

Thử thay đoạn code vấn đề thành từ khóa `let` và coi kết quả.
```javascript
let petName = 'Monkey'
if (true) {
    let petName = 'Bear' // sự vô tình
}
console.log(petName) // Monkey
```
Kết quả là : `Monkey`
Vậy là khai báo trong block đã không còn làm ảnh hưởng đến biến trùng tên bên ngoài block. Ổn áp rồi.
Điều này sẽ tương tự với `const`. Vậy nên đáp án của <b>`0. Câu hỏi`</b> sẽ là` A: Monkey`

<b>Chú ý: </b> Cả `let` và `const` `có thể cập nhật giá trị` nhưng `không thể tái khởi tạo tên biến đó`.

VD về `có thể cập nhật giá trị`:
```javascript
let petName = 'Monkey'
petName = 'Monkey2'
console.log(petName) // Monkey2
```
Kết quả: `Monkey2`
Đã được cập nhật giá trị

VD về `không thể tái khởi tạo tên biến đó`:
```javascript
let petName = 'Monkey'
let petName = 'Bear' //SyntaxError: Identifier 'petName' has already been declared
console.log(petName)
```
Kết quả có lỗi: `SyntaxError: Identifier 'petName' has already been declared`
Lỗi này báo là biến 'petName' đã được khai báo trước đó rồi.

### c. Từ khóa `const`
Tương tự `let` thì `const` cũng có phạm vi sử dụng là `block scope`.
Có 2 trường hợp khởi tạo giá trị của `const`:
- Trường hợp `const` có kiểu dữ liệu là nguyên thủy (primitive) như: string, number, boolean, null, undefine. Thì sẽ `không thể tái khởi tạo giá trị hay cập nhật giá trị` được.
- Trường hợp `const` có kiểu dữ liệu là tham chiếu (reference) như : object, array, function. Thì sẽ `cập nhật giá trị được`, còn `tái khởi tạo giá trị thì không`.

#### Cùng đi vào ví dụ để làm rõ 2 trường hợp trên.

<b>- `const` có kiểu dữ liệu là nguyên thủy (primitive)</b>

Làm rõ `không thể tái khởi tạo giá trị hay cập nhật giá trị`

VD: 
```javascript
const petName = 'Monkey'
petName = 'Bear' //TypeError: Assignment to constant variable
console.log(petName)
```
Kết quả có lỗi: `TypeError: Assignment to constant variable`.

Ở đây muốn tái khởi tạo giá trị `petName = 'Bear'` nhưng đã phát sinh lỗi.

<b>- `const` có kiểu dữ liệu là tham chiếu (reference)</b>

Cùng làm rõ `cập nhật giá trị được`, còn `tái khởi tạo giá trị thì không`.

VD `tái khởi tạo giá trị thì không được`:
```javascript
const pet = {
    name: 'Monkey',
    age: 2
}
pet = {
    name: 'Bear',
    age: 3
} // TypeError: Assignment to constant variable.
console.log(pet)
```
Kết quả có lỗi: `TypeError: Assignment to constant variable.`
Ở đây lỗi là do muốn khởi tạo lại biến `pet` thành 1 object khác.

VD `cập nhật giá trị được`: 
```javascript
const pet = {
    name: 'Monkey',
    age: 2
}
pet.name = 'Bear'
pet.age = 3
console.log(pet) // {name: 'Bear', age: 3}
```
Kết quả:
```javascript
{name: 'Bear', age: 3}
```

<b>Nguyên nhân của sự khác nhau của 2 trường hợp với `const`</b>

`const` sẽ không thay đổi được giá trị của biến đó.
- Đối với giá trị có kiểu dữ liệu là nguyên thủy (primitive) thì giá trị sẽ không thay đổi được là điều hiển nhiên. Vì đây là tính chất của `const`.
- Còn đối với kiểu dữ liệu là tham chiếu (reference) đó là hệ quả của tính chất của từ khóa `const`. Như ta biết về tham chiếu (reference) thì `giá trị của biến sẽ là địa chỉ` của object/array/function. Thế nên ta `cập nhật giá trị` thì không ảnh hưởng gì đến địa chỉ nên giá trị `const` vẫn là không đổi. Còn khi `tái khởi tạo giá trị` thì javascript sẽ tạo object/array/function mới với địa chỉ mới. Sau đó gán giá trị địa chỉ mới vào biến `const`, lúc này thì sẽ gây lỗi.
