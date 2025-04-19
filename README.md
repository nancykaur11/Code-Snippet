# 🔁 JavaScript Event Loop Interview Questions

A collection of real-world event loop interview questions with detailed answers and expected output. Covers `setTimeout`, `Promises`, `async/await`, microtasks, macrotasks, and more!

---

##  1. Classic setTimeout and Promise Order

```javascript
console.log('Start');

setTimeout(() => {
  console.log('Timeout');
}, 0);

Promise.resolve().then(() => {
  console.log('Promise');
});

console.log('End');
```

  Start<br>
  End<br>
  Promise<br>
  Timeout
```javascript
async function foo() {
  console.log('Inside foo 1');
  await Promise.resolve();
  console.log('Inside foo 2');
}

console.log('Start');
setTimeout(() => {
  console.log('Timeout');
}, 0);
foo();
console.log('End');
```

Start<br>
Inside foo 1<br>
End<br>
Inside foo 2<br>
Timeout

##  4. Microtasks Inside Macrotasks
```javascript
setTimeout(() => {
  console.log('Timeout 1');
  
  Promise.resolve().then(() => {
    console.log('Promise inside Timeout');
  });
}, 0);

Promise.resolve().then(() => {
  console.log('Promise 1');
});

setTimeout(() => {
  console.log('Timeout 2');
}, 0);
```
Promise 1<br>
Timeout 1<br>
Promise inside Timeout<br>
Timeout 2
##  5. Tricky setTimeout with Delay
```javascript
console.log('Start');

setTimeout(() => {
  console.log('Timeout after 0ms');
}, 0);

setTimeout(() => {
  console.log('Timeout after 100ms');
}, 100);

console.log('End');
```


Start<br>
End<br>
Timeout after 0ms<br>
Timeout after 100ms
##  6. async/await + setTimeout + Promise
```javascript
async function foo() {
  console.log('foo 1');
  await bar();
  console.log('foo 2');
}

async function bar() {
  console.log('bar');
}

console.log('Start');

setTimeout(() => {
  console.log('timeout');
}, 0);

foo();

Promise.resolve().then(() => {
  console.log('promise');
});

console.log('End');
```
Start<br>
foo 1<br>
bar<br>
End<br>
promise<br>
foo 2<br>
timeout
## 7. Synchronous Blocking & Microtasks
```javascript
console.log('Start');

Promise.resolve().then(() => {
  console.log('Promise 1');
});

for (let i = 0; i < 1e9; i++) {}  // Blocking loop

console.log('End');
```
Start<br>
End<br>
Promise 1
## 8. Chained Promises
```javascript
console.log('Start');

Promise.resolve()
  .then(() => {
    console.log('Promise 1');
    return Promise.resolve();
  })
  .then(() => {
    console.log('Promise 2');
  });

console.log('End');
```
Start<br>
End<br>
Promise 1<br>
Promise 2
## #9. Interleaved Macrotasks & Microtasks
```javascript
setTimeout(() => {
  console.log('setTimeout 1');
  Promise.resolve().then(() => console.log('inner Promise 1'));
}, 0);

setTimeout(() => {
  console.log('setTimeout 2');
  Promise.resolve().then(() => console.log('inner Promise 2'));
}, 0);
```
setTimeout 1
inner Promise 1
setTimeout 2
inner Promise 2
## #10. Trick with queueMicrotask
```javascript
console.log('A');

queueMicrotask(() => console.log('B'));

Promise.resolve().then(() => console.log('C'));

setTimeout(() => console.log('D'), 0);

console.log('E');
```

A<br>
E<br>
B<br>
C<br>
D
## 11. Understanding setTimeout Delays
```javascript
console.log('Start');

setTimeout(() => {
  console.log('Timeout with delay 0');
}, 0);

setTimeout(() => {
  console.log('Timeout with delay 100');
}, 100);

console.log('End');
```


Start<br>
End<br>
Timeout with delay 0<br>
Timeout with delay 100
## 12. Promise Chaining with Delays
```javascript
console.log('Start');

Promise.resolve()
  .then(() => {
    console.log('Promise 1');
    return new Promise(resolve => setTimeout(() => resolve(), 100));
  })
  .then(() => {
    console.log('Promise 2');
  });

console.log('End');
```
Start<br>
Promise 1<br>
End<br>
Promise 2
## 13. setInterval and clearInterval with Multiple Intervals
```javascript
let count = 0;
const intervalId = setInterval(() => {
  console.log(count);
  count++;
  if (count === 5) {
    clearInterval(intervalId);
  }
}, 500);
```
0<br>
1<br>
2<br>
3<br>
4
## 14. Recursive setTimeout with Delay
```javascript
function recursiveTimeout(count) {
  if (count < 5) {
    setTimeout(() => {
      console.log(count);
      recursiveTimeout(count + 1);
    }, 500);
  }
}

recursiveTimeout(0);
```


0<br>
1<br>
2<br>
3<br>
4
## 15. Handling Multiple setTimeout Calls
```javascript
console.log('Start');

setTimeout(() => {
  console.log('Timeout 1');
}, 0);

setTimeout(() => {
  console.log('Timeout 2');
}, 0);

console.log('End');
```


Start<br>
End<br>
Timeout 1<br>
Timeout 2
## 16. Promise Resolution Order
```javascript
console.log('Start');

Promise.resolve('First').then((value) => {
  console.log(value);
  return 'Second';
}).then((value) => {
  console.log(value);
});

console.log('End');
```


Start<br>
End<br>
First<br>
Second
## 17. The Impact of Microtasks on Event Loop
```javascript
setTimeout(() => {
  console.log('Timeout 1');
  Promise.resolve().then(() => console.log('Promise inside Timeout'));
}, 0);

Promise.resolve().then(() => console.log('Promise 1'));
console.log('End');
```
End<br>
Promise 1<br>
Timeout 1<br>
Promise inside Timeout
## 18. Handling async/await with Timeouts
```javascript
async function asyncFunction() {
  console.log('Start async');
  await new Promise(resolve => setTimeout(resolve, 1000));
  console.log('End async');
}

console.log('Start');

asyncFunction();

console.log('End');
```


Start<br>
Start async<br>
End<br>
End async
## 19. Promise.all and Parallel Execution
```javascript
console.log('Start');

Promise.all([
  new Promise(resolve => setTimeout(() => { console.log('Task 1'); resolve(); }, 1000)),
  new Promise(resolve => setTimeout(() => { console.log('Task 2'); resolve(); }, 500)),
]).then(() => console.log('All tasks completed'));

console.log('End');
```


Start<br>
End<br>
Task 2<br>
Task 1<br>
All tasks completed
## 20. Promise Chaining with setTimeout and setInterval
```javascript
console.log('Start');

Promise.resolve()
  .then(() => {
    return new Promise(resolve => setTimeout(() => resolve('Promise Resolved'), 500));
  })
  .then((message) => {
    console.log(message);
    setInterval(() => {
      console.log('Interval running');
    }, 1000);
  });

console.log('End');
```


Start<br>
End<br>
Promise Resolved<br>
Interval running<br>
Interval running<br>
Interval running
...
## 21. Microtasks and Macrotasks Ordering
```javascript
console.log('Start');

setTimeout(() => {
  console.log('Timeout 1');
  Promise.resolve().then(() => console.log('Promise inside Timeout'));
}, 0);

setTimeout(() => {
  console.log('Timeout 2');
  Promise.resolve().then(() => console.log('Promise inside Timeout 2'));
}, 0);

Promise.resolve().then(() => console.log('Promise 1'));
console.log('End');
```
Start<br>
End<br>
Promise 1<br>
Timeout 1<br>
Promise inside Timeout<br>
Timeout <br>
Promise inside Timeout 2
## 22. Event Loop with queueMicrotask
```javascript
console.log('Start');

queueMicrotask(() => console.log('Microtask 1'));
queueMicrotask(() => console.log('Microtask 2'));

console.log('End');
```


Start<br>
End<br>
Microtask 1<br>
Microtask 2
23. setTimeout with Recursive Calls
```javascript
let count = 0;

function recursiveTimeout() {
  if (count < 5) {
    console.log(count);
    count++;
    setTimeout(recursiveTimeout, 1000);
  }
}

recursiveTimeout();
```


0<br>
1<br>
2<br>
3<br>
4
## 24. async Function with Multiple awaits
```javascript
async function asyncFunction() {
  console.log('Start');
  await new Promise(resolve => setTimeout(resolve, 1000));
  console.log('After 1 second');
  await new Promise(resolve => setTimeout(resolve, 2000));
  console.log('After 2 seconds');
}

asyncFunction();
```

Start<br>
After 1 second

## 25. Understanding the Event Loop with setImmediate
```javascript
console.log('Start');

setImmediate(() => console.log('setImmediate 1'));
setTimeout(() => console.log('setTimeout 1'), 0);
Promise.resolve().then(() => console.log('Promise 1'));

console.log('End');
```


Start<br>
End<br>
Promise 1<br>
setImmediate 1<br>
setTimeout 1

## 26. Using setTimeout and Promises Together
```javascript
console.log('Start');

setTimeout(() => {
  console.log('setTimeout 1');
  Promise.resolve().then(() => console.log('Promise inside Timeout'));
}, 0);

Promise.resolve().then(() => console.log('Promise 1'));
console.log('End');
```

Start<br>
End<br>
Promise 1<br>
setTimeout 1<br>
Promise inside Timeout

## 27. Promise.race Example
```javascript
const promise1 = new Promise((resolve, reject) => {
  setTimeout(() => resolve('Promise 1 resolved'), 2000);
});

const promise2 = new Promise((resolve, reject) => {
  setTimeout(() => resolve('Promise 2 resolved'), 1000);
});

Promise.race([promise1, promise2]).then((message) => {
  console.log(message);
});
```

Promise 2 resolved
Why? Promise.race resolves as soon as the first promise in the array resolves. In this case, promise2 resolves first.

## 28. Asynchronous Code in for Loops
```javascript
for (var i = 0; i < 3; i++) {
  setTimeout(() => {
    console.log(i);
  }, 1000);
}
```


3<br>
3<br>
3<br>
Why? The value of i is updated before each setTimeout is executed, and by the time the setTimeout is called, the loop has finished running. To fix this, you can use let (block-scoped) or wrap the setTimeout in an immediately invoked function expression (IIFE).

## 29. The Event Loop with setInterval and clearInterval
```javascript
let counter = 0;
const interval = setInterval(() => {
  console.log(counter);
  counter++;
  if (counter === 3) {
    clearInterval(interval);
  }
}, 1000);
```


0<br>
1<br>
2<br>
Why? setInterval runs the code every 1000ms. It stops when clearInterval is called after three iterations.

## 30. setTimeout with Different Delays
```javascript
console.log('Start');

setTimeout(() => {
  console.log('Timeout 1');
}, 0);

setTimeout(() => {
  console.log('Timeout 2');
}, 100);

setTimeout(() => {
  console.log('Timeout 3');
}, 50);

console.log('End');
```


Start<br>
End<br>
Timeout 1<br>
Timeout 3<br>
Timeout 2<br>










