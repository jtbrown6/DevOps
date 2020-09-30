# Python

Get a list of `types` in python:
```
dir(__builtins__)
```

To get more detail on what you can do with a type which will print out the `attributes/methods` of the type. ex, list, int, str


```
dir(str)
 >> 'capitalize', 'replace', 'upper', 'zfill']
```

To get more detail on what you can do with a `METHOD` or `PROPERTY` use the `help()` function. 
```
help(str.upper) <- Method
 >>> "hello".upper()
```

### Dictionaries

```
grades = {"Jamal": 10, "Other":5}
grades.keys() OR grades.values()
```
