[toc]

## Summary

1. [type conversion](https://www.bilibili.com/video/BV1M14y1i7D4/?spm_id_from=pageDriver&vd_source=527fa4a946b2b7aae00b08df635436bc)
2. [arithmetic operation](https://www.bilibili.com/video/BV16X4y1x7U2/?spm_id_from=333.788&vd_source=527fa4a946b2b7aae00b08df635436bc) 
3. [expression](https://www.bilibili.com/video/BV1TX4y1x79x/?spm_id_from=333.788&vd_source=527fa4a946b2b7aae00b08df635436bc) 

## Type Conversion
### 1. Primitive -> Number
1. true: 1
2. false: 0
3. null: 0
4. undefined: NaN
5. string 
  - empty string, string containing space (e.g., '', ' ', '\n', '\r', '\t'): 0
  - number string is number, else NaN.

### 2. any -> bool
1. null: false
2. undefined: false
3. number:
  - 0: false
  - NaN: false
  - other: true
4. string:
  - only empty string (''): false
  - other: true
5. object: true
  
### 3. primitive -> string
1. null: 'null'
2. undefined: 'undefined'
3. number: 'number'
4. boolean：
  - true: 'true'
  - false: 'false'

### 4. object -> primitive
*Step 1*: call ```[Symbol.toPrimitive]``` if available in an object. Return primitive. Else, throw error.
- we can override ```[Symbol.toPrimitive]```
```js
const a = {
  [Symbol.toPrimitive]() {
    console.log('toPrimitive');
    return 1; // return primitive number 1 (change this to 'this', an error is thrown)
  }
}

if (a == 1) {
  console.log('nice');
}

// toPrimitive
// nice
```
*Step 2*: call ```obj.valueOf```: every object provide this method.
- we can override ```obj.valueOf```
```js
const a = {
  valueOf() {
    console.log('valueOf');
    return 1;
  },
  toString() {
    console.log('toString');
    return 1;
  }
}

if (a == 1 && a == 2 && a == 3) {
  console.log('Nice');
}
/*
valueOf
valueOf

*/

```
*Step 3*: If still obj, call ```obj.toString```
- we can override ```obj.toString```

*Step 4*: If still obj, throw error

```js
const a = {
  valueOf() {
    console.log('valueOf');
    return this;
  },
  toString() {
    console.log('toString');
    return 1;
  }
}

if (a == 1 && a == 2 && a == 3) {
  console.log('Nice');
}
/*
valueOf
toString
valueOf
toString
*/

```
Note: changing valueOf or toString of an object might alter the object behavior.



## Operation
### 1. Arithmatic Operator: + - * / % ++ --

Step 1: convert to primitive type

Step 2: check if there's special case
1. convert to number type and perform operation.
2. special case: x + y where x or y is a string, convert to string and concatenate.
3. special case: compute** NaN with other type get NaN.

```js
/* Example 1
* Step 1: both are primitive.
* Step 2: No special case, then convert to number.
* null --> 0
* undefined --> NaN 
*/
console.log(null + undefined); // NaN


/*Example 2
* Step 1: Not primitive, convert to primitive
* console.log([].valueof() + {}.valueof();
* console.log([].toString() + {}.toString())
* console.log('' + '[object Object]')
* Step 2: it's special case, string concatenation.
*/
console.log([] + {}); // '[object Object]'
```


### 2. Comparison Operators > < >= <= == != === !==

#### 1.  ```> < >= <=```

Step 1: convert to convert to primitive type

Step 2: 
1. convert to number type and compare.
2. special case: both are strings, compare ASCII value of each characters.
3. special case: either / both are NaN, must be false.

#### 2. ```===```
1. type and value must be same.
2. special case: either / both are NaN, must be false.


#### 3. ```==```
1. both are same type, compare values.
2. both are primitive type, convert to number and compare.
3. one is primitive, another is object, convert object to primitve and compare.
4. special case: undefined and null can only compare with themselves or with each other, otherwise return false.   
5. special case: either / both are NaN, must be false.

#### 4. ```!= !==```
negate ```==``` or ```===```

```js
/* Example 1
* console.log(0 > NaN);
*/
console.log(null > undefined);
```

### 3. Logical Operators ! && || ?:

convert to boolean

1. x && y
  - if x is falsy, return x
  - if x is truthy, return y 
2. x || y
  - if x is falsy, return y
  - if x is truthy, return x

## Expression
Data:
1. literal: 
```js
1;
'a';
[1, 2, 3];
```
2. variable: 
```js
let a = 123;
```
3. expression: must have return value
```js
1 + 2;
a * 2; 
a && b; 
[a + b, a - b]; 
arr[arr[2]];
[1,2,3][a];
1 || 2
1 && a;
```
## Application

1. Default

```js
var limit;

// verbose
if (limit === null || limit === undefined) {
  limit = 10;
} 

// Optimised
limit = limit || 10;
```

2. Safe access of property value

```js
var user = {
  name: 'deng',
  addr: {
    provine: 'a',
    city: 'b',
  },
};

/*
Potential Issues:
user might be null.
user.addr might be null.
city might be null.
*/
if (user.addr.city) {
  //...
}

// Optimised
if (user && user.addr) {
  console.log(user.addr.city);
}

// Further Optimised
console.log(user && user.addr && user.addr.city);

```

3. early return
```js
var isFound = false;

if (isFound === true) {
  console.log('a');
} else {
  console.log('b');
}

// Optimised
// if (data) {...}
if (isFound) {
  console.log('a');
} else {
  console.log('b');
}
```

4. null and object
```js
var user = null;

if (user === null) {
  console.log('please log in');
}

// Optimised
if (!user) {
  console.log('please log in');
}

// 
```

5. Conversion

```js
// Number
var page = '10';
page = Number(page);
console.log(page, typeof page);

// Optimised
page = +page


// Boolean
var x = 'aaa';
y = Boolean(x);
console.log(y);

// Optimised

y = !!x;
console.log(y);


```