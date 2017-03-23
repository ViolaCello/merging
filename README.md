## How to Merge

### Background

In insertion sort, we saw how we commonly needed to compare two elements to determine which element was smaller.  Now, in this lesson we'll talk again tackle the problem, but in a specific circumstance: when we start with two ordered arrays and need to combine them into one ordered array.  

### Our Problem
Assume that we are given two ordered arrays.  For example, take a look at the arrays below. 

```javascript

  // independently sorted arrays
  let costOfItemsAtTraderJoes =  [1, 2, 6, 7, 8]
  let costOfItemsAtWholeFoods =  [3, 4, 5, 9, 10]
```

Now neither array is filled with sequential numbers, but each array is independently sorted.  So now, if we are going bargain hunting and want to examine the cheapest items in order, we'll need an algorithm that combines these elements into one sorted array.  

### Solved by Merge

We will call this procedure our merge function.  When it works, as displayed directly below, it should take two sorted arrays and in and produce one sorted array consisting of those numbers.

```javascript
  let costOfItemsAtTraderJoes =  [1, 2, 6, 7, 8]
  let costOfItemsAtWholeFoods =  [3, 4, 5, 9, 10]

merge(firstHalf, secondHalf)
// [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

```

Ok, so given these two arrays to begin with, how would you get to the final sorted array.

![](http://www.funnycatsite.com/pictures/hmmmm.jpg)

Ok, well what we could go from the following insight.  The minimum of each subarray has to be at the zero index.  So the minimum of firstHalf is firstHalf[0] and the minimum of secondHalf is secondHalf[0].  Now what is the minimum of the two halves, well its whichever of these two minimums is smaller.  So, given two sorted subarrays, let's find the minimum.  

```javascript
let firstHalf =  [1, 2, 6, 7, 8]
let secondHalf =  [3, 4, 5, 9, 10]

function findMin(firstHalf, secondHalf){
  let minfirstHalf = firstHalf[0]
  let minsecondHalf = secondHalf[0]

  if(minfirstHalf < minsecondHalf){
    return minfirstHalf
  } else {
    return minsecondHalf
  }
}

findMin(firstHalf, secondHalf)
// 1
```

And thinking about how this turns into a sorted array, upon finding the minimum, we can remove the element and push it into another array.  So we are back to writing a findMinAndRemove function, except this time it is for two sorted subarrays.

```javascript

let firstHalf =  [1, 2, 6, 7, 8]
let secondHalf =  [3, 4, 5, 9, 10]

function findMinAndRemove(firstHalf, secondHalf){
  let minfirstHalf = firstHalf[0]
  let minsecondHalf = secondHalf[0]

  if(minfirstHalf < minsecondHalf){
    return firstHalf.shift()
  } else {
    return secondHalf.shift()
  }
}

findMinAndRemove(firstHalf, secondHalf)
// 1

firstHalf
// [2, 6, 7, 8]
```

Now, just like with selection sort, to arrive at our sorted array, we just have to keep calling our findMinAndRemove function until there are no elements left - ie. both subarrays are empty.  We said that we call this process of going from sorted subarrays to a sorted array our merge function.

```javascript
  let firstHalf =  [1, 2, 6, 7, 8]
  let secondHalf =  [3, 4, 5, 9, 10]

  function merge(firstHalf, secondHalf){
    let sorted = []
    let currentMin;
    while(firstHalf.length != 0 && secondHalf.length != 0){
      let currentMin = findMinAndRemove(firstHalf, secondHalf)
      sorted.push(currentMin)
    }
    return sorted.concat(firstHalf).concat(secondHalf)
  }
```

So let's quickly look at the function above.  Our merge function for given two subarrays continues to call findMinAndRemove until one of our arrays is empty.  Then once one of the arrays is empty, we know that other array only has the remaining sorted elements.  So we can simply concatenate the remaining elements onto the end of our array.  So in the example above, when the function hits the return statement, sorted array, firstHalf and secondHalf will look like the following:

```javascript
  sorted
    // [1, 2, 3, 4, 5, 6, 7, 8]
  firstHalf
    // []
  secondHalf
    // [9, 10]
  sorted.concat(firstHalf).concat(secondHalf)
  // [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
```

And what is the big o of merge(firstHalf, secondHalf)

```javascript
function merge(firstHalf, secondHalf){
  let sorted = []
  let currentMin;
  while(firstHalf.length != 0 && secondHalf.length != 0){
    let currentMin = findMinAndRemove(firstHalf, secondHalf)
    sorted.push(currentMin)
  }
  return sorted.concat(firstHalf).concat(secondHalf)
}

```

Well the worse case scenario is that we go through the cost of every element in each array.  Or, to say it a different way, the worse case scenario is that we pass through our while loop once for each element in each array.  We cannot go through the loop more than that, because each time through our while loop we remove an element.  So the cost of merging two subarrays is firstHalf.length + secondHalf.length or simply the original array's length, or n.

So we can say that the big o of merge sort is the total cost of splitting plus the total cost of merging or log n + n log n.  Now in big o we only consider the largest exponent, and because n * log n is larger than log n the big o is n log n.

### So what?

Remember we started out the lesson with trying to improve upon selection sort.  Remember that selection sort cost us n^2.  Well in this example of sorting an array of eight elements n log n = 24 while n^2 = 64.  If n = 1000 then n^2 = 1,000,000 while n log n = 9,965.  So when is one thousand selection sort takes over a hundred times longer than merge sort.  That's significant.

Going forward, you can assume that the cost of sorting an array is n log n.  And now you know the steps involved in sorting an array.

### Summary

Merge sort involves two large components, recursively splitting up the array into smaller and smaller subarrays until the size of the array is one or smaller, and the merge step of looking at the first element of each subarray to find the min of both of them.  Mergesort can be expressed with the following pseudocode:

```javascript
  function mergeSort(array){
    if(array.length < 2){
      return array;
    } else {
      merge(mergeSort(firstHalfArray), mergeSort(secondHalfArray))
    }
  }
```

Using our old recursive trick of looking of defining a solution in terms of the name of the function we see that a merge-sorted array is the same as merging two subarrays which are both merge-sorted.
