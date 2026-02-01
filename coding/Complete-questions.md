### **Easy Questions**
1. **Find frequency:**  
   Input: `[1, 1, 2, 3, 3, 4]` → Output: `{ '1': 2, '2': 1, '3': 2, '4': 1 }`

2. **Remove duplicates:**  
   Input: `[1, 1, 2, 3, 3, 4]` → Output: `[1, 2, 3, 4]`

3. **Intersection of two arrays:**  
   Input: `let arr1 = [5, 6, 7]; let arr2 = [6, 7, 8];` → Output: `[6, 7]`

4. **Find min or max number in an array:**  
   Input: `[1, 1, 2, 3, 3, 4]` → Output: `min = 1, max = 4`

5. **Reverse a string:**  
   Input: `"hello"` → Output: `"olleh"`

6. **First letter of string uppercase:**   
   Input: `"hey whats up"` → Output: `"Hey Whats Up"`

7. **Check if an object is empty:**  
   Input: `{}` → Output: `true`

8. **Find uppercase string index:**  
   Input: `"He llaBellA"` → Output: `[0, 5, 10]`

9. **Rotate an array by K:**  
   Input: `[2, 3, 4, -5, 6, 2, 0, 9], 3` → Output: `[2, 0, 9, 2, 3, 4, -5, 6]`

---

### **Intermediate Questions**
10. **Check anagram or not:**  
    Input: `let str1 = "listen"; let str2 = "silent";` → Output: `true`

11. **Check array anagram:**  
    Input: `const arr1 = ['eat', 'go', 're ad']; const arr2 = ['tea', 'read', 'go'];` → Output: `true`

12. **Find count of substring G or eek repeated in "GeeksForGeeks":**  
    Input: `"GeeksForGeeks"` → Output: `G = 2, eek = 2`

13. **Flatten nested JavaScript array without using `Array.prototype.flat()`:**  
    Input: `[[1, 2, 3], [4, 5, 6], [7, 8, 9], 10, 11]` → Output: `[1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11]`

14. **Merge two objects by id:**  
    Input: `arr1 = [{ id: 1, name: "John" }]; arr2 = [{ id: 1, age: 30 }];` → Output: `[{ id: 1, name: "John", age: 30 }]`

15. **Group elements by divide by 2:**  
    Input: `[10, 15, 20, 25, 30]` → Output: `{ 0: [10, 20, 30], 1: [15, 25] }`

16. **Reverse a number using a while loop:**  
    Input: `12345` → Output: `54321`

17. **Find the longest and count string from sentence:**  
    Input: `"This is our country Nepal "` → Output: Longest string = `"country"`, Count = `7`

---

### **Hard Questions**
18. **Find indices of numbers whose target is 30:**  
    Input: `[10, 20, 30]` → Output: `[0, 1]`

20. **Calculate factorial of a number using recursion:**   
    Input: `4` → Output: `24`

21. **Find maximum difference between two numbers in an array:**  
    Input: `[1, 2, 90, 10, 110]` → Output: `109`

22. **Destructure nested objects/arrays.**

23. **Remove duplicate objects from an array.**

24. **Find intersection of two arrays with objects.**

25. **Reverse number without using reverse():**  
    Input: `12345` → Output: `54321`

26. **Check if a number is Prime:**  
    Input: `17` → Output: `true`

27. **Generate Fibonacci sequence up to N numbers:**  
    Input: `7` → Output: `[ 0, 1, 1, 2, 3, 5, 8]`

28. **Check if a string is a Palindrome:**  
    Input: `"madam"` → Output: `true`

---

### **Object-Based Questions**

1. **Add a property to an object dynamically:**  
   Input: `{ name: 'Riya' }` → Output: `{ name: 'Riya', age: 21 }`

2. **Delete a property from an object:**  
   Input: `{ name: 'Riya', age: 21 }` → Output: `{ name: 'Riya' }`

3. **Use `Object.assign()` to clone an object:**  
   Input: `{ name: 'Riya' }` → Output: `{ name: 'Riya', age: 21 }`

4. **Deep merge of two objects:**  
   Input: `{ id: 12, name: "Anjana", age: 35 }, { id: 12, school: "Kali zone" }` → Output: `{ id: 12, name: "Anjana", age: 35, school: "Kali zone" }`

5. **Combine properties having the same id:**  
   Input: `{id:12, name:"Anjana", age:35}, {id:12, name:"Anjana", school: "Kali zone"}` → Output: `{id:12, name:"Anjana", age:35, school: "Kali zone"}`

6. **Convert array to object:**  
   Input: `[6, 7, 8]` → Output: `{1: "6", 2: "7", 3: "8"}`

```javascript
const employees = [
    { name: 'John', age: 30, city: 'New York', position: 'Developer' },
    { name: 'Emma', age: 25, city: 'Los Angeles', position: 'Analyst' },
    { name: 'Oliver', age: 28, city: 'Chicago', position: 'Architect' },
    { name: 'Sophia', age: 35, city: 'San Francisco', position: 'Manager' },
    { name: 'Liam', age: 27, city: 'Austin', position: 'Developer' },
    { name: 'Ava', age: 32, city: 'Detroit', position: 'Developer' },
];
```

8. **Group employees by position:**  
   Input: Employees array → Output: `{ Developer: [{...}, {...}, {...}], Analyst: [{...}], Architect: [{...}], Manager: [{...}] }`

9. **Sort employees by position and age:**  
   Input: Employees array → Output: Sorted array by position and age.

10. **Filter employees with age greater than 28 and sort them by age:**  
    Input: Employees array → Output: Filtered and sorted array of employees aged > 28.

11. **Find all developers:**  
    Input: Employees array → Output: `[ { name: 'John', ... }, { name: 'Liam', ... }, { name: 'Ava', ... } ]`

12. **Count employees by position:**  
    Input: Employees array → Output: `{ Developer: 3, Analyst: 1, Architect: 1, Manager: 1 }`

13. **Add a new property "salary" to each employee dynamically:**  
    Input: Employees array → Output: Updated array with `salary` property added to each object.

14. **Delete the "city" property from all employees:**  
    Input: Employees array → Output: Updated array without the `city` property.

15. **Find employees whose names start with a specific letter:**  
    Input: Employees array, `"A"` → Output: `[ { name: 'Ava', age: 32, city: 'Detroit', position: 'Developer' } ]`

16. **Calculate the average age of employees:**  
    Input: Employees array → Output: `29.5`

17. **Convert employees array into an object indexed by id:**  
    Input: Employees array → Output: `{ 1: { name: 'John', ... }, 2: { name: 'Emma', ... }, ... }`

18. **Deep clone the employees array:**  
    Input: Employees array → Output: A deep-cloned copy of the array.

19. **Replace the name of employees whose id is 3 to Anjana Kali:**  
    Input: Employees array → Output: Updated array with the name of the employee with `id: 3` replaced.

---

### **Promise-Based Questions**

1. Implement `Promise.all` function and all methods: `Promise.isSettled`, `Promise.any`, and `Promise.race`.
2. Create a promise that resolves after 1 second.
3. **Add Two Promises**  
   Input: `promise1 = Promise.resolve(2), promise2 = Promise.resolve(3)` → Output: `5`
4. **Convert Promise wrapped with settimeout to async/await.**

---

### **LeetCode Problems**

1. **Reverse String**: Write a function that reverses a string.  
   **Input**: `s = "hello"` → **Output**: `"olleh"`

2. **Move Zeroes**: Given an array `nums`, move all `0`s to the end without changing the relative order of the non-zero elements.  
   **Input**: `nums = [0, 1, 0, 3, 12]` → **Output**: `[1, 3, 12, 0, 0]`

3. **Best Time to Buy and Sell Stock**: Given an array `prices`, find the maximum profit from a single buy and sell.  
   **Input**: `prices = [7, 1, 5, 3, 6, 4]` → **Output**: `5`

4. **Contains Duplicate**: Given an integer array `nums`, return `true` if any value appears at least twice in the array, and `false` if every element is distinct.  
   **Input**: `nums = [1, 2, 3, 1]` → **Output**: `true`

5. **Generate Parentheses**: Given `n` pairs of parentheses, write a function to generate all combinations of well-formed parentheses.  
   **Input**: `n = 3` → **Output**: `["((()))","(()())","(())()","()(())","()()()"]`

6. **Sort Colors**: Given an array `nums` with values `0`, `1`, and `2` representing red, white, and blue respectively, sort them in-place.  
   **Input**: `nums = [2, 0, 2, 1, 1, 0]` → **Output**: `[0, 0, 1, 1, 2, 2]`

7. **Add Strings**: Given two non-negative integers `num1` and `num2` represented as strings, return the sum of `num1` and `num2` as a string.  
   **Input**: `num1 = "11", num2 = "123"` → **Output**: `"134"`

8. **Third Maximum Number**: Given an integer array `nums`, return the third distinct maximum number in this array.  
   **Input**: `nums = [3, 2, 1]` → **Output**: `1`

9. **Rotate Array**: Given an integer array `nums`, rotate the array to the right by `k` steps.  
   **Input**: `nums = [1, 2, 3, 4, 5, 6, 7], k = 3` → **Output**: `[5, 6, 7, 1, 2, 3, 4]`

10. **Longest Increasing Subsequence**: Given an integer array `nums`, return the length of the longest strictly increasing subsequence.  
    **Input**: `nums = [10, 9, 2, 5, 3, 7, 101, 18]` → **Output**: `4`

11. **Longest Palindromic Subsequence**: Given a string `s`, find the length of the longest palindromic subsequence in `s`.  
    **Input**: `s = "bbbab"` → **Output**: `4`

12. **Longest Palindrome**: Given a string `s`, return the length of the longest palindrome that can be built with those letters.  
    **Input**: `s = "abccccdd"` → **Output**: `7`

13. **Find the Duplicate Number**: Given an array `nums` containing `n + 1` integers, find the repeated number.  
    **Input**: `nums = [1, 3, 4, 2, 2]` → **Output**: `2`

14. **Validate IP Address**: Given a string `queryIP`, return "IPv4" if it's a valid IPv4 address, "IPv6" if it's a valid IPv6 address, or "Neither" if it's not a valid IP address.  
    **Input**: `queryIP = "172.16.254.1"` → **Output**: `"IPv4"`

15. **Find All Numbers Disappeared in an Array**: Given an array `nums`, find all the elements from 1 to `n` that do not appear in the array.  
    **Input**: `nums = [4,3,2,7,8,2,3,1]` → **Output**: `[5, 6]`

16. **3Sum Closest**: Given an integer array `nums` and an integer `target`, find the sum of three integers in `nums` closest to `target`.  
    **Input**: `nums = [-1, 2, 1, -4], target = 1` → **Output**: `2`

17. **Chunk Array**  
    Input: `arr = [1, 2, 3, 4, 5], size = 2` → Output: `[[1, 2], [3, 4], [5]]`

18. **Differences Between Two Objects**  
    Input: `obj1 = { a: 1, b: 2 }, obj2 = { a: 1, b: 3 }` → Output: `{ b: [2, 3] }`

19. **Timeout Cancellation**  
    Input: `fn = () => console.log("Hello"), t = 1000` → Output: `"Hello"`

20. **Array Upper Bound**  
    Input: `arr = [1, 2, 2, 3], target = 2` → Output: `2`

21. **Create Object from Two Arrays**  
    Input: `keys = ["a", "b"], values = [1, 2]` → Output: `{ a: 1, b: 2 }`

22. **Repeat String**  
    Input: `str = "hello", times = 2` → Output: `"hellohello"`

23. **Inversion of Object**  
    Input: `obj = { a: 1, b: 2 }` → Output: `{ 1: "a", 2: "b" }`

24. **Bind Function to Context**  
    Input: `fn = function() { return this.a; }, context = { a: 1 }` → Output: `1`

25. **Deep Merge of Two Objects**  
    Input: `obj1 = { a: 1 }, obj2 = { b: 2 }` → Output: `{ a: 1, b: 2 }`

26. **Flatten Deeply Nested Array**  
    Input: `arr = [1, [2, [3, [4]]]], depth = 2` → Output: `[1, 2, 3, [4]]`

27. **JSON Deep Equal**  
    Input: `obj1 = { a: 1 }, obj2 = { a: 1 }` → Output: `true`
    
29. **On given array add 1 to the last number and return the result as a list of digits**. Input: lst = `[1, 2, 3]` Output: `[1, 2, 4]` Input: lst = `[9]` Output: `[1, 0]`

---

### **JavaScript Implementation Questions**

1. Pure function and impure function example.
2. Print a counter value every 1 second.
3. Set timeout, Clear timeout example.
4. Implement closure counter: `counter.increment()`, `counter.decrement()`, `counter.reset()`.
5. Implement polyfill for `map` and `filter`.
6. Implement currying.
7. Implement `call`, `apply`, and `bind`.
8. Memoize a function.
10. Implement debounce and throttle to control the rate of function execution.
12. Write the syntax to set and get items in session and local storage.
13. Use JSON to parse and stringify data.
14. Create an arrow function.

---

### **React Implementation**

1. Implement fetch with custom headers. (**url: 'https://randomuser.me/api/'**)
2. HOC (Higher-Order Component).
3. `useRef` (Persist value between renders, focus input field).
4. Form event handling.
5. Redirect to details page.
6. Create a list of items.
7. Implement form validation and submission.
8. Implement todo app functionality.
9. When onclick, navigate to next page ie. using useNavigate, Link

---

### **CSS Implementation**

1. Center a div using CSS and JavaScript.
2. If screen size is 720px set background color yellow, if greater than 720px green, and if less than 480px set color pink.
3. absolute, relative, static.
4. flex, grid, and float.
