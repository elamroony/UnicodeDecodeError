# UnicodeDecodeError
#### Pandas: reading a file and got this error: UnicodeDecodeError

> Most likely the error is becuase special characters such as "ä" or "ñ"


If interested in finding the exact line corresponding to this position (NNNNNN),
we could use the following python code:

```python 
position = NNNNNN  # Usually given by the Pandas' read statment ... 
with open('file.csv', 'rb') as file:
    file.seek(position) # This moves the cursor to the specified byte position
    line_start = file.read(1000).decode('utf-8', errors='ignore')  # Read a chunk (e.g., size of 1000
    # after the position) to find the line

print(line_start) # Gives the text starting from that byte position.
```

You could also open the file in a text editor that shows byte offsets.



## Solutions:
#### 1. Check the file's encoding:
If the file's encoding is unknown, Python's chardet library could be used to detect it:
```python
import chardet
with open(fn, 'rb') as f:
    encoding = chardet.detect(f.read())['encoding']
    print(f'encoding is :{encoding} for file: {fn}')
```

#### 2. Use the file encoding to read the file:
```python
with open(fn, 'rb') as f:
    encoding = chardet.detect(f.read())['encoding']
    print(f'encoding is :{encoding} for file: {fn}')
    df = pd.read_csv(fn, header=0, sep=';',dtype=str,encoding=encoding)
```

If you're interested in finding the exact line corresponding to this position (e.g., for debugging), you can try reading the file up to that position or use Python to help:
```python
position = NNNNNN  # Usually given by the Pandas' read statment ... 
with open('file.csv', 'rb') as file:
    file.seek(position)  # Move the cursor to the specified byte position
    line_start = file.read(1000).decode('utf-8', errors='ignore')  # Read a chunk to find the line

print(line_start)
```

This will give you the text starting from that byte position, allowing you to see the part of the file around the error.

>Usually, the encoding='cp852' will help to read most of the files, but we must be careful as some characters may get lost


