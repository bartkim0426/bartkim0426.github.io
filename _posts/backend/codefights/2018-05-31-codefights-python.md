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

### 7/11

```javascript
function allLongestStrings(inputArray) {
    max_len = inputArray.sort(function(a, b) {return b.length - a.length;})[0].length;
    return inputArray.filter(function(obj){
        return obj.length === max_len;
    })
}

// more short
function allLongestStrings(inputArray) {
    max_len = Math.max(...inputArray.map(x => x.length));
    return inputArray.filter(x => x.length === max_len);
}
```


### 7/16

```javascript
function alternatingSums(a) {
    var x = 0;
    var y = 0;
    a.forEach(function(el, index){
        if ((index+1) % 2 === 1) {
            x += el;
        } else {
            y += el;
        }
    })
    return [x, y]
}
```

16. addBorder

```
function addBorder(picture) {
    var border = '*'.repeat(picture[0].length + 2)
    new_list = [border]
    picture.forEach(function(el, ind) {
        new_list.push("*" + el + "*");
    })
    new_list.push(border);
    return new_list;
}
```


### 7/18


18. palindromeRearranging

```python
def palindromeRearranging(s):
    from collections import Counter
    result_li = [i % 2 for i in Counter(list(s)).values()]
    if sum(result_li) <= 1:
        return True
    return False
```

```javascript
function palindromeRearranging(inputString) {
    var string_arr = a.split('');
    var counts = {};
    string_arr.forEach(function(x) {
        counts[x] = (counts[x] || 0) + 1;
    });
    var result = 0 ;
    Object.values(counts).forEach(function(x){
        result += (x % 2);
    });
    if (result <= 1) {
        return true
    } else {
        return false
    }
}
```

19. `areEquallyStrong`

```python
def areEquallyStrong(yourLeft, yourRight, friendsLeft, friendsRight):
    return ((max(yourLeft, yourRight) == max(friendsLeft, friendsRight)) and ((yourLeft + yourRight) == (friendsLeft + friendsRight)))
```

```javascript
function areEquallyStrong(yourLeft, yourRight, friendsLeft, friendsRight) {
    return (Math.max(yourLeft, yourRight) === Math.max(friendsLeft, friendsRight)) && ((yourLeft + yourRight) === (friendsLeft + friendsRight))
}
```

### 7/19

20. `arrayMaximalAdjacentDifference`

```python
def arrayMaximalAdjacentDifference(l):
    result = 0
    for i in range(len(l)-1):
        if l[i] >= l[i+1]:
            _result = abs(l[i] - l[i+1])
        else:
            _result = abs(l[i+1] - l[i])
        if _result > result:
            result = _result
    return result

// short answer
def arrayMaximalAdjacentDifference(l):
    return max[abs(a[i]-a[i+1]) for i in range(len(a)-1)]
```

```javascript
function arrayMaximalAdjacentDifference(inputArray) {
    var result = 0;
    inputArray.forEach(function(el, index){
        if (index < inputArray.length - 1) {
            var tmp = Math.abs(el - inputArray[index+1]);
            if (result < tmp) {
                result = tmp;
            }
        }
    })
    return result;
}

// use map
function arrayMaximalAdjacentDifference(arr) {
  return Math.max(...arr.slice(1).map((x,i)=>Math.abs(x-arr[i])))
}
```

21. `isIPv4Address`

```python
def isIPv4Address(s):
    l = s.split('.')
    if len(l) != 4:
        return False
    for i in l:
        try:
            if int(i) < 0 or int(i) > 255:
                return False
        except:
            return False
    return True

# using ipaddress
import ipaddress
def isIPv4Address(inputString):
    try:
        ipaddress.ip_address(inputString)        
    except:
        return False
    return True
```

```javascript
function isIPv4Address(inputString) {
    ip_arr = inputString.split('.');
    var result = true;
    if (ip_arr.length !== 4) {
        result = false;
    }
    ip_arr.forEach(function(el, index){
        if (el > 255 || isNaN(el) || el === '') {
            result = false;
        }
    })
    return result;
}
```

### 7/20

22. `avoidObstacles`

```python

```

```javascript

```

### 7/26

`MineSweaper`

```python
def minesweeper(matrix):
    x, y, = len(matrix), len(matrix[0])
    result = [[0]*y]*x

    for a in len(x):
        for b in len(y):
            if matrix[x][y]:
                current = [x, y]
                plus_minus = [1, -1]
                m = matrix[x + 1][y]
                try:
                    
```
