# funcall
UFCS for Python.

An implementaion of [UFCS (Uniform Function Call Syntax)](https://en.wikipedia.org/wiki/Uniform_Function_Call_Syntax) for Python. This package allows any function to be called using the syntax for member accessing. An object wrapped in the `funcall.obj` function accepts any function as its own member. The object is converted to a `funcall.ChainableObject`, but it behaves as expected.

```python
from funcall import obj

x = obj(5).range.sum
# This is equivalent to;
# x = sum(range(5))

y = x + 5
# x is a ChainableObject, but behaves as an int.
```


## Installation
`funcall` has no dipendencies. Tested on Python 3, might work on Python 2.

Not currently uploaded to PyPi.

## Usage
### ChainableObject.map, filter, reduce
Implementations as methods of builtin-functions. Arguments passed to them should be callable objects.

```python
from funcall import obj

obj('spam bacon sausage spam').split.map(str.capitalize)
# This is equivalent to;
map(str.capitalize, 'spam bacon sausage spam'.split())
```


### ChainableObject.call
A method which calls function passed as an argument. This is useful when using anonymous functions or functions bound to different namespaces.

This fork includes an extension that will traverse the stack frames in search of a match against local defines as a fallback. This allows the usage of locally defined or imported functions, simple lambdas and wrappers, but does not replace more generic and useful `.call` method.

```python
from funcall import obj

obj('spam bacon sausage spam').split.call('-'.join)
# This is equivalent to;
'-'.join('spam bacon sausage spam'.split())

square = lambda x: x**2
obj(3).square
# This is equivalent to:
square(3)

import numpy as np

obj(5).call(np.arange)
# This is equivalent to;
np.arange(5)
# obj(5).np.arange doesn't work...
```

------
Originally by [koji-kojiro](https://github.com/koji-kojiro/funcall). Copyright continues to belong to them.
Extended with scoped lookup.
