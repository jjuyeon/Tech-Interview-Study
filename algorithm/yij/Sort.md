> [💡](#sort) Insertion Sort, Selection Sort, Bubble Sort, Merge Sort, Quick Sort에 대해 설명하고 각각의 장단점에 대해 말해주세요

# Sort Algorithm

: 정렬 알고리즘에는 다양한 종류의 알고리즘이 있습니다(Insertion, Selection, Bubble, Merge, Quick, Heap, Count, Radix, Bucket Sort 등등). 이러한 알고리즘은 각자 고유한 정렬 알고리즘, 특성을 가집니다.

주로 Average/Worst Time Complexity, In-Place/Out-Of-Place, Stable/Unstable의 여부에 따라 각각의 알고리즘을 분류할 수 있습니다.

이 문서에서는 대표적으로 많이 사용하는 Sorting Algorithm 5개에 대해 다루겠습니다.

<br>

## Bubble Sort

    bubble_sort:
      input: arr - 정렬 할 배열
             n - 배열의 길이
      output: arr - 정렬 된 배열
      
      for i in [n...1]:
        for j in [1...i]:
          if arr[j]>arr[j+1]:
            swap arr[j] and arr[j+1]
      return arr
    end

<br>

## Insertion Sort

    insertion_sort:
      input: arr - 정렬 할 배열
             n - 배열의 길이
      output: arr - 정렬 된 배열

      for i in [2..n]:
        for j in [i-1..1] and arr[j]<arr[j+1]:
          swap arr[j] and arr[j+1]
      return arr
    end

<br>

## Selection Sort

    selection_sort:
      input: arr - 정렬 할 배열
             n - 배열의 길이
      output: arr - 정렬 된 배열

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
      input: arr - 정렬할 배열
             left - 정렬할 구간의 좌측 인덱스
             right - 정렬할 구간의 우측 인덱스

      if left >= right:
        return

      middle=floor((left + right)/2)

      merge_sort(arr, left, middle)
      merge_sort(arr, middle + 1, right)

      merge(leftArray, rightArray)
    end

    merge:
      input: arr - 병합할 배열
             left - 병합할 구간의 좌측 인덱스
             mid - 병합할 구간의 가운데 기준 인덱스
             right - 병합할 구간의 우측 인덱스
      
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
        swap(a[pivotIndex], a[right]) // 피벗을 끝으로 옮겨 놓는다.
        storeIndex := left
        for i from left to right-1
            if a[i] <= pivotValue then
                swap(a[storeIndex], a[i])
                storeIndex := storeIndex + 1
        swap(a[right], a[storeIndex]) // 피벗을 두 리스트 사이에 위치시킨다.
        return storeIndex

<br>

## In-Place Sorting Algorithm

원소들의 개수에 비해 충분히 적은 양(무시할 만한)의 메모리를 사용할 때, 이를 **In-Place** Algorithm이라 합니다.

예를 들어 Insertion Sort에서 정렬을 하기 위해 필요한 추가적인 공간은 swap을 하기 위한 temp 변수, 그리고 loop 변수 정도가 있습니다. 이는 원소의 개수와 연관이 있지 않고, 매우 적은 양을 차지하기 때문에 무시할 수 있습니다.

하지만 Merge Sort Algorithm은 위에서 소개한 다른 알고리즘들과 다르게, 행렬을 divide할 때 추가적인 메모리를 할당해 저장합니다. 이는 단순히 공간을 더 차지하는 문제 뿐 아닌, 참조 지역성에도 위배되기 때문에 성능에도 직결됩니다.

<br>

## Stable/Unstable Sort

![image](https://user-images.githubusercontent.com/30489264/132535446-034700d7-035e-48f1-a10c-1bff5dff8e6c.png)

원소들을 정렬할 때 비교 연산자를 이용해 2개의 element를 비교하여 순서를 바꾸는 알고리즘을 **Comparison-based Sorting** 알고리즘이라고 합니다.

이 때, 똑같은 priority를 가진 두 element가 있을 때 기존의 순서를 유지하도록 하는 알고리즘을 **Stable Sort** Algorithm, 이를 보장하지 않는 알고리즘을 **Unstable Sort** Algorithm이라고 합니다.

안정성은 몇 가지 상황에서 중요한 성능 척도가 될 수 있습니다. 

<br>

## Sorting Algorithm 비교

||Best Time Complexity|Average Time Complexity|Worst Time Complexity|In-Place|Stable|
|-|-|-|-|-|-|
|Insertion<br>Sort|O(N)|O(N²)|O(N²)|O|O|
|Selection<br>Sort|O(N²)|O(N²)|O(N²)|O|X|
|Bubble<br>Sort|O(N)|O(N²)|O(N²)|O|O|
|Merge<br>Sort|O(NlogN)|O(NlogN)|O(NlogN)|X|O|
|Quick<br>Sort|O(NlogN)|O(NlogN)|O(N²)|O|X|
