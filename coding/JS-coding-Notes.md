Here is a structured JavaScript cheatsheet for Coding

### Array – Common Operations

```js
let arr = [10, 20, 30, 40, 50];

// Access              → O(1)
arr[2]                  // 30

// indexOf             → O(n)     (returns index or -1)
arr.indexOf(30)         // 2
arr.indexOf(99)         // -1

// find                → O(n)     (returns first matching element or undefined)
arr.find(x => x > 25)   // 30

// findIndex           → O(n)     (returns index or -1)
arr.findIndex(x => x > 25)   // 2

// slice(start, end)   → O(n)     (non-destructive)
arr.slice(1, 4)         // [20, 30, 40]

// splice(start, deleteCount, ...items) → mutates array
arr.splice(2, 1, 99)    // removes 1 item at index 2, inserts 99
// arr is now [10, 20, 99, 40, 50]
```

### Array – split / join

```js
let str = "a,b,c,d";

// split → string → array
let chars = str.split(',');     // ["a", "b", "c", "d"]
let letters = "hello".split(''); // ["h","e","l","l","o"]

// join → array → string
let joined = chars.join('-');   // "a-b-c-d"
let word = letters.join('');     // "hello"
```

### .flat vs .flatMap

```js
let nested = [1, [2, 3], [4, [5, 6]]];

nested.flat(1);       // [1, 2, 3, 4, [5, 6]]     depth 1
nested.flat(2);       // [1, 2, 3, 4, 5, 6]       depth 2
nested.flat(Infinity);// fully flattened

// flatMap = map + flat(1)
let words = ["hello world", "good morning"];
words.flatMap(s => s.split(' '));
// → ["hello", "world", "good", "morning"]
```

### Set – O(1) lookup / uniqueness

```js
let input = [1,1,2,3,3,4];

// Remove duplicates (most common interview way)
let unique = [...new Set(input)];          // [1,2,3,4]

// Using Set methods
let s = new Set();
input.forEach(x => s.add(x));
console.log([...s]);                        // [1,2,3,4]

console.log(s.has(3));     // true          ← O(1)
s.delete(3);
console.log(s.has(3));     // false
```

### Map – key can be anything (object, array, function…)

```js
let m = new Map();

// Add / override (same key → replaces)
m.set("key1", 100);
m.set("key1", 200);         // overrides
m.set({id:1}, "object key"); // valid

console.log(m.get("key1")); // 200
console.log(m.has("key1")); // true        ← O(1)

// Iteration
for (let [key, value] of m) {
    console.log(key, value);
}
```

### Object – useful checks & methods

```js
let obj = { a: 1, b: 2, c: 3 };

console.log(!!Object.values(obj).length);   // true
console.log(Object.keys(obj).length > 0);   // true   ← common check

console.log(Object.values(obj));    // [1,2,3]
console.log(Object.entries(obj));   // [["a",1], ["b",2], ["c",3]]

// seal vs freeze
Object.seal(obj);   // can't add/delete keys, but can change values
Object.freeze(obj); // can't add/delete/change anything

// dynamic access
let prop = "b";
console.log(obj[prop]);     // 2
console.log(obj.b);         // 2 (same when key is known)
```

### ?? (nullish coalescing) vs || (logical OR)

```js
let a = 0;
let b = "";
let c = null;
let d = undefined;

console.log(a || "default");   // "default"   ← 0 is falsy
console.log(a ?? "default");   // 0           ← only null/undefined

console.log(b || "default");   // "default"
console.log(b ?? "default");   // "" 

console.log(c ?? "default");   // "default"
```

### !! → convert to boolean (double negation)

```js
console.log(!!0);         // false
console.log(!!"");        // false
console.log(!!"hello");   // true
console.log(!![]);        // true
console.log(!!{});        // true
```
