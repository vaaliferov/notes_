```python
from inspect import signature
def foo(a, b:int): pass
str(signature(foo))
```

```python
dir(int), dit(float), dir(str)
dir(tuple), dir(list), dir(set), dir(dict)
```

```python
d = {'a':1,'b':2}
d = dict(a=1, b=2)
d = dict.fromkeys(['c','d'])
d.update({'a':5})

d['a'], d.get('x',-1)
d.keys(), d.values(), d.items()
d.pop('a'), d.popitem()

d is not d.copy()
d == d.copy()
d.clear()
```

```python
from collections import OrderedDict

d = OrderedDict(a=1, b=2)
d = OrderedDict.fromkeys(['c','d'])
d.update({'a':5}), d.move_to_end('a')

d['a'], d.get('x',-1)
d.keys(), d.values(), d.items()
d.pop('a'), d.popitem(), d.popitem(0)

d is not d.copy()
d == d.copy()
d.clear()
```

```python
s = {1,2,3}
s = set([1,2,3])

s.pop()
s.add(4)
s.remove(4)
s.discard(4)
s.update({5,6})

s.isdisjoint({9})
s.issuperset({1,2})
s.issubset({1,2,3,4})

s.union({1,5}) # s | {1,5}
s.difference({1,5}) # s - {1,5}
s.intersection({1,5}) # s & {1,5}
s.symmetric_difference({1,5}) # s ^ {1,5}

s is not s.copy()
s == s.copy()
s.clear()
```

```python
t = (1,)
t = tuple([1])
t.count(1)
t.index(1)
```

```python
l = [1]
l = list([1])
l.append(2)
l.insert(0,7)
l.extend([3,4])

l.count(1)
l.index(1)
l.remove(7)
l.pop(0)
l.pop()

l.sort()
l.reverse()
l.sort(reverse=True)
l.sort(key=lambda x: -x)

sorted(l)
reversed(l)
sorted(l, reverse=True)
sorted(l, key=lambda x: -x)

l is not l.copy()
l == l.copy()
l.clear()
```

```python
l = [1,2,3,4,5,6,7,8,9]
l[0], l[1:], l[1:3], l[:3]
```

```python
s = str()
s = 'abc'

'ab'.isalpha()
'a3'.isalnum()
'2²'.isdigit()
'ↁ'.isnumeric()
'ab'.islower()
'AB'.isupper()
'az'.isascii()
'ac'.endswith('c')
'ac'.startswith('a')
'a\nb'.isprintable()
'Paper Towns'.istitle()

'Ab'.lower() # ascii
'Ab'.upper() # ascii
'AB'.swapcase() # ascii
'AB'.casefold() # unicode
'bob'.capitalize() # ascii
'H\te\tl\tl\to'.expandtabs(3)

'aa'.count('a')
'abbc'.find('b') # -1
'abbc'.rfind('b') # -1
'abbc'.index('b') # exception
'abbc'.rindex('b') # exception

''.join(['a','b'])
'a,b,c'.split(',')
'a\nb\nc\n'.splitlines()
'a,b,c'.split(',', maxsplit=1)
'a,b,c'.rsplit(',', maxsplit=1)

'123'.zfill(5) # '00123'
'cat'.ljust(10) # len = 10
'cat'.rjust(10) # len = 10
'cat'.center(10) # len = 10

'aaabbb'.removeprefix('aaa')
'aaabbb'.removesuffix('bbb')
',,ttg..banana.r'.strip(',.grt')
',,ttg..banana.r'.lstrip(',.grt')
',,ttg..banana.r'.rstrip(',.grt')

# ('a ','soft',' kitty')
'a soft kitty'.partition('soft')
'a soft soft kitty'.rpartition('soft')

'aaa'.encode() # b'aaa'
'яяя'.encode() # b'\xd1\x8f\xd1\x8f\xd1\x8f'

chr(83), ord('S')
'aa'.replace('a','b')
'paper towns'.title()
'Ss'.translate({83:80}) # 'Ps'
str.maketrans('Ss', 'Pp') # {83: 80, 115: 112}
```

```python
# GIL - Global Interpreter Lock
(walrus := True) # walrus operator
a if condition else b # ternary operator
```

```
https://docs.python.org/3/library/

False, True, None, NotImplemented, Ellipsis (...)

abs, aiter, all, any, anext, ascii
bin, bool, breakpoint, bytearray, bytes
callable, chr, classmethod, compile, complex
delattr, dict, dir, divmod, enumerate, eval, exec
filter, float, format, frozenset
getattr, globals, hasattr, hash, help, hex
id, input, int, isinstance, issubclass, iter, len, list, locals
map, max, memoryview, min, next, object, oct, open, ord,
pow, print, property, range, repr, reversed, round
set, setattr, slice, sorted, staticmethod, str, sum, super
tuple, type, vars, zip

os, io, time, argparse
pathlib, stat, glob, shutil
pickle, sqlite3, zlib, gzip, bz2, zipfile, tarfile
heapq, bisect, array, copy, enum, math, random
threading, multiprocessing, subprocess, queue
asyncio, socket, ssl, select, signal
json, mimetypes, base64, html, xml
urllib, http, ftplib
doctest, unittest
pdb, timeit, trace, tracemalloc
venv, sys, builtins, warnings, inspect, importlib
tokenize
```
