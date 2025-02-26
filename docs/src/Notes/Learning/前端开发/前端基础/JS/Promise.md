
-------------------------------------------------------------------------------
一、什么是 Promise？
-------------------------------------------------------------------------------
在 JavaScript 中，Promise 是一种处理异步操作的对象，用来简化传统回调函数（callback）在异常处理、业务逻辑拆分等方面的复杂度。Promise 提供了更清晰的语法结构，让我们可以用 .then ()、. Catch () 或 async/await 这样的写法来组织异步代码。

-------------------------------------------------------------------------------
二、Promise 的基本用法
-------------------------------------------------------------------------------
1. 创建一个 Promise  
   你可以通过构造函数 “new Promise ()” 来创建自定义 Promise。该函数接收一个执行器函数 (executor)，并且执行器函数会接收两个回调参数：resolve 和 reject，分别表示成功与失败的逻辑。

```ts
// 创建一个简单的 Promise 示例
const myPromise = new Promise<number>((resolve, reject) => {
  const success = true; // 模拟一个条件，比如异步任务是否成功

  if (success) {
    resolve(42); // 成功时，返回数据 "42"
  } else {
    reject(new Error('Operation failed')); // 失败时，抛出一个错误
  }
});

myPromise
  .then((data) => {
    console.log('成功，接收到数据：', data);
  })
  .catch((error) => {
    console.error('失败，错误信息：', error);
  });
```

2. Then () 与 catch ()  
   - Then (): 当 Promise 状态变为 fulfilled（成功）时会执行 then () 回调，并获取到通过 resolve () 传递的参数。  
   - Catch (): 当 Promise 状态变为 rejected（失败）时会执行 catch () 回调，并获取到通过 reject () 或抛出的错误信息。

3. Finally ()  
   - Finally () 会在 then () 和 catch () 之后被调用，无论 Promise 成功或失败都执行。通常可以在这里做一些“收尾”操作，例如关闭网络连接、隐藏加载动画等。

```ts
myPromise
  .then((data) => {
    console.log('成功，接收到数据：', data);
  })
  .catch((error) => {
    console.error('失败，错误信息：', error);
  })
  .finally(() => {
    console.log('Promise 执行结束');
  });
```

-------------------------------------------------------------------------------
三、async/await 的用法
-------------------------------------------------------------------------------
在 ES 8 中引入了 async/await，它是基于 Promise 的语法糖，可以让异步逻辑更接近同步写法，阅读起来更加直观。

1. Async 函数  
   在函数声明前加上 async，表示该函数内部可能会使用 await 关键字来等待 Promise 被解决（resolve 或 reject）。

2. Await 关键字  
   Await 可以暂停函数的执行，直到 Promise 被解决，然后返回结果；若 Promise 被拒绝 (reject)，会抛出异常。

```ts
async function fetchData() {
  try {
    const data = await myPromise;
    console.log('获取到的数据：', data);
  } catch (error) {
    console.error('捕获到的错误：', error);
  }
}

fetchData();
```

-------------------------------------------------------------------------------
四、Promise 在 Nest. Js 中的应用
-------------------------------------------------------------------------------
在 Nest. Js 的数据获取或处理逻辑中，你通常会看到以下几种场景：

1. Service 中调用数据库或外部 API  
   在 Service 层，我们常常需要调用数据库库（如 TypeORM、Prisma）或外部 HTTP API（通过 HttpModule ）。

```ts
// user.service.ts
import { Injectable } from '@nestjs/common';
import { PrismaService } from './prisma.service'; // 假设使用 Prisma 

@Injectable()
export class UserService {
  constructor(private readonly prisma: PrismaService) {}

  // 通过 async/await 获取用户列表
  async getAllUsers() {
    return await this.prisma.user.findMany(); 
  }
}
```

2. Controller 中调用 Service  
   在 Controller 中，通过 async/await 调用 Service。Nest. Js Controller 的方法也可异步返回 Promise，Nest. Js 会自动处理返回值。

```ts
// user.controller.ts
import { Controller, Get } from '@nestjs/common';
import { UserService } from './user.service';

@Controller('users')
export class UserController {
  constructor(private readonly userService: UserService) {}

  @Get()
  async getUsers() {
    // Service 返回一个 Promise，这里使用 await
    const users = await this.userService.getAllUsers();
    return users;
  }
}
```

3. 处理错误  
   在 Nest. Js 中，如果异步逻辑出现错误，可抛出 HttpException 或其他自定义异常，框架会自动将其转化为 HTTP 错误响应。

```ts
// user.service.ts
import { Injectable, NotFoundException } from '@nestjs/common';

@Injectable()
export class UserService {
  async findUserById(id: number) {
    const user = await this.prisma.user.findUnique({ where: { id } });
    if (!user) {
      throw new NotFoundException(`User with id ${id} not found`);
    }
    return user;
  }
}
```

-------------------------------------------------------------------------------
五、Promise 的几个高级用法
-------------------------------------------------------------------------------
1. Promise.All ()  
   用于并行执行多个异步任务，等待它们都完成后再处理结果。如果其中一个任务失败，则整体 Promise 立即变为 rejected。

```ts
const promiseA = new Promise((resolve) => setTimeout(() => resolve('结果A'), 1000));
const promiseB = new Promise((resolve) => setTimeout(() => resolve('结果B'), 2000));
const promiseC = new Promise((resolve, reject) => setTimeout(() => reject('Error C'), 1500));

Promise.all([promiseA, promiseB, promiseC])
  .then((results) => {
    // 如果都成功，这里能拿到 ['结果A', '结果B', ...]
    console.log('All results:', results);
  })
  .catch((error) => {
    // 如果有失败，这里会被触发
    console.error('Error in Promise.all:', error);
  });
```

2. Promise.Race ()  
   只要有一个 Promise 最先改变状态，就会返回该 Promise 的结果（Succcess 或 Error）。

```ts
const slowPromise = new Promise((resolve) => setTimeout(() => resolve('Slow'), 3000));
const fastPromise = new Promise((resolve) => setTimeout(() => resolve('Fast'), 1000));

Promise.race([slowPromise, fastPromise]).then((winner) => {
  console.log('Winner is:', winner); // Fast
});
```

3. Promise.AllSettled ()  
   等待所有 Promise 都结束（无论成功或失败），然后以对象数组的形式返回结果的状态和值（或原因）。

```ts
const p1 = Promise.resolve('p1');
const p2 = Promise.reject(new Error('p2 rejected'));

Promise.allSettled([p1, p2])
  .then((results) => {
    console.log(results);
    // [
    //   { status: 'fulfilled', value: 'p1' },
    //   { status: 'rejected', reason: Error('p2 rejected') }
    // ]
  });
```

-------------------------------------------------------------------------------
六、在 Nest. Js 中使用 Promise 的注意事项
-------------------------------------------------------------------------------
1. 不要在同一业务流程中混用多种回调模式  
   例如，同时使用回调函数（callback）与 async/await，会令代码逻辑混乱。最好统一使用 async/await。

2. 错误处理  
   如果没有在 Service 中使用 try/catch，需要在 Controller 中进行处理，或者让 Nest. Js 内部捕获并返回给客户端。如果异步流程较为复杂，可以使用 RxJS 或者管道（Pipes/Interceptors）进行额外处理。

3. 性能与可维护性  
   虽然 Promise 可以并行任务和简化异步逻辑，但过多或不必要的并行调用也会给数据库或外部服务带来压力。需要合理规划异步逻辑与并发量。

-------------------------------------------------------------------------------
七、总结
-------------------------------------------------------------------------------
1. **Promise** 是 JavaScript 专门为解决异步操作而设计的对象，提供了明显的代码可读性提升。  
2. 在 **Nest. Js** 框架中，尤其是服务端的数据库查询和 HTTP 请求场景，经常会使用 Promise（或 async/await 语法）来编写异步函数。  
3. **. Then () / .catch () / .finally ()** 和 **async/await** 只是不同的用法场景，同样基于 Promise。一般推荐 **async/await**，因其语义更直观。  
4. 当你在 Nest. Js 中做数据获取或处理时，记得随时留意异常处理和结果返回的规范。  
5. 充分利用 **Promise.All ()**、**Promise.Race ()**、**Promise.AllSettled ()** 等高级方法，可以帮助你优化并发、提升性能，也能应对更复杂的业务需求。


## 八、promise 原理揭晓
────────────────────────────────────────────────────────
### 1 . Promise 基本概念
────────────────────────────────────────────────────────
Promise 是 JavaScript 中用于处理异步操作的一种对象模型。它的核心思想是：一个 Promise 代表了一个即将完成或失败的异步操作及其结果。

• 三种状态 (state):
  1. Pending (进行中)
  2. Fulfilled (已成功)
  3. Rejected (已失败)

状态一旦从 pending 变为 fulfilled 或者 rejected，就不能再变化。

• Promise 的好处:
  1. 更好地管理回调地狱。  
  2. 逻辑更清晰，支持链式调用。  
  3. 与 async/await 结合后，写法更接近同步代码。

────────────────────────────────────────────────────────
### 2. Promise 的核心原理
────────────────────────────────────────────────────────
1. 创建 Promise 对象时，需要传入一个“执行器函数”，该函数接收两个参数：resolve 和 reject  
2. 执行器函数内可以执行异步操作，异步操作成功时调用 resolve (value)，失败时调用 reject (reason)。  
3. Promise 会根据调用 resolve () 或 reject () 的时机，触发 fulfilled 或 rejected 状态。  

下面是一段最简单的手写示例，展示了 Promise 的结构和原理（无需在项目中照搬，仅用于理解）：

```js
class MyPromise {
  constructor(executor) {
    // 初始状态为 pending
    this.state = 'pending';
    this.value = null;
    this.reason = null;
    this.onFulfilledCallbacks = [];
    this.onRejectedCallbacks = [];

    const resolve = (value) => {
      if (this.state === 'pending') {
        this.state = 'fulfilled';
        this.value = value;
        // 执行所有成功回调
        this.onFulfilledCallbacks.forEach(fn => fn(value));
      }
    };

    const reject = (reason) => {
      if (this.state === 'pending') {
        this.state = 'rejected';
        this.reason = reason;
        // 执行所有失败回调
        this.onRejectedCallbacks.forEach(fn => fn(reason));
      }
    };

    // executor 在创建 Promise 时同步执行，
    // 如果执行过程中抛出错误也会进入 reject。
    try {
      executor(resolve, reject);
    } catch (error) {
      reject(error);
    }
  }

  then(onFulfilled, onRejected) {
    // onFulfilled, onRejected 均为可选
    if (typeof onFulfilled !== 'function') {
      onFulfilled = value => value;
    }
    if (typeof onRejected !== 'function') {
      onRejected = reason => { throw reason };
    }

    if (this.state === 'fulfilled') {
      onFulfilled(this.value);
    } else if (this.state === 'rejected') {
      onRejected(this.reason);
    } else {
      // 如果还是 pending 状态，则把回调函数先保存起来
      this.onFulfilledCallbacks.push(onFulfilled);
      this.onRejectedCallbacks.push(onRejected);
    }
  }
}
```

此示例只是帮助你理解 Promise 内部是如何管理异步结果和回调队列的。在实际使用中，你只需要了解创建与使用方式即可，无需自行实现。

────────────────────────────────────────────────────────
### 3 . Promise 的用法
────────────────────────────────────────────────────────
最常见的 Promise 用法流程如下：

1. 创建 Promise 对象：new Promise ((resolve, reject) => {...})
2. 处理成功和失败：. Then ()、. Catch ()、. Finally ()

以下是一个简单的示例，模拟一个异步请求，通过 setTimeout 模拟网络延迟。

```js

function getDataFromServer() {
  return new Promise((resolve, reject) => {
    // 模拟异步操作，如网络请求或数据库查询
    setTimeout(() => {
      const success = Math.random() > 0.5; // 50% 几率成功
      if (success) {
        resolve('数据获取成功！');
      } else {
        reject(new Error('数据获取失败！'));
      }
    }, 1000);
  });
}

// 使用 .then() 和 .catch() 处理结果
getDataFromServer()
  .then(data => {
    console.log('成功返回:', data);
  })
  .catch(error => {
    console.error('发生错误:', error.message);
  })
  .finally(() => {
    console.log('请求完成，无论成功或失败都会执行的操作');
  });
```
```

• .then(onFulfilled, onRejected)  
  - onFulfilled：Promise 成功时执行  
  - onRejected：Promise 失败时执行

• .catch(onRejected)  
  - 捕获 Promise 中的异常或 reject。与在 then 中传第二个参数效果类似，但更通用可读。

• .finally(onFinally)  
  - 无论成功还是失败，都会在最后执行。常用于清理场景（如关闭加载动画）。
```


────────────────────────────────────────────────────────
### 4. async/await 与 Promise
────────────────────────────────────────────────────────
ES2017 (ES8) 引入了 async/await 语法，让异步代码写起来更接近同步风格。  
• async 标识一个函数是异步函数。  
• await 等待一个返回 Promise 的函数执行完成，然后才会继续往下。

示例：

```js
```js
function fetchData() {
  return new Promise((resolve) => {
    setTimeout(() => {
      resolve('模拟异步数据');
    }, 1000);
  });
}

async function main() {
  try {
    console.log('开始请求...');
    const data = await fetchData(); // 等待 Promise 完成
    console.log('获取到数据:', data);
  } catch (error) {
    console.error('发生异常:', error);
  } finally {
    console.log('请求已结束');
  }
}

main();
```
```

• 如果 fetchData() reject 或抛出异常，则会被 try/catch 捕获。  
• async/await 只是 Promise 的语法糖，本质还是基于 Promise
```

────────────────────────────────────────────────────────
### 5. Promise 的并发处理
────────────────────────────────────────────────────────
在多个异步任务之间，你可能需要并发执行并采集结果，或等待所有任务返回后再执行后续逻辑。Promise 提供了以下静态方法处理并发：


1. Promise.all([...])  
   同时发起多个 Promise，全部执行成功后返回结果数组；只要有一个 Promise 失败，就会直接进入 catch 逻辑。

```js
```js
const p1 = Promise.resolve(1);
const p2 = Promise.resolve(2);
const p3 = Promise.resolve(3);

Promise.all([p1, p2, p3])
  .then(results => {
    // results: [1, 2, 3]
    console.log('全部成功，结果为:', results);
  })
  .catch(error => {
    console.error('某个任务失败:', error);
  });
```
```

2. Promise.race([...])  
   只要有一个 Promise 先变为 settled(fulfilled 或 rejected)，就立即进入 then 或 catch，对后面结果不感兴趣。

```js
```js
const pSlow = new Promise(resolve => setTimeout(() => resolve('慢'), 2000));
const pFast = new Promise(resolve => setTimeout(() => resolve('快'), 1000));

Promise.race([pSlow, pFast])
  .then(result => {
    // result: '快'
    console.log('最先返回的结果:', result);
  })
  .catch(error => {
    console.error('出现错误:', error);
  });
```
```

3. Promise.allSettled([...])  
   等待所有 Promise 都返回后才结束，即使有的失败，有的成功，也会汇总到一个结果数组里。适合需要收集所有执行情况的应用场景。

```js
```js
const pA = Promise.resolve('任务A成功');
const pB = Promise.reject('任务B失败');
const pC = Promise.resolve('任务C成功');

Promise.allSettled([pA, pB, pC])
  .then(results => {
    console.log('任务执行情况:');
    results.forEach((res, index) => {
      console.log(`Promise ${index}:`, res);
    });
  });
```
```
```
`
────────────────────────────────────────────────────────
### 6. 在 Nest.js 中的应用
────────────────────────────────────────────────────────
Nest.js 强调模块化与依赖注入，常见的异步场景包括：  
• 数据库操作 (TypeORM, Prisma 等)  
• 网络请求 (HttpService)  
• 文件操作或第三方 API 调用

以下是一个在 Nest.js 中典型的 Service 和 Controller 示例，展示如何结合 async/await（基于 Promise）来获取数据：

• Service 层：
```ts
```ts
import { Injectable } from '@nestjs/common';
import { PrismaService } from '../prisma.service'; // 假设使用 Prisma

@Injectable()
export class UserService {
  constructor(private prisma: PrismaService) {}

  async findAll() {
    // 返回 Promise
    return this.prisma.user.findMany();
  }

  async findOne(id: number) {
    return this.prisma.user.findUnique({ where: { id } });
  }
}
```
```

• Controller 层：
```ts
```ts
import { Controller, Get, Param } from '@nestjs/common';
import { UserService } from './user.service';

@Controller('users')
export class UserController {
  constructor(private readonly userService: UserService) {}

  @Get()
  async getAllUsers() {
    // 这里是 async 方法，用 await 获取返回的 Promise
    const users = await this.userService.findAll();
    return users;  // Nest.js 自动转化为 JSON 返回
  }

  @Get(':id')
  async getUserById(@Param('id') id: string) {
    const user = await this.userService.findOne(Number(id));
    if (!user) {
      return { message: 'User not found' };
    }
    return user;
  }
}
```
```
在上面这种典型场景下，Nest.js 会自动等待 Promise 结束并将结果返回给 HTTP 客户端。如果发生异常（比如数据库连接失败），Nest.js 也会捕获并返回 500 错误，或者你可以通过异常过滤器来定制响应。
```
``
────────────────────────────────────────────────────────
### 7. 常见注意事项
────────────────────────────────────────────────────────
1. 错误处理：如果不使用 try/catch，在 then 中传递 onRejected 或在 catch 中处理错误，避免 Promise 链出现“未捕获的 Promise 异常”。  
2. 不要在同一段异步逻辑中混用 callback、Promise、async/await，否则容易导致流程混乱。  
3. 对于并发量较大的请求，注意使用 Promise.all() 会一次性发起所有请求，可能带来资源消耗大的问题——需要根据业务情况进行批量处理或使用限制并发的策略。  
4. 阅读第三方库或框架源码时，如果看到返回的是 Promise 对象，就应结合 async/await 或 then() 来获取结果。

────────────────────────────────────────────────────────
### 8 . 总结
────────────────────────────────────────────────────────
• Promise 是现代 JavaScript 处理异步逻辑的主流方式，提供了更好的可读性和错误处理机制。  
• async/await 让代码更加直观，同步化的写法在逻辑和流程上更易于维护。  
• Nest.js 内置就支持 async/await 来编写异步请求和数据库操作，避免了层层回调。  
• 学会运用 Promise 及其静态方法（all、race、allSettled 等），能让你更好地控制并发和结果收集。  

通过上面这些示例与讲解，相信你对 Promise 已经有了更系统、深入的理解。在实际项目中灵活运用 Promise，可以极大提升你的异步编程效率和代码质量。若你还有更多疑问，欢迎继续提问！
