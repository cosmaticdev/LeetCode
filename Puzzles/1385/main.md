# 11/05/2026 - Find the Distance Value Between Two Arrays (Easy)

## First Thoughts
- Seem really easy just using for loops, will just go for a solve immediately by iterating through. Will probably end up being O(n^2) worst-case but whatever. I'll see how it comes out first.


## Attempt One (Solving)
First try got some boring results. It works fine but is slow and uses way too much memory.
```py
class Solution:
    def findTheDistanceValue(self, arr1: list[int], arr2: list[int], d: int) -> int:
        total = 0
        for i in range(len(arr1)):
            for x in range(len(arr2)):
                if abs(arr1[i] - arr2[x]) <= d:
                    break
                if x == len(arr2) - 1:
                    total += 1
        return total 
```
Took 35MS to beat 11.39%, and needed 19.41MB to beat only 17.97%.

## Attempt Two (Optimizing)

Theoretically the loop could be optimized even further by just using a binary search on it. I don't know pythons binary search methods off the top of my head so I will instead just rely on having python sort the list and them implimenting it myself, albeit probably not in the most optimal way.

We also know `|arr1[i]-arr2[j]| <= d` is the equivilant of checking whether `arr2[j]` is within `+-d` from `arr1[i]`. Therefore we can check against this range to see if a number is valid.

After a decent bit of trial and error making stupid mistakes I landed on the following;
```py
class Solution:
    def findTheDistanceValue(self, arr1: list[int], arr2: list[int], d: int) -> int:
        total = 0
        arr2.sort()

        for i in arr1:
            left = 0
            right = len(arr2) - 1
            valid = False

            while left <= right:
                mid = (left + right) // 2
                if abs(i - arr2[mid]) <= d:
                    valid = True
                    break
                else:
                    if arr2[mid] < i:
                        left = mid + 1
                    else:
                        right = mid - 1
            if not valid:
                total += 1
                 
        return total 
```

This is significantly better than the previous approach, taking 7ms to beat 76.53% and 19.25MB to beat 48.3%. This is again within the variation you get between different leetcode runs so I'm satisfied and think that my solution is relatively reasonable. I'm sure I could optimize more by cutting out variables or using built-in python functions to do the search, but oh well.


## Final results:
- Tests: 103/103 passed
- Runtime: 7ms (beats 76.53%)
- Memory: 19.25MB (beats 48.3%)