## 퀵정렬
* pivot을 선정하고
* pivot 앞에는 pivot보다 작은 값을, pivot 뒤에는 pivot보다 큰 값이 오도록 두 개의 리스트로 분할
* 분할된 리스트에 대해 재귀적으로 반복

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
