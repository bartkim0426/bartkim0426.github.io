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

