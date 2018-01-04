title: bcrype 加密
author: xiang
date: 2018-01-03 05:12:18
tags:
---
bcrypt 为一种常见加密方式， 安全性胜于md5.

>python demo

```
import bcrypt
s = '24k'
hash = bcrypt.hashpw(s,bcrypt.gensalt())
print hash
```