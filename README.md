### 简答题

1.  请说出下列最终的执行结果，并解释为什么
    ```
    var a = [];
    for(var i=0;i<10;i++>){
        a[i] = function () {
            console.log(i);
        }
    }
    a[6]();
    ```
    > 答：<br>
    > 结果：10<br>
    > 原因：函数`a[6]`执行时，当前作用域内并不存在变量`i`,沿作用域链向上查找，在 for 循环的作用域内找到变量 i，虽然 for 循环此时已经执行完毕，但由于其中变量`i`被函数引用，并未释放，发生闭包，此时`for`循环执行完毕，`i`值为 10，所以`a[6]()`的执行结果为 10
2.  请说出下列最终的执行结果，并解释为什么
    ```
    var temp = 123;
    if(true){
     console.log(temp);
     let temp;
    }
    ```
    > 答：<br>
    > 结果：报错：Cannot access 'temp' before initialization<br>
    > 原因：let/const 在块级作用域中声明的变量，在未赋值之前形成暂时性死区，不可使用
3.  结合 ES6 新语法，用最简单的方式找出数组中的最小值？

    ```
     var arr = [12,34,32,89,4];
    ```

    > 答：<br>

    ```
    const arr = [12,34,32,89,4];
    const getMinOfArr = (arr) => [...arr].sort((a,b)=>a-b)[0];
    const min = getMinOfArr(arr)
    ```

4.  请详细说明 var、let、const 三种声明变量方式之间的具体差别

    > 答：差别如下：

    - 值是否可被修改：var/let 可以，const 不可以
    - 全局作用于是否添加到全局对象：var 会，let/const 不会
    - 未声明前是否可用：var 可用，会进行声明提升，有初值为 undefined，let/const 不可用，在块级作用域内形成暂时性死区，引用会报错

5.  请说出下列最终的执行结果，并解释为什么
    ```
    var a = 10;
    var obj = {
        a:20,
        fn(){
            setTimeout(()=>{
                console.log(this.a)
            })
        }
    }
    obj.fn();
    ```
    > 答：<br>
    > 结果：20<br>
    > 原因：`fn`为普通函数，通过`obj.fn`形式调用，将 fn 中的 this 隐式绑定为`obj`，`setTimeout`的回调箭头函数，其 this 指向定义时作用域中的 this，即为函数`fn`中的 this，即为`obj`,所以结果为 20
6.  简述 Symbol 类型的用途？

    > 答：<br>

    - 产生唯一值
    - 用作对象属性，防止命名冲突
    - 模拟私有属性

7.  说一说什么是浅拷贝，什么是深拷贝？
    > 答：<br>
    > 深浅拷贝对复杂数据类型来说才有意义。<br>
    > 复杂数据类型，在栈内存中存储数据地址，在堆内存中存储具体数据。<br>
    > 浅拷贝就是只复制栈内存中的数据的地址<br>
    > 深拷贝则是既复制栈中地址又复制堆中数据
8.  谈谈你是如何理解 js 异步编程的，Event Loop 是什么，什么是宏任务，什么是微任务？
    答：<br>

    - 异步编程的理解：js 是单线程的，所以所有的代码都是同步执行的，所谓异步，只是通过控制代码的执行流程，达到异步的效果。
    - event loop 是做什么的：用来控制代码执行流程的一套机制，以达到异步效果
      - 将异步代码块分为宏任务和微任务
      - 在任务满足回调触发条件时，将其分别放入宏微任务队列
      - 在执行栈为空时：
        - 先取微任务队列代码，直至执行完毕（在执行过程中可能还会产生各种宏微任务，只需依次放入宏微任务队列即可）
        - 取宏任务队列第一个宏任务到执行栈执行（在执行过程中可能还会产生各种宏微任务，只需依次放入宏微任务队列即可）
        - 以上两步循环往复，直至宏微任务队列均为空
    - 宏任务：script 的加载、交互时间、定时器、io 操作等居委宏任务
    - 微任务：promise、nextTick、MutationObserver 等

9.  将下面异步代码使用 Promise 改进

    ```
    setTimeout(function(){
        var a = "hello ";
        setTimeout(function(){
            var b = "lagou ";
            setTimeout(function(){
                var c = "I ❤ U";
                console.log(a+b+c)
            },10)
        },10)
    },10)

    ```

    > 答：

    ```
    //法一：链式调用
    const delay = (str,time) => {
        return new Promise((reslove)=>{
            setTimeout(()=>{
                reslove(str)
            },time)
        })
    }
    delay("hello ",10)
        .then((res)=>delay(res + "lagou ",10))
        .then((res)=>delay(res + "I ❤ U",10))
        .then(res=>console.log(res))

    //法二：then内逻辑一样，写了个递归
    const delay = (str, time) => {
        return new Promise((reslove) => {
            setTimeout(() => {
            reslove(str);
            }, time);
        });
    };
    const print = async (strArr, time, result) => {
    const [first, ...rest] = strArr;
    if (first) {
        result = await delay(result + first, time);
        return print(rest, time, result);
    } else {
        console.log(result);
    }
    };
    print(["hello ", "lagou ", "I ❤ U"], 10, "");
    ```

10. 简述 ts 与 js 之间的关系？
    > 答：ts = 类型系统 + js 新特性 + js 加强 + js
11. 请谈谈你认为的 ts 的优缺点：
    答：
    - 优点：
      - 有类型声明方便开发者查看代码
      - 错误早发现，易发现
    - 缺点：
      - 需要一定的学习成本
      - 需要定义类型，增加一定代码工作量
