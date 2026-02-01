# JavaScript Cheatsheet: Logic, Algorithms & Rules

## 1. Core Fundamentals (Rules & Syntax)

### Timers
*   `setTimeout`: Executes a function **once** after a specified delay (ms).
*   `setInterval`: Executes a function **repeatedly** at specified intervals.
*   `clearInterval`: Stops the interval execution.

### Regex (Inline Usage)
*   `\w`: Matches word character `[a-zA-Z0-9_]`.
*   `\d`: Matches digit `[0-9]`.
*   **Usage:** `str.match(/\d/g)` (finds all digits).

### Object
*   **Rules:**
    1.  `obj.key` vs `obj[key]`: Bracket notation is used for dynamic keys.
    2.  **Override:** Assigning to an existing key replaces the value.
    3.  **Check:** Use `!!Object.keys(obj).length` to check if empty.
*   **Methods:**
    *   `Object.values(obj)`: Returns array of values.
    *   `Object.entries(obj)`: Returns array of `[key, value]` pairs.
    *   `Object.freeze(obj)`: Prevents any changes.
    *   `Object.seal(obj)`: Prevents add/delete, allows updates.

### Map (O(1))
*   **Rules:**
    1.  **Add:** `.set(key, value)`.
    2.  **Check:** `.has(key)`.
    3.  **Access:** `.get(key)`.
    4.  **Override:** `.set()` on existing key replaces the value.

### Set (O(1))
*   **Rules:**
    1.  Stores unique values.
    2.  **Add:** `.add(value)`.
    3.  **Check:** `.has(value)`.

### Array Methods (Looping & Mutating)
*   `indexOf`: Returns index of element (-1 if not found).
*   `find`: Returns first matching **element**.
*   `findIndex`: Returns first matching **index**.
*   `slice(start, end)`: **Extracts** (Non-mutating).
*   `splice(start, count)`: **Modifies** (Removes/Adds).
*   `flat(depth)`: Flattens nested arrays.
*   `flatMap`: Maps then flattens (depth 1).

---

## 2. Algorithm & Logic Questions

### Easier Questions

#### 1. Swap Two Numbers
*Input: a=5, b=10 → Output: a=10, b=5*

```javascript
let a = 5, b = 10

// Method A: Using Temp Variable
let temp = a
a = b
b = temp

// Method B: Using Destructuring (Modern ES6)
[a, b] = [b, a]
console.log(a, b)
```

#### 2. Destructuring
*Extract values from objects/arrays.*

```javascript
// Method A: Array Destructuring
const input = [1, 2, 3]
const [first, second, third] = input
console.log(first) // 1

// Method B: Object Destructuring
const user = { name: "John", age: 30, city: "NY" }
const { name, age } = user
console.log(name) // John
```

#### 3. First Letter Uppercase
*Input: "hey whats up" → Output: "Hey Whats Up"*

```javascript
let input = "hey whats up set"

// Method A: Split, Map, Join
let resA = input.split(' ').map(word => word[0].toUpperCase() + word.slice(1)).join(' ')

// Method B: Regex Replace
let resB = input.replace(/\b\w/g, char => char.toUpperCase())

console.log(resA)
```

#### 4. Reverse String (Whole)
*Input: "Hello" → Output: "olleH"*

```javascript
let str = "Hello"

// Method A: Built-in methods
let resA = str.split('').reverse().join('')

// Method B: Decrementing For Loop
let resB = ""
for(let i = str.length - 1; i >= 0; i--) {
    resB += str[i]
}
console.log(resA)
```

#### 5. Check Palindrome
*Input: "madam" → Output: true*

```javascript
const str = "madam"

// Method A: Reverse and Compare
const isPalA = str === str.split('').reverse().join('')

// Method B: Two Pointer Loop
const isPalB = (s) => {
    let left = 0, right = s.length - 1
    while(left < right) {
        if(s[left] !== s[right]) return false
        left++; right--
    }
    return true
}
console.log(isPalA)
```

#### 6. Factorial (Recursion)
*Input: 4 → Output: 24*

```javascript
// Method A: Standard Recursion
const factorialA = (n) => n <= 1 ? 1 : n * factorialA(n - 1)

// Method B: Iterative (While Loop)
const factorialB = (n) => {
    let res = 1
    while(n > 0) res *= n--
    return res
}
console.log(factorialA(4))
```

#### 7. Check Prime Number
*Input: 17 → Output: true*

```javascript
const isPrime = (n) => {
    if (n <= 1) return false
    for (let i = 2; i <= Math.sqrt(n); i++) {
        if (n % i === 0) return false
    }
    return true
}
console.log(isPrime(17))
```

#### 8. Currying Function
*Transform f(a,b,c) into f(a)(b)(c)*

```javascript
// Method A: Arrow Functions
const sumA = (a) => (b) => (c) => a + b + c

// Method B: Closure Function
function sumB(a) {
    return function(b) {
        return function(c) {
            return a + b + c
        }
    }
}
console.log(sumA(1)(2)(3))
```

#### 9. Convert Promise to Async Function
*Refactor Promise chain to Async/Await.*

```javascript
const getData = () => Promise.resolve("Data Retrieved")

// Method A: .then() Chain
const handleA = () => {
    getData().then(res => console.log(res))
}

// Method B: Async/Await
const handleB = async () => {
    const res = await getData()
    console.log(res)
}
handleB()
```

---

### Intermediate Questions

#### 10. Find Frequency
*Input: [1, 1, 2, 3, 3, 4] → Output: { '1': 2, '2': 1, '3': 2, '4': 1 }*

```javascript
let input = [1, 1, 2, 3, 3, 4]

// Method A: Reduce (One-liner)
const freqA = input.reduce((acc, curr) => (acc[curr] = (acc[curr] || 0) + 1, acc), {})

// Method B: ForEach Loop
const freqB = (arr) => {
    let obj = {}
    arr.forEach(ele => obj[ele] = (obj[ele] || 0) + 1)
    return obj
}
console.log(freqA)
```

#### 11. Remove Duplicates
*Input: [1, 1, 2, 3, 3, 4] → Output: [1, 2, 3, 4]*

```javascript
let input = [1, 1, 2, 3, 3, 4]

// Method A: Set (Fastest)
const resA = [...new Set(input)]

// Method B: Filter + IndexOf
const resB = input.filter((item, index) => input.indexOf(item) === index)

// Method C: Reduce
const resC = input.reduce((acc, curr) => acc.includes(curr) ? acc : [...acc, curr], [])
console.log(resA)
```

#### 12. Intersection of Two Arrays
*Input: arr1=[5,6,7], arr2=[6,7,8] → Output: [6, 7]*

```javascript
let arr1 = [5, 6, 7]
let arr2 = [6, 7, 8]

// Method A: Filter + Includes
const interA = arr1.filter(x => arr2.includes(x))

// Method B: Set (O(N) for large arrays)
const setB = new Set(arr2)
const interB = arr1.filter(x => setB.has(x))
console.log(interA)
```

#### 13. Target Sum (Two Sum)
*Input: [10, 20, 30], Target 30 → Output: [0, 1] (Indices)*

```javascript
let input = [10, 20, 30]
let target = 30

// Method A: Using Map (One Pass - Efficient)
const findIndexA = (nums, t) => {
    const map = new Map()
    for(let i=0; i<nums.length; i++) {
        let complement = t - nums[i]
        if(map.has(complement)) return [map.get(complement), i]
        map.set(nums[i], i)
    }
}

// Method B: Brute Force (Nested Loops)
const findIndexB = (nums, t) => {
    for(let i=0; i<nums.length; i++) {
        for(let j=i+1; j<nums.length; j++) {
            if(nums[i] + nums[j] === t) return [i, j]
        }
    }
}
console.log(findIndexA(input, target))
```

#### 14. Rotate Array by K
*Input: [2,3,4,-5,6,2,0,9], k=3 → Left: [-5,6,2,0,9,2,3,4]*

```javascript
let input = [2, 3, 4, -5, 6, 2, 0, 9]
let k = 3

// Method A: Slice + Concat (Left Rotate)
const resA = input.slice(k).concat(input.slice(0, k))

// Method B: Slice + Concat (Right Rotate)
const resB = input.slice(-k).concat(input.slice(0, -k))

// Method C: Splice (Mutates original)
input.splice(0, 0, ...input.splice(k)) // Left rotate logic on `input` array directly
console.log(resA)
```

#### 15. Fibonacci Sequence
*Input: 7 → Output: [0, 1, 1, 2, 3, 5, 8]*

```javascript
// Method A: Loop
const fibA = (n) => {
    if(n<=0) return []
    if(n===1) return [0]
    let res = [0, 1]
    for(let i=2; i<n; i++) res.push(res[i-1] + res[i-2])
    return res
}

// Method B: Generator
function* fibB(n) {
    let [a, b] = [0, 1]
    while(n-- > 0) {
        yield a
        [a, b] = [b, a + b]
    }
}
console.log([...fibB(7)])
```

#### 16. Max Difference in Array
*Input: [1, 2, 90, 10, 110] → Output: 109 (110-1)*

```javascript
let input = [1, 2, 90, 10, 110]

// Method A: Reduce (Track Min)
const maxDiff = input.reduce((acc, curr) => {
    let diff = curr - Math.min(...input.slice(0, input.indexOf(curr) + 1))
    return diff > acc ? diff : acc
}, 0)

// Method B: Logic (Best - O(N))
const maxDiffB = (arr) => {
    let minVal = arr[0]
    let maxD = 0
    for(let i=1; i<arr.length; i++) {
        if(arr[i] - minVal > maxD) maxD = arr[i] - minVal
        if(arr[i] < minVal) minVal = arr[i]
    }
    return maxD
}
console.log(maxDiffB(input))
```

#### 17. Reverse Number
*Input: 12345 → Output: 54321*

```javascript
let num = 12345

// Method A: While Loop
let revA = 0
let n = num
while(n > 0) {
    revA = revA * 10 + n % 10
    n = Math.floor(n / 10)
}

// Method B: String Conversion
const revB = Number(String(num).split('').reverse().join(''))
console.log(revA)
```

#### 18. String Anagram
*Input: "listen", "silent" → Output: true*

```javascript
// Method A: Sort & Compare
const isAnagramA = (s1, s2) => {
    return s1.split('').sort().join('') === s2.split('').sort().join('')
}

// Method B: Character Map
const isAnagramB = (s1, s2) => {
    if(s1.length !== s2.length) return false
    const map = {}
    for(let char of s1) map[char] = (map[char] || 0) + 1
    for(let char of s2) {
        if(!map[char]) return false
        map[char]--
    }
    return true
}
console.log(isAnagramA("listen", "silent"))
```

#### 19. Closure (Counter)
*Create a counter that increments.*

```javascript
// Method A: Function Factory
const counterA = () => {
    let count = 0
    return () => ++count
}
const c = counterA()
console.log(c()) // 1
console.log(c()) // 2
```

---

### Hard Questions

#### 20. Flatten Nested Array
*Input: [[1,2],[3,[4]],5] → Output: [1,2,3,4,5]*

```javascript
let input = [[1, 2], [3, [4]], 5]

// Method A: Built-in flat()
const resA = input.flat(Infinity)

// Method B: Reduce + Concat (Recursive)
const flattenB = (arr) => {
    return arr.reduce((acc, val) => 
        Array.isArray(val) ? acc.concat(flattenB(val)) : acc.concat(val), []
    )
}

// Method C: Stack (Iterative)
const flattenC = (arr) => {
    const stack = [...arr]
    const res = []
    while(stack.length) {
        const next = stack.pop()
        if(Array.isArray(next)) stack.push(...next)
        else res.push(next)
    }
    return res.reverse()
}
console.log(flattenB(input))
```

#### 21. Remove Duplicate Objects
*Input: [{id:1, n:'A'}, {id:2, n:'B'}, {id:1, n:'A'}] → Unique by ID*

```javascript
let input = [{id:1, name:'A'}, {id:2, name:'B'}, {id:1, name:'A'}]

// Method A: Filter + FindIndex
const uniqueA = input.filter((val, index, self) =>
    index === self.findIndex((t) => t.id === val.id)
)

// Method B: Map (Preserves order)
const uniqueB = (arr) => {
    const seen = new Map()
    return arr.filter(obj => {
        if(!seen.has(obj.id)) {
            seen.set(obj.id, true)
            return true
        }
        return false
    })
}
console.log(uniqueA)
```

#### 22. Intersection of Arrays with Objects
*Input: arr1=[{id:1}], arr2=[{id:1}, {id:2}] → Output: [{id:1}]*

```javascript
let arr1 = [{id:1}, {id:2}]
let arr2 = [{id:2}, {id:3}]

// Method A: Filter + Set (Lookup by ID)
const ids = new Set(arr2.map(o => o.id))
const resA = arr1.filter(o => ids.has(o.id))

// Method B: Some (Slower O(N*M))
const resB = arr1.filter(o1 => arr2.some(o2 => o1.id === o2.id))
console.log(resA)
```

#### 23. Find Uppercase Index
*Input: "He llaBellA" → Output: [0, 5, 10]*

```javascript
const input = "He llaBellA"

// Method A: For Loop
const idxA = []
for(let i=0; i<input.length; i++) {
    if(input[i] >= 'A' && input[i] <= 'Z') idxA.push(i)
}

// Method B: Reduce
const idxB = input.split('').reduce((acc, char, i) => {
    if(char === char.toUpperCase() && char !== ' ') acc.push(i)
    return acc
}, [])
console.log(idxA)
```

#### 24. Group Elements (Divide by 2)
*Input: [10, 15, 20, 25, 30] → Output: { 0: [10,20,30], 1: [15,25] }*

```javascript
let input = [10, 15, 20, 25, 30]

// Method A: Reduce
const groupA = input.reduce((acc, num) => {
    const key = num % 2
    if(!acc[key]) acc[key] = []
    acc[key].push(num)
    return acc
}, {})

// Method B: ForEach
const groupB = {}
input.forEach(num => {
    const key = num % 2
    groupB[key] = groupB[key] || []
    groupB[key].push(num)
})
console.log(groupA)
```

#### 25. Second Max Number
*Input: [12, 35, 1, 10, 34, 1] → Output: 34*

```javascript
let input = [12, 35, 1, 10, 34, 1]

// Method A: Sort (Easiest)
const maxA = [...new Set(input)].sort((a,b) => b-a)[1]

// Method B: Single Loop (Efficient - O(N))
const maxB = (arr) => {
    let max = -Infinity, second = -Infinity
    for(let num of arr) {
        if(num > max) {
            second = max
            max = num
        } else if(num > second && num !== max) {
            second = num
        }
    }
    return second
}
console.log(maxB(input))
```

#### 26. Most Occurrence Number
*Input: [1, 3, 2, 1, 4, 1] → Output: 1*

```javascript
let input = [1, 3, 2, 1, 4, 1]

// Method A: Reduce + Sort Values
const freq = input.reduce((acc, c) => (acc[c] = (acc[c]||0)+1, acc), {})
const sorted = Object.entries(freq).sort((a,b) => b[1] - a[1])
console.log(Number(sorted[0][0])) // 1

// Method B: Loop and Track Max
const mostOcc = (arr) => {
    const map = {}
    let maxEl = arr[0], maxCount = 1
    for(let el of arr) {
        map[el] = (map[el] || 0) + 1
        if(map[el] > maxCount) {
            maxEl = el
            maxCount = map[el]
        }
    }
    return maxEl
}
console.log(mostOcc(input))
```

#### 27. Array Anagram
*Input: arr1=['eat','go'], arr2=['tea','go'] → Output: true*

```javascript
let arr1 = ['eat', 'go']
let arr2 = ['tea', 'og'] // Modified 'go' to test logic

// Method A: Map + Sort Key
const isArrAnagram = (a1, a2) => {
    if(a1.length !== a2.length) return false
    const map = new Map()
    a1.forEach(x => map.set(x.split('').sort().join(''), true))
    return a2.every(x => map.has(x.split('').sort().join('')))
}
console.log(isArrAnagram(['eat', 'go'], ['tea', 'go']))
```

#### 28. Count Substring (Regex)
*Input: "GeeksForGeeks" → Output: G=2, eek=2*

```javascript
let str = "GeeksForGeeks"

// Method A: Match with Regex (Global)
const countG = (str.match(/G/g) || []).length
const countEek = (str.match(/eek/g) || []).length

// Method B: Split
const countSplit = (s, sub) => s.split(sub).length - 1

console.log("G:", countG, "eek:", countEek)
```

#### 29. Timer: Print 1 to 10 Interval
*Print 1 to 10 every 1 second, then stop.*

```javascript
// Method A: setInterval with clear
let i = 1
let id = setInterval(() => {
    console.log(i)
    if(i++ === 10) clearInterval(id)
}, 1000)

// Method B: Recursive setTimeout
const printRec = (n) => {
    console.log(n)
    if(n < 10) setTimeout(() => printRec(n+1), 1000)
}
printRec(1)
```
