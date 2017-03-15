JavaScript

基础数据类型（值类型、引用类型）、传递参数、栈（堆栈）、堆内存、内存回收机制

- 值类型：undefined Null Boolean Number String 按值访问，可以直接操作保存在变量对象中的值。

- 引用类型:不能直接操作变量，首先从变量对象中获取到该对象的引用，根据该引用去堆内存中取出需要的数据。

- 传递参数：所以函数的参数都是按值传递。

  ​	在向传递参数传递基本类型的值时，被传递的值会被复制给一个局部变量。

  ​	在向参数传递引用类型的值时，会把**这个值在内存中的地址复制给一个局部变量**，因此这个局部变量的变化会反应在函数的外部。（**因为是把地址复制给局部变量arguments对象的一个属性，所以传递的也是一个值类型**）

- 堆栈（栈）：直接访问存取在变量对象中的值。

- 堆内存：通过变量对象中引用来间接访问。

- 内存回收机制：很多种回收机制。但是最常用的是通过**标记清除**的算法来查找哪些对象不在继续被使用，从而来进行回收。**a =null**其实仅仅只是做了一个释放引用的操作，让 a 原本对应的值失去引用，脱离执行环境，这个值会在下一次垃圾收集器执行操作时被找到并释放.

![](http://upload-images.jianshu.io/upload_images/599584-8e93616d7afcf811.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

执行上下文（执行环境）、变量对象、活动对象、变量提升、作用域、作用域链、this

- ​       **执行上下文（执行环境）**：当执行流进入到一个可执行环境时（函数），就会进入一个执行上下文。执行上下文可以理解为当前代码的执行环境。在一个JavaScript程序中，会产生很多个执行上下文（执行环境），JavaScript引擎会以栈（堆栈）的方式来处理他们，我们称其为函数调用栈（call stack）。栈顶永远是当前正在执行的执行上下文，顶底永远是全局上下文。

  ​       全局环境：JavaScript代码运行起来会首先进入该环境

  ​	函数环境：当函数被调用执行时，会进入当前函数中执行代码

  ​	eval

  一个函数被调用时（激活），一个新的执行上下文（执行环境）就会被创建生命周期可以分为两个阶段。

  ***创建阶段：***

  在这个阶段，执行上下文会创建变量对象，建立作用域，确定this的指向。

  ***代码执行阶段：***

  创建完成后，开始执行该执行上下文中的代码，包括变量赋值、函数引用等执行其他代码。

  ![](http://upload-images.jianshu.io/upload_images/599584-391af3aad043c028.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- ​        **变量对象**：每个执行上下文都与之相关联的变量对象。每个执行上下文（执行环境）中的变量和函数都会保存在这个对象中，在执行上下文的创建阶段就会创建变量对象。

  变量对象的创建，依次经过以下几个过程：参数对象arguments、函数声明，变量声明、

  1.建立arguments对象。检查该执行环境（函数）的参数，建立arguments对象的属性以及对应的值。

  2.检查当前执行环境的**函数声明**，也就是function关键字声明的函数。在变量对象中以函数名建立一个属性，**属性值为指向该函数所在内存地址的引用**。**如果该函数名的属性已经存在，那么该属性将会被新的引用覆盖（被该函数所在内存地址的引用）**。

  3.检查当前上下文的**变量声明**，每找到一个变量声明，就在变量对象中以变量名建立一个属性，属性值为undefined，如果该变量名已经存在（是一个function），为了防止同名函数被修改为undefined，则会直接跳过，原属性不会被修改。

  ```javascript
  // demo
  function test() {
      console.log(a);//undefined 不会报not defined 错误，从而实现变量提升;
    	//f1();这里调用会报错。//Uncaught TypeError: f1 is not a function
      foo();

      var a = 1;//变量声明
    	var f1 = function(){ //这也是变量声明
    		console.log('f1');
  	}
      function foo() { //函数声明
          console.log('foo');
      }
  }

  test();
  ```

  ​

  ```javascript
  //test ExecutionContext创建过程
  testEC = {
      // 变量对象
      VO: {},
      scopeChain: {},
      this: {}
  }
  ```


 


```javascript
 // VO 为 Variable Object的缩写，即变量对象
VO = {
 arguments: {...},  //注：在浏览器的展示中，函数的参数可能并不是放在arguments对象中，这里为了方便理解，我做了这样的处理
  foo: <foo reference>  // 表示foo的地址引用
  a: undefined
	f1: undefined
  }
```

```javascript
   //javascript  // 执行阶段  VO ->  AO   // Active Object  
AO = {      
  arguments: {...},      
  foo: <foo reference>,      
  a: 1,      
  f1:<f1 reference>	
}
```

  

- ​       **活动对象**：

  在执行上下文的执行阶段，变量对象会变成活动对象。开始对变量赋值、执行其他代码。

- ​    **标识符提升（hoisting）**：

变量声明提升（hoisting）：变量声明提升，只是提升变量的声明，并不会把变量赋值提升上来。

函数声明提升（hoisting）：函数声明，把声明和赋值都会提升上去。

***变量声明提升***

```javascript
//实际代码
function test(){
	console.log(foo); //undefined
    console.log(a);//undefined
    var foo = function (){
        console.log('foo');
    }
    var a= 1;
}
test();
```

```javascript
//按照我们的理解后转换出来的代码（提升后的代码）
function test(){
  	var foo,a;//创建变量对象时，获取变量声明
	console.log(foo);
	console.log(a);
	foo = function(){
  		console.log('foo');
	}
    a=1;
}
test();
```

***函数声明提升 + 变量声明提升***

```javascript
//实际代码
function test(){
  console.log(foo);
  console.log(bar);
  var foo = 'Hello';
  console.log(foo);
  var bar = function () {
    return 'world';
   }
  function foo() {
    return 'hello';
   }
}
test();
```

```javascript
//按照我们的理解后转换出来的代码（提升后的代码）
function test(){
  //变量对象
  function foo() { //创建变量对象时，获取函数声明及定义。如果该函数名的属性已经存在，那么该属性将会被新的引用覆盖。
    return 'hello';
  }
  var test =foo;
  var bar, foo; //创建变量对象时，获取变量声明,如果该变量名已经存在（是一个function），为了防止同名函数被修改为undefined，则会直接跳过，原属性不会被修改.
  //变量对象->活动对象
  console.log(foo); //foo函数 function foo() {return 'hello';} 
  console.log(bar);// undefined
  
  foo = 'Hello'; //变量赋值
  console.log(foo); //根据作用域链向上找，第一个找到的是foo变量，而不是foo函数、
  bar = function () {
    return 'world';
  }
  console.log(test);//foo函数 function foo() {return 'hello';}  说明之前的foo函数没有被后面变量声明的foo变量覆盖。
}
test();
```

原因：  因为当一个函数执行时（激活），就会进入一个执行上下文。这个执行上下文会进行两个阶段；

​	其中第一步是上下文创建阶段：创建变量对象、建立作用域，确定this 的指向。其中创建变量对象包含一个步骤：检查函数声明，每找到一个函数声明，就在**该变量对象中以函数名建立一个属性，属性值为这个函数所在内存的地址引用** ，另一个步骤就是：检查变量声明，每找到一个变量声明，就在该**变量对象中以变量名建立一个属性，属性值为undefined**

​	第二步就是上下文的执行阶段：在执行阶段时，变量对象（VO）会变成活动对象（AO），这个时候才会**对活动对象中的变量赋值**。