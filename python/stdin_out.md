```python
import sys
import os

in_file = './io_files/in'
out_file = './io_files/out'
print(os.getcwd())
if not os.path.exists(in_file):
    print("no such input file")
os.path.exists(out_file)

in_f = open(in_file, 'r')
out_f = open(out_file, 'w')
sys.stdin, sys.stdout = in_f, out_f

```