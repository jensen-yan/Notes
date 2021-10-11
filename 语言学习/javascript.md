### 简介

网页3门语言

1. html定义网页内容
2. css描述网页布局
3. javascript 控制网页行为



是脚本语言，可以插入html，容易学习

```javascript
document.write("<h1>这是一个标题</h1>");

<button type="button" onclick="alert('欢迎!')">点我!</button>
// alert 多用于条是， onclick是事件

x=document.getElementById("demo");  //查找元素
x.innerHTML="Hello JavaScript";    //改变内容

//element.src.match("bulbon") 的作用意思是：检索 <img id="myimage" onclick="changeImage()" src="/images/pic_bulboff.gif" width="100" height="180"> 里面 src 属性的值有没有包含 bulbon 这个字符串
```



### 用法

1. html中的脚本必须放在 \<script> ...  \<\\script>中间. 可以放在html的body， head部分中

2. ```html
   <p id="demo">一个段落。</p>
   <button type="button" onclick="fn">点这里</button>
   <script>
   function fn()
   {
       x = documnet.getElementById("demo");
       x.innerHTML="hello world";
   }
   </script>
   
   // 存在外部文件my.js
   <script src="my.js"></script>
   ```

3. 可以在chrome中console命令行输入 console.log("hello"). 也可以在sources-》 snippets 中新建脚本文件来执行



### 输出

1. window.alert()  弹出警告框
2. document.write()  把内容写入html文档中   document.write(Date()); 注意会覆盖整个页面
3. innerHTML 写道html元素中
4. consol.log()  写入浏览器的控制台中   



### 语法

1. 类似python，变量不用定义类型

2. 有Number， String, Literal, Array(  [1,2,3]  ), Object(   {one:1, two:2} ), Function (  )

3. 用 var  来定义变量

4. 使用Unicode，不是Ascii

5. ```javascript
   document.write("你好 \
   世界!");
   // 可以用 \  来换行， 只是对字符串换行！
   ```

6. 数据类型

   1. **值类型(基本类型)**：字符串（String）、数字(Number)、布尔(Boolean)、对空（Null）、未定义（Undefined）、Symbol（表示独一无二的值）。
   2. **引用数据类型**：对象(Object)、数组(Array)、函数(Function)。

7. 有动态类型，一个变量可以赋值成不同类型

8.  var cars = new Array(1, 23, 4);  var cars = [1,2,3];

9. 对象 var car = {name:"Fiat", model:500, color:"white"};  车每个属性有不同的值 car.name, car["name"]  都可以访问

10. ```javascript
    var person = {
        firstName: "John",
        lastName : "Doe",
        id : 5566,
        fullName : function() 
    	{
           return this.firstName + " " + this.lastName;
        }
        // 用person.fullName() 访问
    };
    ```

11. 



#### html

1. p 和div区别
   1. p会自动空行一格， div不会
   2. div主要用于构造框架