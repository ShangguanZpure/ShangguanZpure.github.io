---
title: python错误输出和日志模块
date: 2021-11-18 19:42:44
tags:
- 工作
- 技术
- python
- logger
categories: 技术
description: python日志系统
photos:
---

# python 错误日志输出  

```python
	# 堆栈格式化输出
import traceback
try:
  print('error')
except Exception as e:
  print(traceback.format_exc())
```

# python日志模块使用  

如何让python日志自动归档？

