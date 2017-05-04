### es6的promise的学习笔记
- promise 原理
    - promise是es6的异步编程解决方案， 是es6封装好的对象；
    - 一个promise有三种状态：Pending（进行中）、Resolved（已完成，又称 Fulfilled）和Rejected（已失败）；
    - 缺点：一旦执行无法取消；无回掉函数，promise只在内部报错；当处于pending状态时无法监控其是刚开始还是即将结束
- 用法测试
    ```javascript
    //方法编写
    function getjson (num){
        var timer=null
        var promising=new Promise(function(resolve,reject){
            if(!num){
                reject("num don't find")
            }
            timer=setInterval(function(){
                if(num<5){
                    num++
                }else{
                    resolve(num)
                    clearInterval(timer)
                }
            },1000)
        });
        return promising;
    }
    //调用方式1 成功
    getjson(1).then(function(json){
        console.log(222)
        console.log(json)
    },function(error){
        console.log(111)
        console.log(error)
    })
    //调用方式2 成功
    getjson(1).then(function(json){
        console.log(222)
        console.log(json)
    }).catch(function(error){
        console.log(111)
        console.log(error)
    })
    //调用方式3 失败
    getjson(1).then(function(json){
        console.log(222)
        console.log(json)
    }).catch(function(error){
        console.log(111)
        console.log(error)
    })
    //输出结果是5秒之后打印出222和json的值
    
    ```
- 注意点
    - 必须new promise对象，同时传递resolve,reject两个参数，resolve是成功后的调用，reject是失败后的调用；
    - 调用方式2优于调用方式1，理由是Promise 对象的错误具有“冒泡”性质，会一直向后传递，直到被捕获为止。也就是说，错误总是会被下一个catch语句捕获，第二种写法可以捕获前面then方法执行中的错误，也更接近同步的写法（try/catch）；
    - done()和finally()方法在有的浏览器可能不存在。打印执行后的getjson()对象，在原型里没有找到这两个方法，原因未明；
- 源码：git@github.com:Frankltf/js-promise.git