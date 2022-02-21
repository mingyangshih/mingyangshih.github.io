---
title: Introduction of Frontend unit test with Jest - Asynchronous and Ajax
date: 2022-02-21 08:32:03
categories: Unit Test
description: Introduction of Frontend unit test with Jest - Asynchronous and Ajax
tags:
  - Frontend
  - Unit Test
---

JavaScript is syncing and single thread language, so if there is asynchronous event will push into event queue until other codes which has been executed, event in event queue will be executed.

### Example

``` js
// fn.js
const axios = require('axios');
const fns = {
  add: (num1, num2) => num1 + num2,
  isNull: () => null,
  isUndefined: () => undefined,
  isNaN: () => NaN,
  checkValue: (val) => val,
  createUser: () => {
    return {
      name: '小明'
    }
  },
  fetchData: (num) => {
    return axios.get(`https://jsonplaceholder.typicode.com/todos/${num}`)
    .then(res => res.data)
    .catch(err => 'error')
  }
}
module.exports = fns
```

### Test file

`Promise` is common async method. Besides `Promise` there are `Async` , `Await` these two methods can execute in Node.js, so they can be used in `Jest`.

``` js
describe('Test Asynchronous', () => {
  test('Test Async', () => {
    expect.assertions(10);
    // get data in then, so expect need to write in then, to make sure Promise has been executed.
    return fns.fetchData(10).then(data => {
      // check return data
      console.log(data)
      // check if data.title equal to expect data.
      expect(data.title).toEqual('illo est ratione doloremque quia maiores aut')
    })
  })
  // Use ES6 async, await
  test('Test Async 2', async () => {
    expect.assertions(1);
    let res = await fns.fetchData(10);
    console.log(res)
    expect(res.title).toEqual('illo est ratione doloremque quia maiores aut')
  })
})
```

### Assertions
Purpose of `assertions` is to make sure get `Promise` data. If we want to make sure our code to get `Promise Resolve` data, we can use `expect.assertions(num)` (num represents number of assertinos).

當補上 `expect.assertions(1)` 就必須使用 `resolve` 的結果才能通過驗證， `expect.assertions` 在 `async` 的寫法也會更重要。
