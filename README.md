# assessment

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
