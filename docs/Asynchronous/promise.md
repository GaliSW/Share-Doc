# Promise

### Promise是什麼?

- 用來封裝一個非同步的程式碼，並可以在最後獲取成功或失敗的結果值。
- Promise 是ES6出現的新技術，用於取代單純使用回調函數(callback function) 的非同步寫法，擁有鍊式的寫法，可以解決callback hell的問題。
- Promise 是一個能夠將 producing code 及 consuming code連接在一起的JavaScript物件。
    - Producing code：A “producing code” that does something and takes time. For instance, some code that loads the data over a network.
    - Consuming code：A “consuming code” that wants the result of the “producing code” once it’s ready.

### Promise 的使用方式：

producing code 部分: 

- 透過new Promise() 的方式創造promise 物件

```javascript
new Promise(function(resolve, reject) {
  // executor (執行器)
	// producing code寫在這
});
```

傳入new Promise() 中的function，稱之為執行器，當new Promise被建立時，執行器必會依序帶入兩個內建參數：resolve及reject，並自動執行。

- resolve及reject 為JS引擎內建的方法，其功能分別是：
    - resolve(value)：成功時執行的方法
    - reject(error)：失敗時執行的方法
- Promise物件的三種狀態：
    - 擱置 pending，初始狀態
    - 實現 fulfilled，表示操作成功
    - 拒絕 rejected，表示操作失敗
- 範例：

```javascript
let promise = new Promise(function (resolve, reject) {
  let a = 2
  if (a > 1) {
    resolve("success")
  } else {
		//寫法1
    reject("fail")
		//另一種寫法
    //reject(new Error("fail"))
	}
}) 
```

補充說明：promise的三種狀態不會同時存在

consuming code 部分:  (從執行器拿到資料後要做的事寫在這裡)

使用.then()、.catch()，以及.finally()

- .then()的使用
    - .then() 是 promise物件中的一個方法，並由兩個分別針對成功及失敗的結果做處理的回調函式組成。
    - .then()的範例寫法：
    
    ```javascript
    let promise = new Promise(function (resolve, reject) {
    	let a = 2
      if (a > 1) {
        resolve("success")  
      } else {
        reject("fail")  
    	}
    })
    
    //同時有成功及失敗時的處理函式
    promise.then(function(value){
    	// 成功時執行的函式
    	console.log(value)
    }, function(reason){
    	// 失敗時執行的函式
    	console.log(reason)
    })
    
    //在失敗時不執行任何函式
    promise.then(function(value){
    	//...
    })
    
    //在成功時不執行任何函式
    promise.then(null, function(reason){
    	//...
    })
    ```
    
    - 參數的傳遞
        
        ```javascript
        let promise = new Promise(function (resolve, reject) {
        	let a = 2
          if (a > 1) {
            resolve("success")   // 此處success在狀態為"完成"時傳入then中的參數value
          } else {
            reject("fail")   // 此處success在狀態為"失敗"時傳入then中的參數error
        	}
        })
        
        promise.then(function (value) {  
        	// value為"完成"時，從resolve中傳入的參數，此範例為字串"success"
          console.log(value)
        }, function (error) {
        	// error為"失敗"時，從reject中傳入的參數，此範例為字串"fail"
          console.log(error)
        })
        ```
        
        綠色背景為完成時的參數傳遞過程；紅色背景為失敗時的參數傳遞過程
        
    
    - 鍊式的特性，可以讓我們連續執行多個非同步操作。當第一個.then()執行完成後，才會執行下一個.then()，並可將前一個.then()中所return的值，當作下一個.then()的參數。
    
    ```javascript
    // 不宣告變數的promise寫法
    new Promise(function (resolve, reject) {
      setTimeout(() => {
        resolve(1)    // 成功時將數字1傳入第一個.then()中的result
      }, 1000);
    })
    .then(function (result) {  // 由resolve(1) 中的數字1傳入
      console.log(result)   // 1
      return result * 2    // 1 * 2 = 2 
    })
    .then(function (resultTwo) {  //  此處resultTwo等於2，由前一個.then()中的 return之值傳入
      console.log(resultTwo)  // 2
      return resultTwo * 2       // 2 * 2 = 4
    })
    .then(function (resultThree) {   //  此處resultThree等於4，由前一個.then()中的 return之值傳入 
      console.log(resultThree)  // 4
    })
    ```
    
- .catch()的使用
    
    在promise物件回傳”拒絕”的狀態時，除了.then()中的第二個參數能處理錯誤的情況之外，也能使用.catch() 來做為被拒絕時要執行的程式
    
    - .catch()的範例寫法
    
    ```javascript
    let promise = new Promise(function (resolve, reject) {
    	let a = 2
      if (a > 1) {
        resolve("success")   
      } else {
        reject("fail") 
    	}
    })
    
    promise.catch(function (error) {
      // reject時會執行的程式碼
      console.log(error)
    }).finally(function () {
      console.log("finally")
    })  
    ```
    
    - 為何不全部都在.then()中處理錯誤就好?
        
        都使用.then()也可以做到.catch()的功能，但.then()對於錯誤捕捉有其極限存在，在某些狀況下可能會造成問題，請見下個例子：
        
        ```javascript
        let promise = new Promise(function (resolve, reject) {
          resolve(1)
        })
        
        promise.then(function (data) {
          console.log(data)
          no_func() //假設這裡有個不存在的函式導致錯誤，
        						//程式碼便會報錯卡在這行，也不會進入function(err){...}的函式
        						//第二個.then()不會執行
        }, function(err) {
          console.log(err)
        }).then(function(data){
        	console.log(123)
        })
        ```
        
        若在promise物件回傳完成後，.then中第一個參數function(data){…}中程式碼出現錯誤，並不會跳至reject狀態時的錯誤處理函式，此時整個瀏覽器便會報錯並卡在出錯的那行，不再往下執行。因此，我們可以加上.catch()來避免此狀況：
        
        ```javascript
        let promise = new Promise(function (resolve, reject) {
          resolve(1)
        })
        
        promise.then(function (data) {
        
          console.log(data)
          no_func() // 假設此函式不存在，瀏覽器雖會報錯，但也會跳至.catch()中執行錯誤捕捉
        
        }, function (err) {
        
          console.log(err)
        
        }).then(function(data){
        
        	console.log(123)
        
        }).catch(function (err) {
          //resolve()中的no_func()不存在，而觸發錯誤
        	//並不會進到第一個.then()中的function(err){...}
        	//因為function(err){}必須在reject()時才會被執行，因此跳至catch
          
        	console.log(err)
        })
        ```
        
- .finally()的使用
    - finally()是在所有操作都完成後，不論完成或失敗，都要執行的東西
    - 使用finally的時機：
        - 若在promise物件回傳狀態後，不論完成或拒絕，有皆要執行的程式碼，可提出放置於finally中，讓程式碼更簡潔。
        - 在promise的操作執行完畢後，可以用來回復預設狀態。
    
    ```javascript
    let isLoading = true;
    
    fetch(myRequest)
      .then((response) => {
        const contentType = response.headers.get("content-type");
        if (contentType && contentType.includes("application/json")) {
          return response.json();
        }
        throw new TypeError("Oops, we haven't got JSON!");
      })
      .then((json) => {
        /* process your JSON further */
      })
      .catch((error) => {
        console.error(error); // this line can also throw, e.g. when console = {}
      })
      .finally(() => {
        isLoading = false;
      });
    ```
    

### Promise API

- Promise.resolve()

Promise.resolve() 是一個靜態方法，前面一定要以Promise開頭。

Promise.resolve() 只會產生 fulfilled(已實現)狀態的 Promise 物件，寫法等同於：

```javascript
let promise = new Promise(resolve => resolve(value));
```

- Promise.reject()

Promise.reject() 是一個靜態方法，前面一定要以Promise開頭。

Promise.reject() 只會產生 rejected(已拒絕)狀態的 Promise 物件，寫法等同於：

```javascript
let promise = new Promise((resolve, reject) => reject(reason));
```

- Promise.all()

Promise.all() 用於我們想要同時執行多個promise物件，直到他們的狀態都回傳fulfilled。

Promise.all()的參數一般會以陣列方式帶入，若這個陣列中的每一個promise物件皆為”實現”的狀態，Promise.all()才會回傳”實現”的狀態；相反的，若是這個陣列中有一個promise物件的狀態為”拒絕”，則promise.all()的狀態便會回傳”拒絕”。

寫法為：

```javascript
let promise = Promise.all([...])
```

```javascript
const p1 = Promise.resolve(123)
const p2 = "文字"
const p3 = new Promise((resolve, reject) => {
  setTimeout(() => resolve('foo'), 1000)
})

Promise.all([p1, p2, p3])
.then(value => {
  console.log(value)   // [123, "文字", "foo"]
})
.catch(err => {
  console.log(err)
})
```

陣列中的值如果不是 Promise 物件，會自動使用Promise.resolve()方法來轉換

- Promise.allSettled()

Promise.allSettled()會等所有的promise都執行完，不論promise結果為實現或拒絕

```javascript
const p1 = Promise.reject(123)
const p2 = "文字"
const p3 = new Promise((resolve, reject) => {
  setTimeout(() => resolve('foo'), 1000)
})

Promise.allSettled([p1, p2, p3])
.then(value => {
  console.log(value)   // [123, "文字", "foo"]
})
.catch(err => {
  console.log(err)
})
```

- Promise.race()

Promise.race()的用法與Promise.all()雷同，不同之處在於Promise.race() 只要在所傳入的參數中，

取第一個執行完成的promise物件之值，不論其狀態為實現或拒絕。

```javascript
const p = Promise.reject("this is rejected")  // 這項第一個完成
const p1 = Promise.resolve(123)
const p2 = "文字"   
const p3 = new Promise((resolve, reject) => {
  setTimeout(() => resolve('foo'), 1000)
})

Promise.race([p, p1, p2, p3])
.then(value => {
  console.log(value)   
})
.catch(err => {
  console.log(err)   // "this is rejected"
})   
```

- Promise.any()

Promise.any()的用法與Promise.race()雷同，但只對第一個狀態為”實現”的promise有作用。

寫法為：

```javascript
let promise = Promise.any([...]);
```

Any object that has a method named “ then” is called a “thenable” object.

```javascript
// Promise: 初始化、then、catch、finally
let my_promise = new Promise(function (resolve, reject) {
  let a = 1
  if (a >= 1) {
    resolve("success")
  } else {
		//寫法1
    reject("fail")
		//寫法2
    //reject(new Error("fail"))
	}
})
my_promise.then(function (data) {
  //resolve時會執行的，如果確定不會被執行，則function(data){...}可用null取代
  console.log("resolve")
  console.log(data)
}, function (error) {
  //reject時會執行的，如果確定不會被執行，則可省略
  console.log("reject")
  console.log(error)
  // console.error(error)
})

my_promise.catch(function (error) {
  // reject時會執行的
  console.log("reject")
  console.log(error)
}).finally(function () {
  console.log("finally")
})  
```

```javascript
//寫一個delay函式，會回傳Promise物件，取代setTimeout
function delay(ms) {
  // 寫法1
  return new Promise(function (resolve, reject) {
    setTimeout(() => {
      resolve()
    }, ms);
  })
  // 寫法2
  // let my_promise = new Promise(function (resolve, reject) {

  // })
  // return my_promise
}

delay(2000).then(function () {
  console.log("resolve")
})
```

從Promise開始的JavaScript異步生活

[https://eyesofkids.gitbooks.io/javascript-start-es6-promise/content/](https://eyesofkids.gitbooks.io/javascript-start-es6-promise/content/)

Promises, async/await 

[https://javascript.info/async](https://javascript.info/async)

JavaScript - Promise 基礎語法

[https://www.youtube.com/watch?v=K53NMu8Y5tM](https://www.youtube.com/watch?v=K53NMu8Y5tM)


Promise從入門到精通

[https://www.youtube.com/watch?v=DF34DpYL5Jo&list=PLmOn9nNkQxJF-I5BK-wNUnsBkuLXUumhr](https://www.youtube.com/watch?v=DF34DpYL5Jo&list=PLmOn9nNkQxJF-I5BK-wNUnsBkuLXUumhr)
