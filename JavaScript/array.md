# Array
 - Array is zero-indexed, used for storing a collection of different data types under a single variable name.

```
let arr = [10,15,20,25];
let arr1 = new Array(1,2,3,4,5);
let arr2 = new Array(5).fill(10);
console.log(arr[2]);  // returns 3rd element - 20
```
### length - returns array length
```
arr.length; // 4
```

### push - adds element to end
```
arr.push(30); //  [ 10, 15, 20, 25, 30 ]
```
### shift - removes first element
``` 
arr.shift(); // [ 15, 20, 25, 30 ]
```
### unshift - add new element as first element
``` 
arr.unshift(5); // [ 5, 15, 20, 25, 30 ]
```
### pop - removes last element
```
arr.pop(); //  [ 5, 15, 20, 25 ]
```
### lastIndexOf - returns index from end
```
let arr5 = [40, 60, 50, 40, 30, 40]
let lastIndex = arr5.lastIndexOf(40);   // 5
```
### find - returns first element which matches condition
```
arr.find((ele) => ele > 15 ) // 20
```

### findIndex - returns first index matches condition
```
arr.findIndex((ele) => ele > 15 ) // 2
```

### includes - if array has that value returns true else false.
```
arr.includes(20);  // true
```
### filter - returns array of elements matching condition
```
arr.filter((ele) => ele < 30 ) //  [ 5, 15, 20, 25 ]
```
### some - validates atleast one element matches condition
```
arr.some((ele) => ele % 10 === 0); // true
```

### every - validates all elements match the condition
```
arr.every((ele) => ele % 10 === 0); // false
```

### map - creates a new array with resultant elements from function.
```
arr.map((x) => x *5); // [ 50, 75, 100, 125 ]
```

### reduce - runs a "reducer" callback function over all elements, in ascending-index order, and accumulates them into a single value.
```
arr.reduce(((accu,curVal) => accu + curVal), 0); // 70
```

### reverse - reverses an array in place and returns the reference to the same array.
```
arr.reverse(); // [ 25, 20, 15, 10 ]
```
### sort - sorts stringified elements of an array in place and returns the reference to the same array
```
arr.sort(); // [ 10, 15, 20, 25 ]
```
