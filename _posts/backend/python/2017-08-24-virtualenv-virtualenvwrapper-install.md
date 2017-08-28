---
layout: post
title:  "install virtualenv, virtualenvwrapper in max os"
categories: linux
---

## install virtualenv, virtualenvwrapper in max os

### installing virtualenv, virtualenvwrapper
```bash
pip install virtualenv virtualenvwrapper

export WORKON_HOME=~/virtualenv
mkdir -p $WORKON_HOME
source /usr/local/bin/virtualenvwrapper.sh
```

### Make virtualenv (basic usage)
```bash
mkdirvirtualenv myenv
# activate virtualenv
workon myenv
# deactivate virtaulenv
source deactivate
```
