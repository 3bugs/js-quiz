# JavaScript Quiz

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

ประโยคคำสั่ง `var a = b = 3;` จะทำ `b = 3` ก่อน (assignment operator จะทำจากขวาไปซ้าย)

แต่เนื่องจากไม่มีการใช้คีย์เวิร์ด `var`, `let` หรือ `const` ในการประกาศ `b` จึงทำให้ `b` กลายเป็นตัวแปรใน global scope (
และมีค่าเท่ากับ 3)

นอกจาก `b = 3` จะกำหนดค่า 3 ให้กับ `b` แล้ว มันยังเป็นนิพจน์ที่ให้ค่า `b` ออกมา (ซึ่งก็คือ 3)
และค่านี้ถูกนำไปกำหนดให้ `a` ต่อ ดังนั้น `a` จึงมีค่าเท่ากับ 3 ด้วย

แต่เนื่องจาก `a` ถูกประกาศด้วย `var` จึงทำให้มันเป็น local variable ของฟังก์ชั่น (ในที่นี้คือนิพจน์ฟังก์ชั่นที่ถูก
define แล้วเรียกใช้แบบ inline ทันที) ดังนั้นภายนอกฟังก์ชั่นจึงไม่เห็นตัวแปร `a`

เมื่อตรวจสอบ type ของ `a` ที่ global scope จึงได้ว่า `a` มี type เป็น `undefined` ส่วน `b` ซึ่งมีค่า 3 จะมี type
เป็น `number`

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

`new Greet()` จะสร้าง instance ของ `Greet` (`Greet` object) ขึ้นมาใหม่ โดยค่าที่เราส่งให้กับ `new Greet()` จะถูกเก็บลงในพร็อพเพอร์ตี้ `mode` ในออบเจ็ค (instance) นั้นๆ (`this.mode`) 

ดังนั้น เมื่อเรียกใช้ฟังก์ชั่น `callgreet()` บนแต่ละออบเจ็ค จึงเป็นการพิมพ์คำว่า Good แล้วตามด้วยค่าของพร็อพเพอร์ตี้ `mode` ของออบเจ็คนั้นๆออกมาที่ console

