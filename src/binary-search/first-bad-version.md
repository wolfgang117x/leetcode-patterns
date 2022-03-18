# [First Bad Version](https://leetcode.com/problems/first-bad-version/)

**Difficulty: Easy**

---

## Description

You are a product manager and currently leading a team to develop a new product. Unfortunately, the latest version of your product fails the quality check. Since each version is developed based on the previous version, all the versions after a bad version are also bad.

Suppose you have `n` versions `[1, 2, ..., n]` and you want to find out the first bad one, which causes all the following ones to be bad.

You are given an API bool `isBadVersion(version)` which returns whether version is bad. Implement a function to find the first bad version. You should minimize the number of calls to the API.

### Example 1:

```
Input: n = 5, bad = 4
Output: 4
Explanation:
call isBadVersion(3) -> false
call isBadVersion(5) -> true
call isBadVersion(4) -> true
Then 4 is the first bad version.
```

### Example 2:

```
Input: n = 1, bad = 1
Output: 1
```

### Constraints:

* `1 <= bad <= n <= 231 - 1`

---

## Solution

```java
// O(log n) Time, O(1) Space
public class Solution extends VersionControl {
    public int firstBadVersion(int n) {
        int low = 1, high = n, versionIdx = 0;
        while (low <= high) {
            int mid = low + (high-low)/2;
            if (isBadVersion(mid)) {
                versionIdx = mid;
                high = mid - 1;
            } else {
                low = mid + 1;
            }
        }
        return versionIdx;
    }
}
```

---

## Explanation

The problem is asking us to find the first bad version of a product out of `n` versions with minimal calls to the `isBadVersion` api. This can be generalized as find the minimum value in sorted array that meets specific a condition, the condition being the value is a bad version.

### Linear Approach

We can solve this problem with a linear approach.
```
- Iterate through the array from oldest to newest version => i: 0 -> n
  - isBadVersion(i)
    - return i
```

This approach has a time complexity of O(n) as the latest / last version can be first bad version and it will take N iterations. Also this approach does not minimize api calls as it makes N api calls.

### Binary Search

We can take advantage of the fact that the array is sorted i.e each new version is greater than the previous.  
Using binary search we can reduce the search space with each iteration, this also minimizes the number of api calls.

```java
int low = 1, high = n, versionIdx = 0;
```
* First we initialize the bounds of binary search, we set the lower bound to `1` and higher bound as `n` as we know from the description the versions range from [1,2,3...n].
* We also initialize a variable called `versionIdx` aka version index, this will help us keep track of the minimum bad version as we're iterating.

```java
while(low<=high) {
    int mid = low + (high-low)/2;
```
* Now we begin the while loop for the binary search with condition being the lower bound variable `low` is less than or equal to higher bound variable `high`. 
* Next, we calculate the `mid` value using `low + (high-low)/2` to avoid overflow and ensure its within array bounds.

```java
if (isBadVersion(mid)) {
    versionIdx = mid;
    high = mid-1;
}
```
* We check if the `mid` version is a bad version.
* If yes, we know that it is either the first bad version or there are bad versions before it.
* So, we update the `versionIdx` value to `mid`.
* We reduce the search space by setting `high = mid-1`, we're setting it to `mid-1` as we already know mid is a bad version. In the next iteration we'll only be search versions older than `mid`.

```java
} else {
    low = mid+1;
}
```
* If `mid` isn't a bad version, we know the first bad version is a newer version than `mid`.
* So, we reduce the search space part by setting `low = mid+1` and search versions newer than `mid`.

```java
return versionIdx;
```
* The while loop exits when `low <= high` meaning we've narrowed down and exhausted the search space.
* We then return `versionIdx` which contains the minimum or first bad version.

This approach takes O(log n) time as we halve the search space with each iteration, and it takes O(1) space as we have used only variables which take constant space.
