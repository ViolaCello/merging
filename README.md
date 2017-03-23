## How to Merge

### Background

In insertion sort, we saw one approaching for sorting elements.  Now, in this lesson we'll talk again tackle the sorting problem, but in a specific circumstance: we will start with two *ordered arrays* and will need to combine them into one ordered array.  

### Our Problem
Assume that we are given two ordered arrays.  For example, take a look at the arrays below. 

```javascript

  // independently sorted arrays
  let costOfItemsAtTraderJoes =  [1, 2, 6, 7, 8]
  let costOfItemsAtWholeFoods =  [3, 4, 5, 9, 10]
```

Now neither array is filled with sequential numbers, but each array is independently sorted.  So now, assume we are going bargain hunting and want to examine the cheapest items in order.  We'll need an algorithm that combines these elements into one sorted array.  


Note that we can't simply tack one array at the end of the other, nor can we alternate between the arrays to combine them.

```javascript

  // independently sorted arrays
  let costOfItemsAtTraderJoes =  [1, 2, 6, 7, 8]
  let costOfItemsAtWholeFoods =  [3, 4, 5, 9, 10]
  
  // attempt at tacking on 
  let notCombinedByTackOn = [1, 2, 6, 7, 8] + [3, 4, 5, 9, 10]
	// [1, 2, 6, 7, 8, 3, 4, 5, 9, 10]
  
  
  // attempting at alternating
  let costOfItemsAtTraderJoes =  [1, 2, 6, 7, 8]
    let costOfItemsAtWholeFoods =  [3, 4, 5, 9, 10]
  let notCombinedByAlternating = [1, 3, 2, 4, 6, 5, 7, 9, 8, 10]
```

### Solved by Merge

We will call this procedure our merge function.  When it works, as displayed directly below, it should take two sorted arrays and will produce one sorted array consisting of those numbers. 

```javascript
  let costOfItemsAtTraderJoes =  [1, 2, 6, 7, 8]
  let costOfItemsAtWholeFoods =  [3, 4, 5, 9, 10]

merge(firstHalf, secondHalf)
// [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

```

Let's call these beginning arrays, `costOfItemsAtTraderJoes` and `costOfItemsAtWholeFoods` our subarrays, as the will make up our resulting array.  Ok, so given these two subarrays to begin with, how would you get to the final sorted array.  Take a minute and try to think about how you would sort ordered lists of items from two different stores.  You'll know you are thinking properly if your face looks something like this.

![](http://www.funnycatsite.com/pictures/hmmmm.jpg)

Ok, well in thinking of how to merge the two arrays what we could go from the following insight.  The minimum of each subarray has to be at the zero index.  So the minimum of firstHalf is firstHalf[0] and the minimum of secondHalf is secondHalf[0].  Now what is the minimum of the two halves?  Again, give it a shot.

![](https://www.funnypica.com/wp-content/uploads/2015/06/Funny-Pictures-of-Animals-Thinking-8-570x499.jpg)

Well its whichever of these two minimums is smaller.  So, given two sorted arrays, let's find the minimum.  

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

Now, just like with selection sort, to arrive at our sorted array, we just have to keep calling our findMinAndRemove function until there are no elements left - ie. both subarrays are empty.  We said that we call this process of going from sorted subarrays to a combined sorted array our merge function.

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

So let's quickly look at the function above, and focus on the last line of the function.  Our merge function for two given arrays continues to call findMinAndRemove until one of our arrays is empty.  Then once one of the arrays is empty, we know that other array only has the remaining sorted elements.  So we can simply concatenate the remaining elements onto the end of our array.  So in the example above, when the function hits the return statement, `sorted`, `firstHalf` and `secondHalf` will look like the following:

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
Because either first half or second half will always be empty, and the non-empty half will have the remaining ordered elements, we can simply take the ordered elements, along with the remaining elements and concatenate the two together.

### Big O 

So now that we have written a function for merging, what is the big o of merge(firstHalf, secondHalf)

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

Well the worse case scenario is that we go through every element in each array.  Or, to say it a different way, the worse case scenario is that we pass through our while loop once for each element in each array.  We cannot go through the loop more than that, because each time through our while loop we remove an element.  So the cost of merging two subarrays is firstHalf.length + secondHalf.length.

So we can say that the big o of our merge function is the length of our two subarray1 + subarray2.  


### Summary

Merging two sorted arrays into one combined sorted array involves looking at the first element of each array, comparing the two and then placing the smaller element into a new array.  Because we remove an element from a subarray with each comparison, the big o of merging is the combined length of our subarrays.  