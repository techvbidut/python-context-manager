# CONTEXT MANAGER IN PYTHON

<i>How often have you used files in your program? I hope you remeber how to open a text file to write it using `open('test.txt', 'w')`. Do you remember closing it? 
If you haven't close a file yet you should start doing it. Have a look:</i>

```
f = open("mydemofile.txt", "r")
print(f.readline())
f.close()
```

## Why do we need to close a file/resource after use?
If there are a lot of files and resources opened but, not closed properly. Then the program will eventually run out of file handles or memory space and crash at last.

Let us understand with an example:

```
file_list = []
for x in range(10000):
    file_list.append(open('testing.txt', 'w'))
```

This will throw an OS error: `OSError: [Errno 24] Too many open files: 'testing.txt'`, if NOT, then try increasing the range.

<b>The problem:</b> 
1. Resources are limited in supply.
2. As a developer, we tend to forget releasing a resource after use.
3. This creates a leakage in resouces and therefore slows down the system or may crash in the worst case.

<b>The solution:</b>
1. We can make a mechanism in which closing or releasing of the resource will be automated.
2. In python, we have `context managers` to help us in the proper handling of resources.

We can make our context manager using classes. We must ensure that this class has the two following methods:
1. __enter__ (returns the resource that needs to be managed like opening a file and returning file object)
2. __exit__ (does not return anything but performs the cleanup operations like closing files and database connections)

### <i>Let us understand with an example of file management using context manager</i>

```
class CustomFileContextManager():
    def __init__(self, filename, mode):
        self.filename = filename
        self.mode = mode
        self.file = None
         
    def __enter__(self):
        self.file = open(self.filename, self.mode)
        return self.file
     
    def __exit__(self, exc_type, exc_value, exc_traceback):
        self.file.close()
 
with CustomFileContextManager('testing.txt', 'w') as f:
    f.write('Testing context manager')
 
print(f.closed)
```

`To be changed`
File management using context manager and with statement: On executing the with block, the following operations happen in sequence:

A FileManager object is created with test.txt as the filename and w(write) as the mode when __init__ method is executed.
The __enter__ method opens the test.txt file in write mode(setup operation) and returns a file object to variable f.
The text ‘Test’ is written into the file.
The __exit__ method takes care of closing the file on exiting the with block(teardown operation). When print(f.closed) is run, the output is True as the FileManager has already taken care of closing the file which otherwise needed to be explicitly done.
