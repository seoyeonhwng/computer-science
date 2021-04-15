## 퀵정렬
- pivot을 선정하고
- pivot 앞에는 pivot보다 작은 값을, pivot 뒤에는 pivot보다 큰 값이 오도록 두 개의 리스트로 분할
- 분할된 리스트에 대해 재귀적으로 반복
- pivot에 의해 균형있게 분할되지 못한 경우 O(n^2)가 될 수 있음

```
def quicksort(A, lo, hi):
  def partition(lo, hi):
    pivot = A[hi]
    left = lo
    for right in range(lo, hi):
      if A[right] < pivot:
        A[right], A[left] = A[left], A[right]
        left += 1
    
    A[left], A[hi] = A[hi], A[left]
    return left
       
  if lo < hi:
    # 두 개의 리스트로 분할
    pivot = partition(lo, hi)
    # 각 리스트에 대해서 재귀 호출
    quicksort(A, lo, pivot-1)
    quicksort(A, pivot+1, hi)
```

## 병합정렬
- 배열을 더이상 나눌 수 없을때까지 반으로 분할
- 두 배열씩 값을 비교하여 정렬하며 합침
- 항상 O(nlogn)을 보장 + 안정 정렬
```
def merge_sort(arr):
  # 종료 조건
  if len(arr) <= 1:
      return arr
  
  # 배열을 절반으로 분할한 뒤 재귀적으로 호출
  mid = len(arr) // 2
  left = merge_sort(arr[:mid])
  right = merge_sort(arr[mid:])
  
  # 두 배열을 합친 결과를 반환
  return merge(left, right)
  
def merge(left, right):
    result = []
    left, right = deque(left), deque(right)
    
    while left and right:
        if left[0] < right[0]:
            result.append(left.popleft())
        else:
            result.append(right.popleft())
            
    while left:
        result.append(left.popleft())
    while right:
        result.append(right.popleft())
    return result
```
