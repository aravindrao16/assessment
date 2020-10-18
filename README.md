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
