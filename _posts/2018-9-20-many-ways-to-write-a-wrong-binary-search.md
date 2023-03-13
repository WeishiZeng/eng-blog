---
layout: post
title: Many Ways to Write a Wrong Binary Search Algorithm
date: '2018-09-21T15:43:02.002-07:00'
author: Weishi Zeng
tags: algorithm
---

# Binary Search Algorithm

## Summary

A good practice to go through all possible scenarios of a binary search and learn what can go wrong.

Estimated time for practice: 2h

### Variants
* Variant 1:
looking for target itself
* Variant 2:
looking for target range
* Variant 3:
looking for elements closest to target.


### Strategy
1. Open a range [left, right]
2. Each loop it narrows down the range.
3. The answer, if exists, will not escape the range.
4. Examine the elements in the range.

### Template
1. Open a full range [left, right]
2. Move left/right onto mid based on comparison
3. Exit the loop when they're at farthest next to each other
3. The answer, if exists, sits in [left, right]
4. Examine the elements in the range.


### Side note:
> Integer Division:    
> Java rounds towards 0,   
> Python rounds towards negative infinity.

## Problem A: Search a single target

Given a sorted array, search a single target.  

> Given [-1,0,3,5,9,12] and target value 9,
return 4.


### Implementation #1: Naive :x:
1. Narrow down scope by moving left/right onto mid.  
2. Exit loop when left **crosses** right.  
3. Return mid.

```java
public int search(int[] nums, int target) {
    int left = 0;
    int right = nums.length - 1;

    while (left < right) {
        int mid = (left + right) / 2;
        if (nums[mid] < target) {
            left = mid;
        } else if (nums[mid] > target) {
            right = mid;
        } else {
            return mid;
        }
    }

    return -1;
}  
```

#### What's wrong:
1. Array of one.
  * missing target. E.g. [5], 5
2. Array of two
  * infinite loop. E.g. [1,2], 2

### Test Cases
Any array of 3 or more will reduce to problem of array of 1 or 2. So we can draw a table below to cover all test cases.

| # of Elements | Has Target? | Is target on the left? |
| :-------------: |:-------------:| :-----:|
| 1 | Y | - |
| 1 | N | - |
| 2 | Y | Y |
| 2 | Y | N |
| 2 | N | - |


### Implementation #2: Move index aggressively :x:
1. Narrow down scope by moving left/right past mid.
2. Exit loop when left **crosses** right.  
3. Check if mid is target and return accordingly.

```java
public int search(int[] nums, int target) {
    int left = 0;
    int right = nums.length - 1;
    int mid = (left + right) / 2;

    while (left < right) {
        mid = (left + right) / 2;
        if (nums[mid] < target) {
            left = mid + 1;
        } else if (nums[mid] > target) {
            right = mid - 1;
        } else {
            return mid;
        }
    }

    return nums[mid] == target ? mid : -1;
}
```

#### What's wrong:

| # of Elements | Has Target? | Is target on the left? | Example | Working?|
| :---: | :---: | :---: | :---: | :---: |
| 2 | Y | N | [2,5], 5 | :x: |

1. Exit while loop when left = right. But mid is not updated to (left + right) / 2.


### Implementation #3: Return left instead of mid
1. Narrow down scope by moving left/right past mid.
2. Exit loop when left **crosses** right.  
3. Check if left is target and return accordingly.

```java
public int search(int[] nums, int target) {
    int left = 0;
    int right = nums.length - 1;

    while (left < right) {
        int mid = (left + right) / 2;
        if (nums[mid] < target) {
            left = mid + 1;
        } else if (nums[mid] > target) {
            right = mid - 1;
        } else {
            return mid;
        }
    }
    /*
     * Post Condition:
     * 0 <= left <= mid + 1 <= nums.length - 1
     */  
    return nums[left] == target ? left : -1;
}
```

```java
public int search(int[] nums, int target) {
    int left = 0;
    int right = nums.length - 1;
    int mid = (left + right) / 2;

    while (left < right) {
        if (nums[mid] < target) {
            left = mid + 1;
        } else if (nums[mid] > target) {
            right = mid - 1;
        } else {
            return mid;
        }

        //java is rounding towards 0.
        //(-1 + 0) / 2 = 0
        mid = (left + right) / 2;
    }

    return nums[mid] == target ? mid : -1;
}
```

### Implementation #4: A standardized template.
1. Narrow down scope by moving left/right onto mid.
2. Exit loop when left **is next to** right.
3. Check both left and right, return accordingly.

```java
public int search(int[] nums, int target) {
    int left = 0;
    int right = nums.length - 1;

    while (left + 1 < right) {
        int mid = left + (right - left) / 2;

        if (nums[mid] < target) {
            left = mid;
        } else if (nums[mid] > target) {
            right = mid;
        } else {
            return mid;
        }

        /*
         * Post Condition:
         * if has target, it's in the range of (left, right), exclusive.
         */
    }

    /*
     * Post Condition:
     * 1. left and right is either
     * a) the same, or b) next to each other
     *
     * 2. if a), then we only have 1 element.
     *    if b), then target is in range (left, right), which doesn't exist.
     *
     */

    if (nums[left] == target) {
        return left;
    } else if (nums[right] == target) {
        return right;
    } else {
        return -1;
    }
}
```

## Problem B-1: Find first target element.
We could extend problem A into the following:
> Find first/last element index.

| # of Elements | Has Target? | Is target on the left? | Example |
| --- | :---: | :---: | :---: |
| 2 | Y | N | [2,5], 5 |
| 2 | Y | Y | [2,5], 2 |
| 2 | N | Y | [2,5], 1 |
| 2 | N | N | [2,5], 6 |

### Implementation #5: Figure out exact indices
1. Narrow down scope by  
  a) If mid is not equal to target: moving left past mid, or moving right past mid.  
  b) If mid equals target, move right onto mid.  
2. Exit loop when left **crosses** right.  
3. Check if left is target and return accordingly.

```java
public int search(int[] nums, int target) {

    int left = 0;
    int right = nums.length -1;

    while (left < right) {
        int mid = left + (right - left) / 2;
        /*
         * Post Condition:
         * 1. mid is never equal to right
         */
        if (nums[mid] < target) {
          left = mid + 1;
        } else if ( target < nums[mid]) {
            right = mid - 1;
        } else { //equal
            right = mid; //since right != mid, we are effectively changing right in every loop
        }
        /*
         * Post Condition:
         * 1. If has target, first target is in range [left, right]
         */
    }
    return nums[left] == target ? left : -1;
}
```

### Implementation #6: Adapt from template
1. Narrow down scope by moving left/right onto mid.
2. Exit loop when left ***s next to*** right.
3. Check both left and right, return accordingly.

```java
public int search(int[] nums, int target) {
    int left = 0;
    int right = nums.length - 1;

    while (left + 1 < right) {
        int mid = left + (right - left) / 2;

        if (nums[mid] < target) {
            left = mid;
        } else if (nums[mid] > target) {
            right = mid;
        } else { //=
            right = mid;
        }

        /*
         * Post Condition:
         * if has target, it's in the range of (left, right]
         */
    }

    /*
     * Post Condition:
     * 1. left and right is either
     * a) the same, or
     * b) next to each other
     *
     * 2. in case of b, if has target, then first target is in range (left, right]
     *
     */

    //this takes care of case a automatically.
    if (nums[left] == target) {
        return left;
    } else if (nums[right] == target) {
        return right;
    } else {
        return -1;
    }
}
```

## Problem B-2: Find last target element
### Implementation #7: Figure out exact indices
1. Narrow down scope by  
  a) moving left past mid, or  
  b) moving right onto mid.
2. Exit loop when left **crosses** right.  
3. Check if left is target and return accordingly.

```java
public int search(int[] nums, int target) {

    int left = 0;
    int right = nums.length -1;

    while (left < right) {
        int mid = (left + right) / 2;
        //configure mid to be on the right
        if (mid * 2 != left + right) {
            mid++;
        }
        /*
         * Post Condition:
         * 1. mid is never equal to left
         */
        if (nums[mid] < target) {
            left = mid + 1;
        } else if ( target < nums[mid]) {
            right = mid - 1;
        } else { //equal
            left = mid; //since never equals to left, this always narrows down the scope.
        }
        /*
         * Post Condition:
         * 1. If has target, last target is in range [left, right]
         *
         */
    }
    //
    return nums[right] == target ? right : -1;
}
```

### Implementation #8: Adapt from template
1. Narrow down scope by moving left/right onto mid.
2. Exit loop when left ***is next to*** right.
3. Check both left and right, return accordingly.
```java
public int search(int[] nums, int target) {
    int left = 0;
    int right = nums.length - 1;

    while (left + 1 < right) {
        int mid = left + (right - left) / 2;

        if (nums[mid] < target) {
            left = mid;
        } else if (nums[mid] > target) {
            right = mid;
        } else { //=
            left = mid;
        }

        /*
         * Post Condition:
         * if has target, it's in the range of [left, right)
         */
    }

    /*
     * Post Condition:
     * 1. left and right is either
     * a) the same, or
     * b) next to each other
     *
     * 2. In case of b, if has target, then last target is in range [left, right)
     *
     */

    //this takes care of case a automatically.
    if (nums[right] == target) {
        return right;
    } else if (nums[left] == target) {
        return left;
    } else {
        return -1;
    }
}
```

## Problem B: Search for a range

Given a sorted array of n integers, find the starting and ending position of a given target value.

If the target is not found in the array, return [-1, -1].

> Given [5, 7, 7, 8, 8, 10] and target value 8,
return [3, 4].

This can be solved by applying B-1 and B-2.

## Problem C: Find first element bigger than target.

> Given [-1,0,3,5,9,12] and target value 3 or 4,
return 5.

### Implementation #9: Figure out exact indices
1. Narrow down scope by  
  a) moving left past mid, or  
  b) moving right onto mid.
2. Exit loop when left **crosses** right.  
3. Check if left is target and return accordingly.

```java
public int search(int[] nums, int target) {
    int left = 0;
    int right = nums.length - 1;

    while (left < right) {
        int mid = left + (right - left) / 2;
        if (nums[mid] < target) {
            left = mid + 1;
        } else if (target < nums[mid]) {
            right = mid;
        } else { //=
            left = mid + 1;
        }
    }
    //if has result, it is in [left, right]
    if (nums[left] > target) {
        return left;
    } else {
        if (left + 1 >= nums.length) {
            return -1;
        } else {
            return left + 1;
        }
    }
}
```

### Implementation #10: Adapt from template
1. Narrow down scope by moving left/right onto mid.
2. Exit loop when left ***is next to*** right.
3. Check both left and right, return accordingly.
```java
public int search(int[] nums, int target) {
    int left = 0;
    int right = nums.length - 1;

    while (left + 1 < right) {
        int mid = left + (right - left) / 2;

        if (nums[mid] < target) {
            left = mid;
        } else if (nums[mid] > target) {
            right = mid;
        } else { //=
            left = mid;
        }

        /*
         * Post Condition:
         * if has result index, it's in the range of (left, right]
         */
    }

    /*
     * Post Condition:
     * 1. if array length is > 1,
     *    1. then left and right is next to each other
     *    2. if has result index, then it is in range (left, right]
     *
     */

    /* Three possible path:
     * a. 1 element array -> left = right
     * b. 2 elements array without result
     * c. 2 elements array with result
     */
    if (nums[left] > target) {
        return left;
    } else if (nums[right] > target) {
        return right;
    } else {
        return -1;
    }
}
```
