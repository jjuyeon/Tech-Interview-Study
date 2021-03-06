> [๐ก](#sort) Insertion Sort, Selection Sort, Bubble Sort, Merge Sort, Quick Sort์ ๋ํด ์ค๋ชํ๊ณ  ๊ฐ๊ฐ์ ์ฅ๋จ์ ์ ๋ํด ๋งํด์ฃผ์ธ์

# Sort Algorithm

: ์ ๋ ฌ ์๊ณ ๋ฆฌ์ฆ์๋ ๋ค์ํ ์ข๋ฅ์ ์๊ณ ๋ฆฌ์ฆ์ด ์์ต๋๋ค(Insertion, Selection, Bubble, Merge, Quick, Heap, Count, Radix, Bucket Sort ๋ฑ๋ฑ). ์ด๋ฌํ ์๊ณ ๋ฆฌ์ฆ์ ๊ฐ์ ๊ณ ์ ํ ์ ๋ ฌ ์๊ณ ๋ฆฌ์ฆ, ํน์ฑ์ ๊ฐ์ง๋๋ค.

์ฃผ๋ก Average/Worst Time Complexity, In-Place/Out-Of-Place, Stable/Unstable์ ์ฌ๋ถ์ ๋ฐ๋ผ ๊ฐ๊ฐ์ ์๊ณ ๋ฆฌ์ฆ์ ๋ถ๋ฅํ  ์ ์์ต๋๋ค.

์ด ๋ฌธ์์์๋ ๋ํ์ ์ผ๋ก ๋ง์ด ์ฌ์ฉํ๋ Sorting Algorithm 5๊ฐ์ ๋ํด ๋ค๋ฃจ๊ฒ ์ต๋๋ค.

<br>

## Bubble Sort

    bubble_sort:
      input: arr - ์ ๋ ฌ ํ  ๋ฐฐ์ด
             n - ๋ฐฐ์ด์ ๊ธธ์ด
      output: arr - ์ ๋ ฌ ๋ ๋ฐฐ์ด
      
      for i in [n...1]:
        for j in [1...i]:
          if arr[j]>arr[j+1]:
            swap arr[j] and arr[j+1]
      return arr
    end

<br>

## Insertion Sort

    insertion_sort:
      input: arr - ์ ๋ ฌ ํ  ๋ฐฐ์ด
             n - ๋ฐฐ์ด์ ๊ธธ์ด
      output: arr - ์ ๋ ฌ ๋ ๋ฐฐ์ด

      for i in [2..n]:
        for j in [i-1..1] and arr[j]<arr[j+1]:
          swap arr[j] and arr[j+1]
      return arr
    end

<br>

## Selection Sort

    selection_sort:
      input: arr - ์ ๋ ฌ ํ  ๋ฐฐ์ด
             n - ๋ฐฐ์ด์ ๊ธธ์ด
      output: arr - ์ ๋ ฌ ๋ ๋ฐฐ์ด

      for i in [1..n]:
        minIndex = i
        for j in [i+1..n]:
          if arr[j]<arr[minIndex]:
            minIndex=j
        swap arr[i] and arr[minIndex]
      
      return arr
    end

<br>

## Merge Sort

    merge_sort:
      input: arr - ์ ๋ ฌํ  ๋ฐฐ์ด
             left - ์ ๋ ฌํ  ๊ตฌ๊ฐ์ ์ข์ธก ์ธ๋ฑ์ค
             right - ์ ๋ ฌํ  ๊ตฌ๊ฐ์ ์ฐ์ธก ์ธ๋ฑ์ค

      if left >= right:
        return

      middle=floor((left + right)/2)

      merge_sort(arr, left, middle)
      merge_sort(arr, middle + 1, right)

      merge(leftArray, rightArray)
    end

    merge:
      input: arr - ๋ณํฉํ  ๋ฐฐ์ด
             left - ๋ณํฉํ  ๊ตฌ๊ฐ์ ์ข์ธก ์ธ๋ฑ์ค
             mid - ๋ณํฉํ  ๊ตฌ๊ฐ์ ๊ฐ์ด๋ฐ ๊ธฐ์ค ์ธ๋ฑ์ค
             right - ๋ณํฉํ  ๊ตฌ๊ฐ์ ์ฐ์ธก ์ธ๋ฑ์ค
      
      lptr = mid - left + 1
      rptr = right - mid

      allocate leftArray[1..lptr] and rightArray[1..rptr]

      for i = 1 to lptr:
        leftArray = arr[left - 1 + i]
      for j = 1 to rptr:
        rightArray = arr[mid + j]

      leftArray[lptr + 1] = rightArray[rptr + 1] = INF

      i = 1
      j = 1
      for k = left to right:
        if leftArray[i] <= rightArray[j]:
          arr[k] = leftArray[i]
          i++
        else:
          arr[k] = rightArray[j]
          j++
    end

<br>

## Quick Sort

    function quick_sort(a, left, right)
        if right > left then
            select a pivot value a[pivotIndex]
            pivotNewIndex := partition(a, left, right, pivotIndex)
            quick_sort(a, left, pivotNewIndex-1)
            quick_sort(a, pivotNewIndex+1, right)

      function partition(a, left, right, pivotIndex)
        pivotValue := a[pivotIndex]
        swap(a[pivotIndex], a[right]) // ํผ๋ฒ์ ๋์ผ๋ก ์ฎ๊ฒจ ๋๋๋ค.
        storeIndex := left
        for i from left to right-1
            if a[i] <= pivotValue then
                swap(a[storeIndex], a[i])
                storeIndex := storeIndex + 1
        swap(a[right], a[storeIndex]) // ํผ๋ฒ์ ๋ ๋ฆฌ์คํธ ์ฌ์ด์ ์์น์ํจ๋ค.
        return storeIndex

<br>

## In-Place Sorting Algorithm

์์๋ค์ ๊ฐ์์ ๋นํด ์ถฉ๋ถํ ์ ์ ์(๋ฌด์ํ  ๋งํ)์ ๋ฉ๋ชจ๋ฆฌ๋ฅผ ์ฌ์ฉํ  ๋, ์ด๋ฅผ **In-Place** Algorithm์ด๋ผ ํฉ๋๋ค.

์๋ฅผ ๋ค์ด Insertion Sort์์ ์ ๋ ฌ์ ํ๊ธฐ ์ํด ํ์ํ ์ถ๊ฐ์ ์ธ ๊ณต๊ฐ์ swap์ ํ๊ธฐ ์ํ temp ๋ณ์, ๊ทธ๋ฆฌ๊ณ  loop ๋ณ์ ์ ๋๊ฐ ์์ต๋๋ค. ์ด๋ ์์์ ๊ฐ์์ ์ฐ๊ด์ด ์์ง ์๊ณ , ๋งค์ฐ ์ ์ ์์ ์ฐจ์งํ๊ธฐ ๋๋ฌธ์ ๋ฌด์ํ  ์ ์์ต๋๋ค.

ํ์ง๋ง Merge Sort Algorithm์ ์์์ ์๊ฐํ ๋ค๋ฅธ ์๊ณ ๋ฆฌ์ฆ๋ค๊ณผ ๋ค๋ฅด๊ฒ, ํ๋ ฌ์ divideํ  ๋ ์ถ๊ฐ์ ์ธ ๋ฉ๋ชจ๋ฆฌ๋ฅผ ํ ๋นํด ์ ์ฅํฉ๋๋ค. ์ด๋ ๋จ์ํ ๊ณต๊ฐ์ ๋ ์ฐจ์งํ๋ ๋ฌธ์  ๋ฟ ์๋, ์ฐธ์กฐ ์ง์ญ์ฑ์๋ ์๋ฐฐ๋๊ธฐ ๋๋ฌธ์ ์ฑ๋ฅ์๋ ์ง๊ฒฐ๋ฉ๋๋ค.

<br>

## Stable/Unstable Sort

![image](https://user-images.githubusercontent.com/30489264/132535446-034700d7-035e-48f1-a10c-1bff5dff8e6c.png)

์์๋ค์ ์ ๋ ฌํ  ๋ ๋น๊ต ์ฐ์ฐ์๋ฅผ ์ด์ฉํด 2๊ฐ์ element๋ฅผ ๋น๊ตํ์ฌ ์์๋ฅผ ๋ฐ๊พธ๋ ์๊ณ ๋ฆฌ์ฆ์ **Comparison-based Sorting** ์๊ณ ๋ฆฌ์ฆ์ด๋ผ๊ณ  ํฉ๋๋ค.

์ด ๋, ๋๊ฐ์ priority๋ฅผ ๊ฐ์ง ๋ element๊ฐ ์์ ๋ ๊ธฐ์กด์ ์์๋ฅผ ์ ์งํ๋๋ก ํ๋ ์๊ณ ๋ฆฌ์ฆ์ **Stable Sort** Algorithm, ์ด๋ฅผ ๋ณด์ฅํ์ง ์๋ ์๊ณ ๋ฆฌ์ฆ์ **Unstable Sort** Algorithm์ด๋ผ๊ณ  ํฉ๋๋ค.

์์ ์ฑ์ ๋ช ๊ฐ์ง ์ํฉ์์ ์ค์ํ ์ฑ๋ฅ ์ฒ๋๊ฐ ๋  ์ ์์ต๋๋ค. 

<br>

## Sorting Algorithm ๋น๊ต

||Best Time Complexity|Average Time Complexity|Worst Time Complexity|In-Place|Stable|
|-|-|-|-|-|-|
|Insertion<br>Sort|O(N)|O(Nยฒ)|O(Nยฒ)|O|O|
|Selection<br>Sort|O(Nยฒ)|O(Nยฒ)|O(Nยฒ)|O|X|
|Bubble<br>Sort|O(N)|O(Nยฒ)|O(Nยฒ)|O|O|
|Merge<br>Sort|O(NlogN)|O(NlogN)|O(NlogN)|X|O|
|Quick<br>Sort|O(NlogN)|O(NlogN)|O(Nยฒ)|O|X|
