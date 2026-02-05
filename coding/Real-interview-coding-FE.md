
```js
const obj = [
    { name: "Ramesh", age: 36, home: "ktm" },
    { name: "Sita", age: 36, home: "ktm" },
    { name: "Hari", age: 26, home: "chisapani" },
    { name: "Ram", age: 26, home: "pkr" },
    { name: "Kishor", age: 26, home: "pkr" },
]
```
## Questions: 
### 1. Group above object by home, and sort contnet by age or name (try both age and name sort)
### 2. Find Frequency Input: [1, 1, 2, 3, 3, 4] → Output: { '1': 2, '2': 1, '3': 2, '4': 1 }


```js
// Method 1: Using reduce
const groupByHome = (arr) => {
    return arr.reduce((acc, item) => {
        const home = item.home;
        if (!acc[home]) {
            acc[home] = [];
        }
        acc[home].push(item);
        return acc;
    }, {});
}

// Method 2: Using for...of loop
const groupByHome2 = (arr) => {
    const output = {};
    
    for (const item of arr) {
        const home = item.home;
        
        if (!output[home]) {
            output[home] = [];
        }
        
        output[home].push(item);
    }
    
    return output;
}

// Method 3: Using forEach
const groupByHome3 = (arr) => {
    const output = {};
    
    arr.forEach(item => {
        const home = item.home;
        
        if (!output[home]) {
            output[home] = [];
        }
        
        output[home].push(item);
    });
    
    return output;
}

// Test the function
console.log(groupByHome(obj));
```
