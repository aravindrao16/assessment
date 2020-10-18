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
