# 패턴 매칭

## 고지식한 패턴 검색 알고리즘

- 본문 문자열을 처음부터 끝까지 차례대로 순회하면서 패턴 내의 문자들을 일일이 비교하는 방식으로 동작

<img src="https://blog.kakaocdn.net/dn/cukZpf/btqXLenvc65/hPibIkY9DK5lx1fULkO9A0/img.png" alt="img" style="zoom:80%;" />

```python
# Target : 검색 대상 // Pattern : 검색 패턴

target = "SSAFY 10th Let's go"
pattern = "go"


def bruteForce(pattern, target):
    N = len(target)     # 검색 대상의 길이
    M = len(pattern)    # 검색 패턴의 길이

    i = 0   # target의 인덱스
    j = 0   # pattern의 인덱스

    # 각 인덱스가 타겟과 패턴의 길이보다 작을 동안 반복
    while j < M and i < N:
        # 패턴과 다른 곳을 발견했을 때
        if target[i] != pattern[j]:
            # j만큼 온 상태에서 틀린 곳을 발견함
            # 지금위치 - j + 1
            i = i - j
            # -1로 j를 초기화하는 이유, 패턴과 일치하는 문자열이 발견됐을 때
            # j + 1을 해 주기 때문
            j = -1
        # 패턴과 같을 때
        i = i + 1
        j = j + 1
        
    if j == M:
        # 패턴 인덱스 j가 패턴의 길이만큼 탐색된 것 == 탐색 성공
        return i - M
    else:
        return -1
```



### 시간 복잡도

**O(MN)**



## KMP 알고리즘

- 불일치가 발생한 텍스트 스트링의 앞 부분에 어떤 문자가 있는지를 미리 알고 있으므로, 불일치가 발생한 앞 부분에 대하여 다시 비교하지 않고 매칭을 수행
- 패턴을 전처리하여 배열 next[M]을 구해서 잘못된 시작을 최소화함
  - next[M]: 불일치가 발생했을 경우 이동할 다음 위치

![Kmp 문자열 탐색](https://images.velog.io/images/jimmy48/post/2b959c8e-4b4e-40da-8061-ff5343c3c8a9/3.PNG)

```python
def kmp(t, p):
    N = len(t)
    M = len(p)
    lps = [0] * (M+1)
    # preprocessing
    j = 0   # 일치한 개수 == 비교할 패턴 위치
    lps[0] = -1
    for i in range(1, M):
        lps[i] = j
        if p[i] == p[j]:
            j += 1
        else:
            j = 0
    lps[M] = j
    print(lps)
```

```python
# Target : 검색 대상 // Pattern : 검색 패턴

# kmp 전처리
def pre_process(string):
    lps = [0] * len(string)
    j = 0   # lps를 만들기 위한 prefix에 대한 idx
    # i : pattern에서 지나가는 idx
    # j : 지나가고 있는 idx와 pattern 앞 부분과 같은 곳에 대한 idx
    for i in range(1, len(string)):
        # j가 양수이고 target[i]와 target[j]가 다르면 lps[j-1]을 계속 j에 대입
        while j > 0 and string[i] != string[j]:
            j = lps[j-1]
            # target[i]와 target[j]가 일치하면 j를 1증가
        	if string[i] == string[j]:
            	j += 1
            lps[i] = j
    return lps

def KMP(target, pattern):
    lps = pre_process(pattern)    # SKIP TABLE 만들기

    # lps를 활용해서 탐색
    i = 0
    j = 0
    # possition 값이 재할당되지 않는다면, 탐색 실패를 의미
    position = -1
    while i < len(target):
        # 같으면 이동 (고지식한 탐색과 같음)
        if pattern[j] == target[i]:
            j += 1
            i += 1
        else:
            # 다른데, j가 0이 아니라면, i 자리는 유지, j만 이동 후 탐색
            if j != 0:
                j = lps[j - 1]
            # 다른데, j가 0이라면, i를 한 칸 이동해서 처음부터 탐색
            else:
                i += 1
        if j == len(pattern):
            position = i - len(pattern)
            break
    return position

target = 'abaceababaceababad'
pattern = 'abaceababad'
print(KMP(target, pattern))
```



### 시간 복잡도

**O(M+N)**



## 보이어-무어 알고리즘

- 오른쪽에서 왼쪽으로 비교
- 대부분의 상용 소프트웨어에서 채택하고 있는 알고리즘
- 보이어-무어 알고리즘은 패턴에 오른쪽 끝에 있는 문자가 불일치하고 이 문자가 패턴 내에 존재하지 않는 경우, 이동 거리는 무려 패턴의 길이만큼이 됨

![img](https://blog.kakaocdn.net/dn/cusjSo/btqDWrZ1RMA/JmTP9vUbWVMd8RjPJQmDw0/img.png)

```python
# 참고

# Skip table
# 1. 보이어무어 패턴 검색의 장점은 검색하는 패턴의 길이만큼 스킵할 수 있다는 점
#    마지막 idx를 제외할 것임 -> pattern의 마지막 인덱스를 검사했을 때,
#    일치하지 않는다면 len(pattern)만큼 skip할 것임
#    마지막에 나오는 Char는 없는 것으로 취급

def pre_process(T):
    M = len(T)
    
    # 배열 대신 딕셔너리로 skip table 구성
    skip_t = dict()
    # 기록되지 않은 문자는 get() 메서드의 default 값을 활용할 예정
    for i in range(M - 1):  
        skip_t[T[i]] = M - (1 + i)
    return skip_t


def boer_moore(T, P):
    skip_t = pre_process(T)
    M = len(P)
    
    i = 0   # target idx
    while i <= len(T) - M:
        # 뒤에서부터 탐색
        j = M - 1
        # 비교를 시작할 위치 (현재위치 + M번째 idx)
        k = i + (M - 1)
        
        # 탐색할 j idx가 남아 있고, target과 pattern이 같으면 1씩 줄여 가면서 비교
        while i >= 0 and P[j] == T[k]:
            i -= 1
            j -= 1
        # 뒤에서부터 탐색을 하기 때문에, j가 -1값이 된다면, 매칭이 성공했다는 뜻    
        if j == -1:
            position = i
            return position
        
        # Skip할 곳
        # i를 비교해서 탐색을 시작할 위치에 해당하는 문자 T[i + M - 1]
        # skip_t에서 해당 문자를 찾아, 해당 문자의 skip 값만큼 스킵
        i += skip_t.get(T[i + M - 1], M)
```



### 문자열 매칭 알고리즘 비교

- 찾고자 하는 문자열 패턴의 길이: m / 총 문자열 길이: n

- 고지식한 패턴 검색 알고리즘: O(mn)
- 카프-라빈 알고리즘: Ω(n)
- KMP 알고리즘:  Ω(n)

- **보이어-무어 알고리즘**

  - 앞의 두 매칭 알고리즘들의 공통점은 문자열의 문자를 적어도 한 번씩 훑는다는 것이며, 따라서 최선의 경우에도 Ω(n)

  - 보이어-무어 알고리즘은 텍스트 문자를 다 보지 않아도 됨 (패턴의 오른쪽부터 비교)

  - 최악의 경우 수행시간 Ω(mn)

  - 입력에 따라 다르지만 일반적으로 Ω(n)보다 시간이 덜 듦