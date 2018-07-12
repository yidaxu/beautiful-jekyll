---
layout: post
title: Tips for python coding style
bigimg: /img/path.jpg
tags: [python, coding style]
---

###  Only use one way to do thing.

```python
a = [1]
a = a + [2] # kind of okay.
a.append([2]) # but we pref this way.
```
### Never commit before fix PEP 8 coding style violation. Coding style is very very very important 
* in pycharm, right-click - inspect_code.
* fix PEP 8 coding style violation.

### Never include test code and unnecessary code, comments in the final version of github. 
This may cause other people's confusion. Your code are needed by others.

### import order.
```python
sytem model
# one space
third party

internal model 
```

### poc
Do test data in poc namely proof of correctness.
