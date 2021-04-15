### 다이나믹 프로그래밍
- 큰 문제를 작은 문제로 나눠서 푼다
- 작은 문제로 나누다보면 계산이 중복될 수 있음 -> 이미 계산한 값을 저장하여 중복 계산하지 않음 (메모제이션)

### 피보나치
- top-down 방식
```
dp = collections.defaultdict(int)
def fib(n):
    if n <= 1:
        return n
    if dp[n]:
        return dp[n]
  
    dp[n] = fib(n-1) + fib(n-2)
    return dp[n]
```

- bottom-up 방식
```
def fib(n):
    if n <= 1:
        return n
        
    dp = [0] * (n+1)
    dp[0], dp[1] = 0, 1
    
    for i in range(2, n+1):
        dp[i] = dp[i-1] + dp[i-2]
    return dp[n]
```
