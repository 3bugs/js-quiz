# JavaScript Quiz, solved & explained by Promlert Lovichit

### 1. What will the code below output to the console?

```javascript
(function () {
  var a = b = 3;
})();
console.log("a defined? " + (typeof a !== 'undefined'));
console.log("b defined? " + (typeof b !== 'undefined'));
```

#### ตอบ:

ผลลัพธ์ที่ console คือ

```
a defined? false
b defined? true
```

#### เหตุผล:

ในประโยคคำสั่ง `var a = b = 3;` นิพจน์ `b = 3` จะถูกทำ (evaluate) ก่อน เพราะ assignment operator จะถูกทำจากขวาไปซ้าย

แต่เนื่องจากไม่มีการใช้คีย์เวิร์ด `var`, `let` หรือ `const` ในการประกาศ `b` จึงทำให้ `b` กลายเป็นตัวแปรใน global scope (
และมีค่าเท่ากับ 3)

นอกจากนิพจน์ `b = 3` จะเป็นการกำหนดค่า 3 ให้กับ `b` แล้ว ยังเป็นนิพจน์ที่ให้ค่า `b` ออกมา (ซึ่งก็คือ 3)
และค่านี้ถูกนำไปกำหนดให้ `a` ต่อ ดังนั้น `a` จึงมีค่าเท่ากับ 3 ด้วย

แต่เนื่องจาก `a` ถูกประกาศด้วย `var` จึงทำให้มันเป็น local variable ของฟังก์ชั่น (ในที่นี้คือ *นิพจน์ฟังก์ชั่น - function expression* ที่ถูก
define แล้วเรียกใช้แบบ inline ทันที) ดังนั้นภายนอกฟังก์ชั่นจึงไม่เห็นตัวแปร `a`

เมื่อตรวจสอบ type ของ `a` ที่ global scope จึงได้ว่า `a` มี type เป็น `undefined` ส่วน `b` ซึ่งมีค่า 3 จะมี type
เป็น `number`

ดังนั้น นิพจน์ `typeof a !== 'undefined'` จึงให้ค่า `false` ส่วนนิพจน์ `typeof b !== 'undefined'` ให้ค่า `true`

### 2. What will the following code output to the console?

```javascript
(function () {
  var a = b = 5;
})();
console.log(b);
```

#### ตอบ:

ผลลัพธ์ที่ console คือ

```
5
```

#### เหตุผล:

ตามเหตุผลในข้อ 1 จะได้ว่า `b` กลายเป็นตัวแปรใน global scope และมีค่าเท่ากับ 5

### 3. What will the following code output to the console?

```javascript
function test() {
  console.log(a);
  console.log(foo());

  var a = 1;

  function foo() {
    return 2;
  }
}

test();
```

#### ตอบ:

ผลลัพธ์ที่ console คือ

```
undefined
2
```

#### เหตุผล:

JavaScript มีคุณสมบัติ **Hoisting** หมายถึง การประกาศตัวแปรและฟังก์ชั่นต่างๆจะถูกยกขึ้นมาไว้ข้างบนสุดของ scope นั้นๆ

ในที่นี้ การอ่านค่าตัวแปร `a` และเรียกใช้ฟังก์ชั่น `foo()` ก่อนการประกาศ 2 สิ่งนี้ จึงสามารถทำได้

แต่ตอนที่ log ค่าของ `a` นั้น `a` ยังไม่ได้ถูกกำหนดค่าเริ่มต้น (เพราะ *การกำหนดค่า* ให้กับ `a` มาทีหลังการ log
ค่าของ `a` ถึงแม้ *การประกาศ* `a` จะถูก hoist ขึ้นมาก็ตาม) จึงทำให้ `a` มีค่าเป็น `undefined` และแสดงค่านี้ออกมาที่
console

ส่วน log อันที่ 2 จะ call ฟังก์ชั่น `foo()` จึงได้ค่าที่ฟังก์ชั่น `foo()` return กลับมา นั่นก็คือ 2 แสดงออกมาที่ console

### 4. What will the following code output to the console?

```javascript
function Greet(mode) {
  this.mode = mode;
  this.callgreet = function () {
    console.log('Good ' + this.mode);
  }
}

obj1 = new Greet("Morning");
obj1.callgreet();
obj2 = new Greet("Evening");
obj2.callgreet();
```

#### ตอบ:

ผลลัพธ์ที่ console คือ

```
Good Morning
Good Evening
```

#### เหตุผล:

ฟังก์ชั่น `Greet()` ในที่นี้คือ constructor function ซึ่ง `new Greet()` จะเป็นการสร้าง `Greet` instance/object
ขึ้นมาใหม่ โดยค่าที่เราส่งให้กับ `new Greet()`
จะถูกเก็บลงในพร็อพเพอร์ตี้ `mode` ในออบเจ็ค (instance) นั้นๆ (`this.mode`)

ดังนั้น เมื่อเรียกใช้ฟังก์ชั่น `callgreet()` บนแต่ละออบเจ็ค จึงเป็นการพิมพ์คำว่า **Good**
แล้วตามด้วยค่าของพร็อพเพอร์ตี้ `mode` ของออบเจ็คนั้นๆ ออกมาที่ console

### 5. What will the following code output to the console?

```javascript
var games = function () {
  this.name = '';
}
games.prototype.getName = function () {
  this.name = 'Age of Empire';
  return this;
}
games.prototype.setName = function (n) {
  this.name = n;
}
var g = new games();
g.getName().setName('Angry Birds');
console.log(g.name);
```

#### ตอบ:

ผลลัพธ์ที่ console คือ

```
Angry Birds
```

#### เหตุผล:

ตัวแปร `games` ในที่นี้คือ constructor function (ถูก define ในรูปแบบ *นิพจน์ฟังก์ชั่น*) ซึ่งภายใน constructor
มีการกำหนดพร็อพเพอร์ตี้ `name` ของ instance ให้มีค่าเริ่มต้นเป็นสตริงว่าง

จากนั้นมีการ define ฟังก์ชั่น (เมธอด) `getName()` และ `setName()` เพิ่มให้กับ `games`

เมธอด `getName()` จะกำหนดพร็อพเพอร์ตี้ `name` ของ instance เป็นค่า `'Age of Empire'` แล้ว return instance นั้นๆ
กลับออกไป

เมธอด `setName()` จะนำค่าของตัวแปร `n` ที่เราส่งให้เมธอดนี้ ไปกำหนดให้พร็อพเพอร์ตี้ `name` ของ instance

`var g = new games();` คือการสร้าง `games` object เก็บไว้ที่ตัวแปร `g`

`g.getName().setName('Angry Birds');` คือการเรียกใช้ `getName()` เพื่อกำหนดพร็อพเพอร์ตี้ `name` ในออบเจ็ค `g`
เป็นค่า `'Age of Empire'` แล้วเรียกใช้ `setName()` เพื่อเปลี่ยนพร็อพเพอร์ตี้ `name` เป็น `'Angry Birds'`
ตามค่าที่ส่งให้กับ `setName()`

ดังนั้นเมื่อ log ค่า `g.name` จึงแสดงคำว่า **Angry Birds** ออกมาที่ console

### 6. What will the following code output to the console?

```javascript
function ObjSort(arr) {
  var len = arr.length;
  for (var i = len - 1; i >= 0; i--) {
    for (var j = 1; j <= i; j++) {
      if (arr[j - 1] > arr[j]) {
        var temp = arr[j - 1];
        arr[j - 1] = arr[j];
        arr[j] = temp;
      }
    }
  }
  return arr;
}

var arrayResult = ObjSort([7, 5, 2, 4, 3, 9]);
var result = "";
for (var i = 0; i < arrayResult.length; i++) {
  result = result + arrayResult[i] + " ";
}
console.log(result);
```

#### ตอบ:

ผลลัพธ์ที่ console คือ

```
2 3 4 5 7 9
```

#### เหตุผล:

ฟังก์ชั่น `ObjSort()` จะรับ array เข้ามาทำการเรียงลำดับข้อมูลจากน้อยไปหามาก โดยใช้ bubble sort algorithm

ในแต่ละรอบของ loop ชั้นนอก ค่ามากสุดจะถูก bubble ไปทางขวาของ array ยกตัวอย่างเช่น 

- เมื่อจบ loop ชั้นนอกในรอบแรก ค่ามากสุด คือ 9 จะถูกย้ายไปยังตำแหน่งสุดท้าย (ขวาสุด) ของ array 
- เมื่อจบ loop ชั้นนอกในรอบที่ 2 ค่ามากสุดรองลงมา คือ 7 จะถูกย้ายไปยังตำแหน่งรองสุดท้ายของ array
- ฯลฯ

`ObjSort()` จะ return array ที่เรียงลำดับข้อมูลแล้วออกไป (จริงๆก็คือ array reference เดิม เพราะ `ObjSort()` ทำการ mutate ข้อมูลใน array ที่รับเข้ามาโดยตรง) 

ในที่นี้เก็บ array ที่เรียงลำดับแล้วไว้ในตัวแปร `arrayResult`

โค้ดส่วนที่เหลือ เป็นการนำข้อมูล (ค่าจำนวนเต็ม) ใน `arrayResult` มาแปลงเป็นสตริง แล้วเชื่อมต่อกันเป็นสตริงชุดเดียว โดยแยกแต่ละค่าด้วยช่องว่าง แล้ว log ออกมาที่ console

ที่จริงโค้ด 5 บรรทัดสุดท้ายที่ทำหน้าที่แสดงค่าของ `arrayResult` สามารถยุบให้เหลือบรรทัดเดียว โดยใช้ array's reduce method ดังนี้

```javascript
var result = arrayResult.reduce((acc, item) => acc + item + " ", "");
```

หรือใช้ backquote (`) ในการทำ string interpolation แทนการต่อสตริงด้วย + ก็จะได้โค้ดที่สวยและเข้าใจง่ายขึ้นอีก

```javascript
const space = ' ';
var result = arrayResult.reduce((acc, item) => `${acc}${item}${space}`, "");
```
