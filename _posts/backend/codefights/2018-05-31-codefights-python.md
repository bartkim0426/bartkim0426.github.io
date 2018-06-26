---
layout: post
title:  "codefights: python"
categories: til
---



### 18-05-31

```python
# my answer
commit = "0??+0+!!someCommIdhsSt"

def getCommit(commit):
    return ''.join([x for x in list(commit) if x.isalpha()])

# best answer
def getCommit(commit):
    return commit.lstrip('0?+!')
```


### 18-06-05

**lambda**

```python
repeatChar = lambda ch, n: ch * n
```

```python
def getPoints(answers, p):
    questionPoints = lambda i, ans: i+1 if ans else -p

    res = 0
    for i, ans in enumerate(answers):
        res += questionPoints(i, ans)
    return res
```


```python
def sortStudents(students):
    students.sort(key= lambda s: s.split()[-1] )
    return students
```

```python
def isTestSolvable(ids, k):
    digitSum = lambda x: sum(map(int, str(x)))

    sm = 0
    for questionId in ids:
        sm += digitSum(questionId)
    return sm % k == 0
```

### 6/14

```python
def coolPairs(a, b):
    uniqueSums = {x+y for x in a for y in b if (x*y) % (x+y) == 0}
    return len(uniqueSums)
```



### 6/19

```python
def chessTeams(smarties, cleveries):
    return [list((x, y)) for x, y in zip(smarties, cleveries)]

def chessTeams(smarties, cleveries):
    return list(zip(smarties, cleveries))
```

### 6/22

```python

```
