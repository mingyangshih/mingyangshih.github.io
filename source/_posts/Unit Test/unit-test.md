---
title: Introduction of Frontend unit test with Jest
date: 2022-02-11 11:25:30
categories: Unit Test
description: Introduction of Frontend unit test with Jest
tags:
  - Frontend
  - Unit Test
---

### 為什麼要寫測試

在開發時許多功能開發後要進行一連串動作驗證功能的正確性，如：重新點擊特定目標、重新填寫表單、送出表單等，當功能越來越複雜時，人工測試時間也會越來越長，撰寫測試工具可以節省許多時間並避免上線後才發生錯誤。

* 避免修改程式碼後的錯誤：修改程式的過程中可能會影響到其他程式碼，A 處修改的原始碼卻使看似毫無關聯的 B 處錯誤。
* 不需要每次修改都重新人工測試。

### 前端測試類別

1. Unit Test : 是以行為進行測試，驗證運行是否符合結果。
2. E2E Test : 模擬使用者在瀏覽器上的行為進行測驗。

### Jest
Jest 在 React 中有許多開發者使用，Vue-Cli 中也是可做為預設的單元測試選項。

單元測試的基本觀念是對 `function` 做測試，下面以一個簡單的運算做範例，會新增兩個檔案一個是 `count.js`(function 的行為) 另一個是 `count.test.js`(function的測試腳本)。

要被測試的檔案必須 `export` 才能被測試檔案接收並測試。
``` js
// count.js
const count = {
  count: function(bill, price) {
    return bill - price;
  }
};

module.exports = count;
```

在 `count.js` 檔名中補上 `.test.` 作為測試檔(測試預設的檔名)，測試黨內容會明確寫上 `測試的目標敘述 test(...)` ，並且定義 `測試的結果是否符合預期 expect()...` 。

```js
// count.test.js
const count = require('./count');

test('Test bill - price', () => {
  const bill = 200; 
  const price = 127;

  // 期望結果是符合預期的
  expect(count.count(bill, price)).toBe(73);
});
```
<font color=#ffff00> 上方的測試腳本可拆成三個部分來看：</font>
1. 描述測試內容或目標 `test('...', () => {})`
2. 引用要測試的函式 `count.count()`
3. 測試的期望是什麼 `expect(...).toBe(...)` 

### 安裝環境並執行測試

1. `npm install jest --save-dev` 安裝 Jest
2. 打開 `package.json` 內將 script 內新增方法並加入 `jest` 
3. 執行  `npm run test` 查看結果，若執行成功會看到 <font color=#ffff00> `passed` </font> 文字

``` json
{
  "dependencies": {
    "jest": "^27.5.1"
  },
  "scripts": {
    "test": "jest"
 // 使用 npm run testwatch 時會持續用監控的形式，而不是只有單一次報告
  },
  // ...
}
```
監控測試：可將 `package.json` 中 `scripts` 方法改為如下，就可以不用每次都執行一次 `npm run test` 
``` json
{
  "dependencies": {
    "jest": "^27.5.1"
  },
  "scripts": {
    "testwatch": "jest --watchAll"
 // 使用 npm run testwatch 時會持續用監控的形式，而不是只有單一次報告
  },
  // ...
}
```

### VSCode 搭配套件
除了使用 terminal 執行測試外，Jest 與 VSCode 也有整合好的套件可以使用，不需要每次都執行 `npm run test`，即可以在每次存擋後看到測試的結果。
(Jest, Jest Snippets)。

新增一個 `jest.config.js` ， 此測試檔案預設僅需要會出一空的物件即可。(即使用官方預設)
``` js
module.exports = {
}
```

### Jest snippets cheat sheet
`tb` -> `expect().toBe()`
`tblt` -> `expect().toBeLessThan()`
`tblte` -> `expect().toBeLessThanOrEqual()`

### 常見的條件驗證方式 - matchers
寫測試時，要讓值符合預期，Jest 中的 `expect` 後可以使用 `matchers` 做為條件驗證，如上方的範例 `expect(...).toBe(...)` 中的 `toBe` 就屬於 `matchers` ，作為檢測不同條件的驗證使用。

* 開發中較常見的 `matchers`，新增 `fn.js` 作為範例。在檔案中定義了一些方法，這些方法會回傳計算結果、`null`、`undefined` 等各種值，另外還會回傳物件<font color=#ffff00> (物件的驗證概念與純值不同) </font>

``` js
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
  }
}
module.exports = fns
```

* 在檢查純值時除了 `toBe` 外，還有其他 `matchers` 可使用。
* 在JS中如果檢查 `NaN === NaN` 會回傳 `false` 因為 `NaN` 不等於任何值，在 Jest 中可使用 `toBeNaN` 或 `toBe(NaN)` 做檢測。
* 判斷 `truthy` `falsy` 並不一定完全是布林值的 `true` `false` ，可以使用 `toBeFalsy`、`toBeTruthy` 進行驗證。

``` js 
const fns = require('./fn');
// 判斷值是否完全相符，判斷是使用 Object.is()
test(' Test add ', () => {
  expect(fns.add(1,3)).toBe(4);
})
// 測試回傳值是否為 null
test('Test null', () => {
  expect(fns.isNull()).toBeNull();
})
// 測試回傳值是否為 undefined
test('Test undefined', () => {
  expect(fns.isUndefined()).toBeUndefined();
});
// 測試回傳值是否為 NaN
test('Test NaN', () => {
  expect(fns.isNaN()).toBe(NaN)
  expect(fns.isNaN()).toBeNaN()
});
// 判斷 truthy or falsy
test('Test Truthy and Falsy', () => {
  expect(fns.checkValue(2)).toBeTruthy()
  expect(fns.checkValue(0)).toBeFalsy()
});
```

### 物件檢測
* <font color=#ffff00> 物件比對 </font> : JavaScript 物件特性 <font color=#00ff> 物件是傳參考不是傳值 </font> 所以會出現物件內容相同但比較後的結果是 `false` 的情況。
* 在 Jest 中也是一樣的概念，如果直接使用 `toBe` 檢測物件，儘管屬性值都相同，一樣會得到 `failed`的結果。要改成使用 `toEqual` 才能檢測兩個物件內的值是否相同。

``` js
const a = {
  a : 'a'
}
console.log(a === {a: 'a'}) // false
```

``` js 
test('Test Object', () => {
  // 下方執行會 failed
  expect(fns.createUser()).toBe({name: '小明'})
  // 應該要改成
  expect(fns.createUser()).toEqual({name: '小明'})
});
```

### toBe 及 toEqual 的差異
* `toBe` 是使用 `Object.is` 做判斷，不是使用 `===` 。
* `toEqual` 是深度比對(deep equality)，全部透過 `Object.is` 比對物件或陣列內的值；如同將物件內的值一一取出比對，因此效能可能較差。

### 數值比較
Jest 有提供 大於、小於、大於等於、小於等於 等方法：

``` js
test('Test little than 10', () => {
  const num1 = 5;
  const num2 = 4;
  expect(num1 + num2).toBeLessThan(10);
});

test('Test little than or equal 10', () => {
  const num1 = 5;
  const num2 = 5;
  expect(num1 + num2).toBeLessThanOrEqual(10);
});
```

### 字串檢查

除了使用 `toBe` 外，還可以使用 `toMatch` 搭配 Regular Expression 正規表達式進行驗證，如檢測Email格式

``` js
test('測試 email 格式是否正確', () => {
  expect('gres@gmail.com').toMatch(
    /^\w+((-\w+)|(\.\w+))*\@[A-Za-z0-9]+((\.|-)[A-Za-z0-9]+)*\.[A-Za-z]+$/
  );
});
```

### Array 內是否含有特定值

Array 與 Object 檢測相同都是使用 `toEqual()` ，此外也可以使用 `toContain` 檢查 Array 中是否包含特定值。

``` js
test('Test Array Value', () => {
  const newArray = [1,2,3];
  expect(newArray).toContain(3);
});
```

### describe 

`describe` 的用途是提供一個群組的敘述：
執行 `npm run test` 後就可以顯示群組名稱及各個測試的內容。

``` js
describe('....', () => {
  test('...',() => {});
  test('...',() => {});
  test('...',() => {});
})
```



### 參考資料
[十分鐘上手前端單元測試 - 使用 Jest](https://www.casper.tw/development/2020/02/02/jest-intro/?fbclid=IwAR17OgIYp2Vsk8mdvR8C9oxMvdQgvaTr4pCsQd77buwFdbrokgt48g6UwdQ)
[Jest 官網](https://jestjs.io/docs/getting-started)
