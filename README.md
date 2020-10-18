### Two Sum
#### Given an array of integers nums and an integer target, return indices of the two numbers such that they add up to target. You may assume that each input would have exactly one solution, and you may not use the same element twice. You can return the answer in any order

```Javascript
var twoSum = function(nums, target) {
    var temp = {};
  for (let i = 0; i < nums.length; i++) {
    var diff = target - nums[i];
    if (diff in temp) {
      return [temp[diff], i];
    }
    temp[nums[i]] = i;
      console.log("Temp", temp)
  }
  return null;
};
```

### Integer to English Words
#### Convert a non-negative integer num to its English words representation
```Javascript
const map = {
  0: 'Zero',
  1: 'One',
  2: 'Two',
  3: 'Three',
  4: 'Four',
  5: 'Five',
  6: 'Six',
  7: 'Seven',
  8: 'Eight',
  9: 'Nine',
  10: 'Ten',
  11: 'Eleven',
  12: 'Twelve',
  13: 'Thirteen',
  14: 'Fourteen',
  15: 'Fifteen',
  16: 'Sixteen',
  17: 'Seventeen',
  18: 'Eighteen',
  19: 'Nineteen',
  20: 'Twenty',
  30: 'Thirty',
  40: 'Forty',
  50: 'Fifty',
  60: 'Sixty',
  70: 'Seventy',
  80: 'Eighty',
  90: 'Ninety',
}


var numberToWords = function(num) {
  if (num === 0) return map[num];
  const groups = ['', 'Thousand', 'Million', 'Billion'];
  let groupsIndex = 0;
  let result = '';
  
  while (num > 0) {
    let currentGroup = num % 1000;
      
    if (currentGroup !== 0) {
        if (result.length > 0) {
            result = parseHundreds(currentGroup) + ' ' + groups[groupsIndex] + ' ' + result;
        } else {
            result = parseHundreds(currentGroup) + ' ' + groups[groupsIndex];
        }
    }
    
    num = parseInt(num/1000);
    groupsIndex++;
  }
  
  return result.trim();
};


function parseHundreds(cashGroup) {
    const HUNDRED = 'Hundred';
    const tensVal = cashGroup % 100;
    const tensString = parseTens(tensVal);
    const hundredsPlace = parseInt(cashGroup / 100);

    if ( hundredsPlace === 0 ) {
        return tensString; // forty-seven 
    }
    let result = map[hundredsPlace] + ' ' + HUNDRED;
    if (tensString) {
        result += ' ' + tensString
    }

    return result; 
}
  
function parseTens(val) {
  if (val === 0) return ''; 

  if (val < 20) {
    return map[val];
  } 
  const tensPlace = parseInt(val /10) * 10;
  
  const onesPlace = val % 10;
  if (onesPlace === 0) {
    return map[tensPlace];
  }
  
  return map[tensPlace] + ' ' + map[val % 10];
}
```

### First Unique Character in a String
#### Given a string, find the first non-repeating character in it and return its index. If it doesn't exist, return -1.
Examples:
s = "leetcode"
return 0.
s = "loveleetcode"
return 2.

```Javascript
/**
 * @param {string} s
 * @return {number}
 */
var firstUniqChar = function(s) {
     for(i=0;i<s.length;i++){
       if (s.indexOf(s[i])===s.lastIndexOf(s[i])){
          return i;
      } 
   }
   return -1;
};
```

### Most Common Word
#### Given a paragraph and a list of banned words, return the most frequent word that is not in the list of banned words.  It is guaranteed there is at least one word that isn't banned, and that the answer is unique. Words in the list of banned words are given in lowercase, and free of punctuation.  Words in the paragraph are not case sensitive.  The answer is in lowercase.

```Javascript
var mostCommonWord = function(paragraph, banned) {
let most;
  let temp = {};
  let words = paragraph.toLowerCase().split(/[ !?',;.]/);
    console.log("Words", words)
  words.forEach(w => {
    if (w && !banned.includes(w)) {
      temp[w] = (temp[w] || 0) + 1;
      if (!most || temp[w] > temp[most]) most = w;
    }
  });
  return most;
};
```

### Reorder Log Files
#### You have an array of logs.  Each log is a space delimited string of words.
#### For each log, the first word in each log is an alphanumeric identifier.  Then, either:
* Each word after the identifier will consist only of lowercase letters, or;
* Each word after the identifier will consist only of digits.
* We will call these two varieties of logs letter-logs and digit-logs.  It is guaranteed that each log has at least one word after its identifier.
#### Reorder the logs so that all of the letter-logs come before any digit-log.  The letter-logs are ordered lexicographically ignoring identifier, with the identifier used in case of ties.  The digit-logs should be put in their original order.
#### Return the final order of the logs.

```Javascript
/**
 * @param {string[]} logs
 * @return {string[]}
 */
var reorderLogFiles = function(logs) {
    const body = s => s.slice(s.indexOf(' ') + 1); // get the body after identifier
  const isNum = c => /\d/.test(c);

  // if body same then compare identifier
  const compare = (a, b) => {
    const n = body(a).localeCompare(body(b));
    if (n !== 0) return n;
    return a.localeCompare(b);
  };

  const digitLogs = [];
  const letterLogs = [];
  for (const log of logs) {
    if (isNum(body(log))) digitLogs.push(log);
    else letterLogs.push(log);
  }
  return [...letterLogs.sort(compare), ...digitLogs];
};
```

### Trapping Rain Water
#### Given n non-negative integers representing an elevation map where the width of each bar is 1, compute how much water it can trap after raining
```Javascript
/**
 * @param {number[]} height
 * @return {number}
 */
var trap = function(height) {
    const size = height.length;
    const leftMax = new Array(size);
    const rightMax = new Array(size);
    let water = 0
    
    leftMax[0] = height[0]
    rightMax[size - 1] = height[size - 1];
    
    // find the height of left wall for each elevation
    for (let i = 1; i < size; i++) {
        leftMax[i] = Math.max(height[i], leftMax[i - 1]);
    }
    // find the height of right wall for each elevation
    for (let i = size - 2; i >= 0; i--) {
        rightMax[i] = Math.max(height[i], rightMax[i + 1]);
    }
    // the height of the water for each elevation would be the 
    // the height of the shorter wal minus the elevation height
    for (let i = 1; i < size - 1; i++) {
        water += Math.min(leftMax[i], rightMax[i]) - height[i]   
    }
    
    return water
};
```

### Copy List with Random Pointer
#### A linked list is given such that each node contains an additional random pointer which could point to any node in the list or null.
Return a deep copy of the list.
The Linked List is represented in the input/output as a list of n nodes. Each node is represented as a pair of [val, random_index] where:
val: an integer representing Node.val
random_index: the index of the node (range from 0 to n-1) where random pointer points to, or null if it does not point to any node.

```Javascript
/**
 * // Definition for a Node.
 * function Node(val, next, random) {
 *    this.val = val;
 *    this.next = next;
 *    this.random = random;
 * };
 */

/**
 * @param {Node} head
 * @return {Node}
 */
var copyRandomList = function(head) {
    if (!head) return null;
    let curr = head;
    while (curr) {
        let copy = new Node(curr.val, curr.next, null);
        copy.next = curr.next;
        curr.next = copy;
        curr = curr.next;
        curr = curr.next;
    }
    
    curr = head;
    while(curr) {
        curr.next.random = curr.random ? curr.random.next : null;
        curr = curr.next.next;
    }
    
    curr = head;
    let result = head.next;
    let resPtr = result;
    while(curr) {
        curr.next = curr.next.next;
        curr = curr.next;
        resPtr.next = resPtr.next ? resPtr.next.next : null;
        resPtr = resPtr.next;
    }
    
    return result;
};
```

### Binary Tree Zigzag Level Order Traversal
#### Given a binary tree, return the zigzag level order traversal of its nodes' values. (ie, from left to right, then right to left for the next level and alternate between).
For example:
Given binary tree [3,9,20,null,null,15,7],

```Javascript
/**
 * Definition for a binary tree node.
 * function TreeNode(val, left, right) {
 *     this.val = (val===undefined ? 0 : val)
 *     this.left = (left===undefined ? null : left)
 *     this.right = (right===undefined ? null : right)
 * }
 */
/**
 * @param {TreeNode} root
 * @return {number[][]}
 */
var zigzagLevelOrder = function(root) {
    if (!root) return [];
    let stackQueue = [root];
    let result = [];
    let level = 1;
    
    while (stackQueue.length > 0) {
        const isLtoR = level % 2 === 1;
        const subLength = stackQueue.length;
        const subList = [];
        for (let i = 0; i < subLength; i++) {
            let node;
            if (isLtoR) {
               node = stackQueue.shift();
                if (node.left) stackQueue.push(node.left);
                if (node.right) stackQueue.push(node.right);
            } else {
                node = stackQueue.pop();                
                if (node.right) stackQueue.unshift(node.right);
                if (node.left) stackQueue.unshift(node.left);
                
            }
            subList.push(node.val);
        }
        level++;
        result.push(subList);
    }
    return result;
};
```

### Number of Islands
#### Given an m x n 2d grid map of '1's (land) and '0's (water), return the number of islands.
#### An island is surrounded by water and is formed by connecting adjacent lands horizontally or vertically. You may assume all four edges of the grid are all surrounded by water.

```Javascript
/**
 * @param {character[][]} grid
 * @return {number}
 */
var numIslands = function(grid) {
    let count = 0
    for (let i = 0; i < grid.length; i++) {
        for (let j = 0; j < grid[i].length; j++) {
            if (grid[i][j] === '1') {
                dfs(grid, i, j)
                count++
            }
        }
    }
    return count
};

const dfs = (grid, x, y) => {
    const row = grid.length, col = grid[x].length
    if (grid[x][y] === '0') return false
    grid[x][y] = '0'
    if (x !== 0) dfs(grid, x - 1, y)
    if (x !== row - 1) dfs(grid, x + 1, y)
    if (y !== 0) dfs(grid, x, y - 1)
    if (y !== col - 1) dfs(grid, x, y + 1)
}
```

### Kth Largest Element in an Array
#### Find the kth largest element in an unsorted array. Note that it is the kth largest element in the sorted order, not the kth distinct element.
Example 1:
Input: [3,2,1,5,6,4] and k = 2
Output: 5

```Javascript
/**
 * @param {number[]} nums
 * @param {number} k
 * @return {number}
 */
var findKthLargest = function(nums, k) {
    let length = nums.length;
    let rs = nums.sort((a,b) => a-b);
    return rs[length-k]
};
```
### K Closest Points to Origin
#### We have a list of points on the plane.  Find the K closest points to the origin (0, 0). (Here, the distance between two points on a plane is the Euclidean distance.) You may return the answer in any order.  The answer is guaranteed to be unique (except for the order that it is in.)
Example 1:
Input: points = [[1,3],[-2,2]], K = 1
Output: [[-2,2]]
Explanation: 
The distance between (1, 3) and the origin is sqrt(10).
The distance between (-2, 2) and the origin is sqrt(8).
Since sqrt(8) < sqrt(10), (-2, 2) is closer to the origin.
We only want the closest K = 1 points from the origin, so the answer is just [[-2,2]].

```Javascript
/**
 * @param {number[][]} points
 * @param {number} K
 * @return {number[][]}
 */
var kClosest = function(points, K) {
     return points.sort((a, b) => a[0]*a[0]+a[1]*a[1] - b[0]*b[0] - b[1]*b[1]).slice(0,K);
};
```

### Longest Palindromic Substring
#### Given a string s, return the longest palindromic substring in s.
Example 1:
Input: s = "babad"
Output: "bab"
Note: "aba" is also a valid answer.

```Javascript
/**
 * @param {string} s
 * @return {string}
 */
var longestPalindrome = function(s) {
    let maxPal = '';
    
    for(let i = 0; i < s.length; i++) {
        bubble(i, i); // odd palindrome
        bubble(i, i+1); // even palindrome
    }
    
    function bubble(left, right) {

        while(left >= 0 && s[left] === s[right]) {
            left--;
            right++;
        }
        left++;
        right--;
        
        if(maxPal.length < right-left+1) {
            maxPal = s.slice(left, right+1)
        }
    }
    return maxPal;
};
```

### LRU Cache
#### Design a data structure that follows the constraints of a Least Recently Used (LRU) cache.

Implement the LRUCache class:

LRUCache(int capacity) Initialize the LRU cache with positive size capacity.
int get(int key) Return the value of the key if the key exists, otherwise return -1.
void put(int key, int value) Update the value of the key if the key exists. Otherwise, add the key-value pair to the cache. If the number of keys exceeds the capacity from this operation, evict the least recently used key.
Follow up:
Could you do get and put in O(1) time complexity?

 

Example 1:

Input
["LRUCache", "put", "put", "get", "put", "get", "put", "get", "get", "get"]
[[2], [1, 1], [2, 2], [1], [3, 3], [2], [4, 4], [1], [3], [4]]
Output
[null, null, null, 1, null, -1, null, -1, 3, 4]

Explanation
LRUCache lRUCache = new LRUCache(2);
lRUCache.put(1, 1); // cache is {1=1}
lRUCache.put(2, 2); // cache is {1=1, 2=2}
lRUCache.get(1);    // return 1
lRUCache.put(3, 3); // LRU key was 2, evicts key 2, cache is {1=1, 3=3}
lRUCache.get(2);    // returns -1 (not found)
lRUCache.put(4, 4); // LRU key was 1, evicts key 1, cache is {4=4, 3=3}
lRUCache.get(1);    // return -1 (not found)
lRUCache.get(3);    // return 3
lRUCache.get(4);    // return 4

```Javascript
/**
 * @param {number} capacity
 */
var LRUCache = function(capacity) {
    this.cache = new Map();
    this.capacity = capacity;
};

/** 
 * @param {number} key
 * @return {number}
 */
LRUCache.prototype.get = function(key) {
    console.log("\nGetting value of ",key);
    if (!this.cache.has(key)) return -1;
    console.log("Mapping is", this.cache);
    const v = this.cache.get(key);
    console.log("V = ", v);
    this.cache.delete(key);
    console.log("deleted the key");
    this.cache.set(key, v);
    console.log("set the key again");
    console.log("Mapping is", this.cache);
    console.log("\n");
    return this.cache.get(key);
};

/** 
 * @param {number} key 
 * @param {number} value
 * @return {void}
 */
LRUCache.prototype.put = function(key, value) {
    console.log("\nSetting the value ",key," and ",value);
    console.log("Mapping is ", this.cache);
    if (this.cache.has(key)) {
      console.log("Cache had the key, so deleting it");
      this.cache.delete(key);
    }
    this.cache.set(key, value);
    console.log("Value is set");
    console.log("Cache size ", this.cache.size );
    console.log("Capacity ", this.capacity )
    if (this.cache.size > this.capacity) {
      console.log("Mapping is ", this.cache);
      console.log("Deleting ", this.cache.keys().next().value);
      this.cache.delete(this.cache.keys().next().value);  // keys().next().value returns first item's key
    }
    console.log("Mapping is ", this.cache);
    console.log("\n");
};
```
