Talk about the merge operation, returning a sorted array, whih





```javascript
  let array =  [1, 2, 6, 7, 8, 3, 4, 5]

  function mergeSort(array){
    if(array.length < 2){
      return array
    } else {
      let midpoint = array.length/2
      let firstHalf = array.slice(0, midpoint)
      let secondHalf = array.slice(midpoint, array.length)
      merge(mergeSort(firstHalf), mergeSort(secondHalf))
    }
  }

  mergeSort(array)
  // mergeSort([6, 1, 2, 7, 8, 3, 4, 5])
  // mergeSort([6, 1, 2, 7])      mergeSort([8, 3, 4, 5])
    //                    //
  // mergeSort([6, 1]),  mergeSort([2, 7])
        //           //  
  // mergeSort([6]) mergeSort([1])

  // [6] [1] [2] [7] [8] [3] [4] [5]
// at length one, we hit our base case, and therefore merge sort is just the    array.  
```

So now our array has been split broken down fully.  What's left is to call the merge procedure:
```javascript

  // [6] [1] [2] [7] [8] [3] [4] [5]
  merge([6], [1]) merge([2], [7]), merge([8], [3]) merge([4], [5])

  // Notice that this gives us four sorted arrays of length 2 or
  merge([1, 6], [2, 7]) merge([3, 8], [4, 5])

  // and this gives us two sorted arrays of length four
  merge([1, 2, 6, 7]) merge([3, 4, 5, 8])

  // so finally we merge two sorted arrays
  merge([1, 2, 6, 7], [3, 4, 5, 8])
  // [1, 2, 3, 4, 5, 6, 7, 8]
```

In other words, when you see something like
```javascript

function mergeSort(array){
  if(array.length < 2){
    return array
  } else {
    let midpoint = array.length/2
    let firstHalf = array.slice(0, midpoint)
    let secondHalf = array.slice(midpoint, array.length)
    merge(mergeSort(firstHalf), mergeSort(secondHalf))
  }
}

let array = [6, 1, 2, 7, 8, 3, 4, 5]
mergeSort(array)
  merge(mergeSort([6, 1, 2, 7]), [3, 4, 5, 8])
    merge([6, 1], [2, 7]), merge([3, 4], [5, 8])



```
