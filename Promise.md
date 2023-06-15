# 基本概念

## 函数和对象和实例对象

- 实例对象就是通过new关键字生成出来的在堆空间中存在的对象，称为实例对象
- 函数对象是指一个构造函数本身，但并不需要实例化

## 同步回调和异步回调

- 同步回调

  - ```js
    const arr = [1, 2, 3];
    arr.forEach(item => {
        console.log(item);
    });
    console.log('输出');
    //	会先等forEach执行完毕之后再执行后面的语句，虽然foeEach里面也算是回调函数，这就是同步回调
    ```

- 异步回调

  - ```js
    setTimeout(()=>{
       console.log('这是异步回调'); 
    }, 0);
    console.log('输出');
    //	setTimeout中的回调函数是异步回调函数，在执行的时候，会先放入到异步队列中，而先执行后面的语句，等所有的语句都执行完毕之后，就开始执行异步队列中的语句
    ```

## 错误类型

- Error：所有错误的类型
- ReferenceError：引用的变量不存在
- TypeError：数据类型不正确的错误
- RangeError：数据值不在其所允许的范围内
- SyntaxError：语法错误

## 错误处理

- try catch
- throw new Error

# promise

## promise的概念

- promise是一个新的异步编程解决方案
  - 旧的解决方案是单纯的使用回调函数
- 具体表达
  - promise是一个构造函数
  - 从功能上来说，promise对象用来封装一个异步操作并获得其结果

## promise的状态改变

- pending为状态

- pending变为resolved——>成功
- pending变为rejected——>失败
- 只有这两种，且一个promise对象智能改变一个，无论成功和失败都会有一个结果数据，成功的结果一般为value，失败的一般为reason

## Promise的属性

- promise.PromiseStatu——》当前promise的状态
  - pending	处理中
  - resolved 或  fullfilled都是成功
  - rejected   失败

## promise的基本使用

- ```js
  const p = new Promise((resolve, reject) => {
      const n = Math.floor(Math.radom() * 100);
      setTimeout( () => {
          if (n > 50) {
              resolve(n);
          }else {
              reason(n);
          }
      }, 1000);
  });
  p.then(
  	value => {
         console.log('成功调用！' + n); 
      },
     reason => {
         console.log('失败调用！' + n); 
     }
  );
  ```

# 如何使用promise

## Promise的API

- Promise(executor)；构造函数

  - executor函数：执行器（resolve，reject） => {}
  - resolve函数：内部定义异步函数成功时调用，在then中
  - reject函数：内部定义函数失败时调用
  - excutor中是立即同步执行的

- then指定回调函数——》属于实例对象

  - onResolved函数，成功的回调函数 value => {};
  - onRejected函数，失败的回调函数 reason => {};
  - 返回一个新的promise对象

- catch指定失败的回调函数——》属于实例对象

  - onRejected函数，失败的回调函数 reason => {};

- resolve(参数)——》属于函数对象

  - 如果参数为非Promise对象，则生成一个成功状态下的promise对象，并返回

  - 如果参数为promise对象，则生成的对象的状态为参数的状态

  - ```js
    let p = Promise.resolve(111);
    p = Promise.resolve(new Promise( (resolved, rejected) => {
       rejected(123);		//会调用失败的回调函数
       resolved(456);		//调用成功的回调函数
    }));
    p.then(value => {
      console.log(value);
    },reason => {
      console.warn(reason);
    });
    ```

- reject(参数)——》函数对象

  - 不管传入的参数是Promise对象还是非Promise对象返回的都是一个失败的对象

  - ```js
    let p = Promise.reject(456);			//	失败对象
    // let p = Promise.reject(new Promise( (resolved, rejected) => {		//	失败对象
    //     resolved(123);
    // }));
    p.catch(reason => {
      console.warn(reason);		//	失败的结果就是传入的那个参数，如果参数为非Promise对象则就是那个数
        					//	如果是Promise对象，则reason就是那个Promise对象
    });
    ```

- all(数组)——》函数对象，数组里面的元素都是Promise对象

  - 当数组里面的所有Promise的状态都是成功时，则返回的Promise对象的状态就是成功，有一个失败则返回的是失败对象。

  - 当都成功时，返回的值就是所有成功Promise对象的成功值所组成的数组

  - 当失败时，返回值是那个失败的Promise对象的值

  - ```js
    const p1 = Promise.resolve('success');
    const p2 = Promise.resolve(new Promise( (resolve, rejecte) => {
      resolve('okok!');
    }));
    const p3 = Promise.reject('j');
    const p = Promise.all([p1, p2, p3]);
    console.log(p);		//	状态为失败的Promise对象
    ```

- race(数组)——》函数对象

  - 参数也是promise对象，其中参数中的对象谁先改变状态，就返回那个promise对象

# Promise的关键

## Promise中的then方法

- then方法中的回调函数是异步执行的

- ```js
  const p = new Promise( (resolve, reject) => {
      resolve(2);
      console.log(1);
  });
  p.then(value => {
      console.log(value);
  });
  console.log(3);
  //	执行结果为132
  ```

- 

## 改变Promise的状态

- 在执行器中调用成功和失败的回调

- 抛出异常会更改为rejected

  - ```js
    const p = new Promise((resolve, reject) => {
        resolve(value);		//		resolved
        reject(reason);		//		rejected
        thorw '出错';			//	rejected
    })
    ```

## 为一个Promise对象指定多个多个回调函数

- 都会执行

- ```js
  const p = new Promise( (resolve, reject) => {
      resolve('ok');
  });
  p.then(value =>{
      console.log(value);
  });
  p.then(value => {
      alter(value);
  });
  ```

- 按照指定的顺序，依次执行

## 改变Promise的状态和指定回调函数，谁先谁后

- 两种情况都有可能，当执行器函数中的任务是异步任务时，则先指定回调函数，如果执行器中的函数是同步函数，则先改变状态

- ```js
  const p = new Promise((resolve, reject) => {
      resolve(value);		//	先改变装填，后指定
      setTimeout(()=>{
          resolve(value);
      }, 1000)			//	由于setTimeout是异步任务，所以会先指定回调
  });
  p.then(value => {
      
  }, reason => {
      
  };
  ```

## then方法的返回

- 如果回调函数中返回的是一个非Promise对象，则返回的那个promise对象的状态就是resolved，如果返回的是一个promise对象，则返回的就是这个Promise对象的状态和值，如果没有return，返回的Promise对象的值是undefined

- ```js
  const p1 = p.then(value => {
      // return new Promise((resolve, reject) => {
      //     reject('1');
      // });
  }, reason => {
      console.warn(value);
  });
  console.log(p1);
  ```

## then方法的链式调用

- 由于then方法的返回值也是一个promise对象，也就可以继续给这个promise对象继续指定回调函数

- ```js
  const p = new Promise((resolve, reject) => {
      resolve('ok');
  });
  p.then(value => {
       return new Promise((resolve, reject) => {
             resolve('success');
       });
  }).then(value => {
       console.log(value);
   });
  ```

## 异常穿透

- 在then方法的链式调用中，只需要在最后指定失败的回调，即可，则这个链中任意出现失败，则调用这个失败回调

- ```js
  const p = new Promise((resolve, reject) => {
      resolve('ok');
  });
  p.then(value => {
      console.log(1111);
  }).then(value => {
      console.log(2222);
      thorw 'err';
  }).then(value => {
      console.log(3333);
  }).catch(reason => {
      console.warn(1234):
  });
  ```

- 任意一个回调中出现错误，则调用最后一个失败回调

## 中断Promise链

- 在链中，有且只有一个中断方法，就是在正确或错误回调函数中，返回一个pending状态的promise对象，则后续的方法都不会执行，返回其他任何，都不会中断

- ```js
  const p = new Promise((resolve, reject) => {
      resolve('ok');
  });
  p.then(value => {
      console.log(1111);
  }).then(value => {
      console.log(2222);
      return new Promise(() => {});
  }).then(value => {
      console.log(3333);
  }).catch(reason => {
      console.warn(1234):
  });
  ```

# async/await

## aysnc

- 写在函数前面

- 返回值是一个Promise对象

- 如果这个函数的返回值是一个非Promise对象，则返回的Promise对象的状态为成功，resolved，结果为函数的返回值

- ```js
  async function example() {
      
  }
  const p = example();	//	返回一个Promise对象，且为成功状态，结果为undefined
  ```

- 如果函数的返回值是一个promise对象，则async返回的promise对象的状态和值，依据函数返回的那个promise对象

- ```js
  async function example() {
  	return new Promise((resolve, reject) => {
          resolve(1);
      });
  }
  const p = example();	//	返回的状态为fulfilled，结果为1
  ```

  

- 函数中抛出异常的话，返回的是一个失败的promise对象，结果为抛出的结果

- ```js
  async function example() {
      throw 'error';
  }
  const p = example();		//	返回的promise对象状态为rejected，结果为error
  ```

## await

- await必须放在async函数中，否则是语法错误

- 如果await的右边是一个非Promise对象，则返回的就是那个值

- 如果await的右边是一个promise对象，且状态为成功，则会返回成功的那个值

- ```js
  async () => {
      const p = new Promise((resolve, rejcte) => {
          resolve(111);
      });
      const example = await p;
      console.log(example);		//	输出值为1
  }
  ```

- 如果await右边的promise对象状态为失败，则会抛出一个异常，捕获以后，捕获的值，则为失败的那个结果

- ```js
  (async () => {
      const p = new Promise( (resolve, reject) => {
          reject('error');
       });
      try {
          const example = await p;
      }catch (e) {
          console.log(e);
      }
  })()
  ```

# Js异步之宏队列与微队列

- js中的所有异步任务都会放在栈中，为了不阻塞主线程上程序的运行，只有在异步任务触发时才会调入到异步队列中等待执行
- 在Js中事件、Ajax、setTimeout、Promise都是异步的
- 其中事件、Ajax、setTimeout都放在宏队列中
- Promise放在微队列中
- 微队列中的任务优先级永远高于宏队列

