## 이진 탐색 (binary search)
* 정렬된 배열에서 탐색 범위를 절반씩 좁혀가면서 원소를 찾는 탐색 알고리즘 (log n)
* 중간점에 있는 데이터와 찾고자 하는 데이터를 비교하여 다음 탐색 범위를 결정함

* 재귀 버젼
```
def binary_search(left, right):
  # 종료 조건
  if left > right:
    return -1
   
  mid = (left + right) // 2
  if nums[mid] < target: # 뒷범위 탐색
    return binary_search(mid + 1, right)
  elif nums[mid] > target: # 앞범위 탐색
    return binary_search(left, mid - 1)
  else:
    return mid
```

* 반복 버젼
```
while left <= right:
  mid = (left + right) // 2
  if nums[mid] < target: # 뒷범위 탐색
    left = mid + 1
  elif nums[mid] > target: # 앞범위 탐색
    right = mid - 1
  else:
    return mid
return -1
```

* bisect 모듈
  * 리스트에서 특정 원소의 인덱스를 구하는 경우 -> index()와 bisect 모듈
  * index()는 O(n)이고 bisect은 O(logn)이므로 bisect이 더 빠름
```
from bisect import bisect_left

idx = bisect_left(nums, target)
if idx < len(nums) and nums[idx] == target:
  return idx
return -1
```
