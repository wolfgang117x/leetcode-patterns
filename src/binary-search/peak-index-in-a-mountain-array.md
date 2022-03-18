# [Peak Index in a Mountain Array](https://leetcode.com/problems/peak-index-in-a-mountain-array/)

**Difficulty: Easy**

---

## Description

Let's call an array `arr` a mountain if the following properties hold:

* `arr.length >= 3`
* There exists some `i` with `0 < i < arr.length - 1` such that:
  * `arr[0] < arr[1] < ... arr[i-1] < arr[i]`
  * `arr[i] > arr[i+1] > ... > arr[arr.length - 1]`

Given an integer array `arr` that is guaranteed to be a mountain, return any `i` such that `arr[0] < arr[1] < ... arr[i - 1] < arr[i] > arr[i + 1] > ... > arr[arr.length - 1]`.

### Example 1:

```
Input: arr = [0,1,0]
Output: 1
```

### Example 2:
```
Input: arr = [0,2,1,0]
Output: 1
```

### Example 3:
```
Input: arr = [0,10,5,2]
Output: 1
``` 

### Constraints:

* `3 <= arr.length <= 104`
* `0 <= arr[i] <= 106`
* `arr` is guaranteed to be a mountain array.

---

## Solution

```java
// O(log n) Time, O(1) Space
class Solution {
    public int peakIndexInMountainArray(int[] arr) {
        int left = 0, right = arr.length - 1, peakIdx = -1;
        while (left <= right) {
            int mid = left + (right-left)/2;
            if (mid-1 > -1 && arr[mid] > arr[mid+1]) {
                peakIdx = mid;
                right = mid - 1;
            } else {
                left = mid + 1;
            }
        }
        return peakIdx;
    }
}
```

---

## Explanation

As per the constraints on the array provided, we know that there will always be a peak. A peak here can be defined as an index `i` which has values lesser values than itself to its left and right.  

If we find an increasing sequence of numbers, we know that the peak is to the right & similarly if we find a decreasing sequence, we know that the peak is to the left as `i` has values lesser than itself on both its sides.  

Using binary search we can narrow down the search space. The comparision condition is going to check if the mid element is greater than its next element aka have we encountered a decreasing sequence, if yes we move towards left else we move towards right.

```java
int left = 0, right = arr.length - 1, peakIdx = -1;
```
* First we initialize the bounds for binary search, the `left` bound to index `0` and `right` bound to end of the array `arr.length - 1`.
* We initialize `peakIdx` as -1, an invalid value, until we find the actual peak index. This variable will be used to keep track of left most index which is greater than its next value. The left most index which is greater than its next value is the peak element in the array because the peak element must be greater than the elements to its left.

```java
while (left <= right) {
    int mid = left + (right-left)/2;
```
* Now we begin the while loop for the binary search with condition being the lower bound variable `left` is less than or equal to higher bound variable `right`. 
* Next, we calculate the `mid` value using `left + (right-left)/2` to avoid overflow and ensure its within array bounds.

```java
if (arr[mid] > arr[mid+1]) {
    peakIdx = mid;
    right = mid - 1;
}
```
* Once we have the `mid`, we compare the `mid` element and `mid+1` element.
* If the `mid` element is greater than `mid+1`, we have encountered a decreasing sequence meaning the peak element is to the left and we have to move our search space to the left.
* We do this by setting `right = mid - 1`, also we set `peakIdx = mid` as its the left most value with decreasing elements to the right we've encountered so far. 

```java
} else {
    left = mid + 1;
}
```
* If `mid` is not greater than `mid+1`, we know that we've encountered an increasing sequence meaning the peak element is to the right and we have to move our search space to the right.
* We do this by setting `left = mid + 1`.

```java
return peakIdx;
```
* The while loop exits when `left <= right` meaning we've narrowed down and exhausted the search space.
* We then return `peakIdx` which points to the peak element in the array.

This approach takes O(log n) time as we halve the search space with each iteration, and it takes O(1) space as we have used only variables which take constant space.