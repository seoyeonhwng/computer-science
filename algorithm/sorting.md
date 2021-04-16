## 퀵정렬
- pivot을 선정하고
- pivot보다 작은, 큰, 같은 값을 각각 저장
- 작은/큰 값을 저장한 리스트에 대해 재귀적으로 퀵정렬 
- pivot에 의해 균형있게 분할되지 못한 경우 O(n^2)가 될 수 있음

```
def quicksort(arr):
  # 종료 조건
  if len(arr) == 1:
      return arr
      
  # pivot 선정 (중앙에 위치한 값으로)
  mid = len(arr) // 2
  pivot = arr[mid]
  
  # pivot을 기준으로 리스트 분할
  small, equal, large = [], [], []
  for a in arr:
      if a < pivot:
          small.append(a)
      elif a > pivot:
          larget.append(a)
      else:
          equal.append(a)
        
  # small, large에 대해 재귀적으로 반복
  small = quicksort(small)
  large = quicksort(large)
  
  return small + equal + large
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
