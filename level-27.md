# Python Challenge - Level 27

- Link: http://www.pythonchallenge.com/pc/hex/speedboat.html
- Username: **butter**
- Password: **fly**

## Problem

![](src/level_27/zigzag.jpg)

## Solution

```python
>>> palette = im.getpalette()[::3]
>>> len(palette)
256

>>> b = bytes([i for i in range(256)])
>>> b
b'\x00\x01\x02\x03\x04\x05\x06\x07\x08\t\n\x0b\x0c\r\x0e\x0f\x10\x11\x12\x13\x14\x15\x16\x17\x18\x19\x1a\x1b\x1c\x1d\x1e\x1f !"#$%&\'()*+,-./0123456789:;<=>?@ABCDEFGHIJKLMNOPQRSTUVWXYZ[\\]^_`abcdefghijklmnopqrstuvwxyz{|}~\x7f\x80\x81\x82\x83\x84\x85\x86\x87\x88\x89\x8a\x8b\x8c\x8d\x8e\x8f\x90\x91\x92\x93\x94\x95\x96\x97\x98\x99\x9a\x9b\x9c\x9d\x9e\x9f\xa0\xa1\xa2\xa3\xa4\xa5\xa6\xa7\xa8\xa9\xaa\xab\xac\xad\xae\xaf\xb0\xb1\xb2\xb3\xb4\xb5\xb6\xb7\xb8\xb9\xba\xbb\xbc\xbd\xbe\xbf\xc0\xc1\xc2\xc3\xc4\xc5\xc6\xc7\xc8\xc9\xca\xcb\xcc\xcd\xce\xcf\xd0\xd1\xd2\xd3\xd4\xd5\xd6\xd7\xd8\xd9\xda\xdb\xdc\xdd\xde\xdf\xe0\xe1\xe2\xe3\xe4\xe5\xe6\xe7\xe8\xe9\xea\xeb\xec\xed\xee\xef\xf0\xf1\xf2\xf3\xf4\xf5\xf6\xf7\xf8\xf9\xfa\xfb\xfc\xfd\xfe\xff'
>>> len(b)
256
```


Full Solution

```python
from PIL import Image
import bz2

im = Image.open('zigzag.gif')


palette = im.getpalette()[::3]

table = bytes.maketrans(bytes([i for i in range(256)]), bytes(palette))

raw = im.tobytes()

trans = raw.translate(table)

zipped = list(zip(raw[1:], trans[:-1]))

diff = list(filter(lambda p: p[0] != p[1], zipped))

indices = [i for i,p in enumerate(zipped) if p[0] != p[1]]

im2 = Image.new("RGB", im.size)
colors = [(255, 255, 255)] * len(raw)
for i in indices:
    colors[i] = (0, 0, 0)
im2.putdata(colors)
#im2.show()


s = [t[0] for t in diff]
text = bz2.decompress(bytes(s))

import keyword
print(set([w for w in text.split() if not keyword.iskeyword(w.decode())]))
```

Result:

```
{b'exec', b'print', b'../ring/bell.html', b'repeat', b'switch'}
```

Well, this level needs to be updated, since ``exec`` and ``print`` are not keywords in Python 3. The only two words left are ``repeat`` and ``switch``

## Next Level

http://www.pythonchallenge.com/pc/ring/bell.html

- username: repeat
- password: switch
