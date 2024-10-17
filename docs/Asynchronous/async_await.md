# async/await

```javascript
let promise = new Promise(function(resolve, reject){
  resolve();
});

promise.then(function(){
  ...
});
```

**`async/await` and `promise.then/catch`**

當我們使用 `async/await` 時，通常不需要 `.then`，因為 `await` 已經處理了等待的過程，並且可以使用常規的 `try..catch` 來替代 `.catch`。

---

- function 被加上async的意義：

指此function 一定會回傳promise物件

```javascript
async function f() {
  return 1;
}

f().then(alert); // 1
```

以上等同於：

```javascript
async function f() {
  return Promise.resolve(1);
}

f().then(alert); // 1
```

- await 的意義：

能使js等待直到promise物件執行完畢並取得結果

```javascript
async function f() {

  let promise = new Promise((resolve, reject) => {
    setTimeout(() => resolve("done!"), 1000)
  });

  let result = await promise; // 會在此等待promise執行完畢，再賦值給變數

  alert(result); // "done!"
}

f();
```

注意：`await` 不能写在一般函数中或单独出现。它必须被 `async function fn() { /* 代码 */ }` 包起来。

- 錯誤捕捉

使用try... catch... 寫法

```javascript
async function f() {
  try {
    let response = await fetch('/no-user-here');
    let user = await response.json();
  } catch(err) {
    // catches errors both in fetch and response.json
    alert(err);
  }
}

f();

```

或使用promise的.then/.catch 寫法

```javascript
async function f() {
  let response = await fetch('http://no-such-url');
}

// f() becomes a rejected promise
f().catch(alert);
```


參考資料

[JAVASCRIPT.INFO](https://javascript.info/async-await)
