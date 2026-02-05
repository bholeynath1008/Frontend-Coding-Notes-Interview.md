# Simple Promise Examples with setTimeout
### Convert to resolve Chaining to async/ await
### Best pratices: Always use try, catch, finally block
## **The Promise:**
```javascript
// Simple promise that resolves or rejects after 2 seconds
const simplePromise = new Promise((resolve, reject) => {
    setTimeout(() => {
        const success = true; // Change to false to see rejection
        
        if (success) {
            resolve("Success! Operation completed.");
        } else {
            reject("Failed! Something went wrong.");
        }
    }, 2000); // 2 second delay
});
```

---

## **Method 1: Using Promise Chaining (.then/.catch)**

### **Success Case:**
```javascript
console.log("Starting operation with chaining...");

simplePromise
    .then(result => {
        console.log("Result:", result);
    })
    .catch(error => {
        console.log("Error:", error);
    })
    .finally(() => {
        console.log("Operation finished (success or fail)");
    });

// Output after 2 seconds:
// "Result: Success! Operation completed."
// "Operation finished (success or fail)"
```

### **Failure Case (change success = false):**
```javascript
console.log("Starting operation with chaining...");

simplePromise
    .then(result => {
        console.log("Result:", result);
    })
    .catch(error => {
        console.log("Error:", error);
    });

// Output after 2 seconds:
// "Error: Failed! Something went wrong."
```

---

## **Method 2: Using async/await**

### **Success Case:**
```javascript
async function handlePromise() {
    console.log("Starting operation with async/await...");
    
    try {
        const result = await simplePromise;
        console.log("Result:", result);
    } catch (error) {
        console.log("Error:", error);
    } finally {
        console.log("Operation finished");
    }
}

handlePromise();

// Output after 2 seconds:
// "Result: Success! Operation completed."
// "Operation finished"
```

### **Failure Case (change success = false):**
```javascript
async function handlePromise() {
    console.log("Starting operation...");
    
    try {
        const result = await simplePromise;
        console.log("Result:", result);
    } catch (error) {
        console.log("Error:", error);
    }
}

handlePromise();

// Output after 2 seconds:
// "Error: Failed! Something went wrong."
```

---

## **Even Simpler Examples:**

### **Example 1: Always Success**
```javascript
const happyPromise = new Promise(resolve => {
    setTimeout(() => resolve("😊 All good!"), 1000);
});

// Chaining
happyPromise.then(msg => console.log(msg));

// Async/Await
async function run() {
    const msg = await happyPromise;
    console.log(msg);
}
run();
```

### **Example 2: Always Failure**
```javascript
const sadPromise = new Promise((resolve, reject) => {
    setTimeout(() => reject("😢 Something broke"), 1000);
});

// Chaining
sadPromise
    .then(msg => console.log(msg))
    .catch(err => console.log(err)); // This runs

// Async/Await  
async function run() {
    try {
        const msg = await sadPromise;
        console.log(msg);
    } catch (err) {
        console.log(err); // This runs
    }
}
run();
```

### **Example 3: 50/50 Chance**
```javascript
const randomPromise = new Promise((resolve, reject) => {
    setTimeout(() => {
        const random = Math.random(); // 0 to 1
        if (random > 0.5) {
            resolve("🎉 You won! " + random);
        } else {
            reject("💔 You lost! " + random);
        }
    }, 1000);
});

// Try multiple times to see both outcomes
randomPromise
    .then(msg => console.log("Success:", msg))
    .catch(err => console.log("Failure:", err));
```

---

## **Quick Comparison:**

| Method | Success Case | Error Case |
|--------|--------------|------------|
| **Chaining** | `.then()` runs | `.catch()` runs |
| **Async/Await** | `try` block runs | `catch` block runs |

Both do the same thing, just different syntax!
