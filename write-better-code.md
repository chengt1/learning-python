# 10 Tips and Tricks
Reference: [Corey Schafer](https://www.youtube.com/watch?v=C-gEQdGVXbk)
Markdown [reference](https://github.com/tchapi/markdown-cheatsheet/blob/master/README.md)

## Ternary Operator
Ternary means having 3 elements.

This is not pythonic
```python
condition = False
if condition:
    x = 1
else:
    x = 0
print(x)
```

You are able to replace the if else statements with a ternary operation
```python
condition = False
x = 1 if condition else 0
print(x)
```

## Working with large numbers

It is hard to read when working with large numbers.

Without underscore
```python
num1 = 100000000000
num2 = 100000000

total = num1 + num2

print(total)
```
Result is 
```bash
100100000000
```

You can make large numbers easier to read by adding _underscores_ and it won't affect the program.

With underscore
```python
num1 = 100_000_000_000
num2 = 100_000_000

total = num1 + num2

print(total)
```
The output is still quite unreadable so we can add string formatting 
```python
print(f'{total:,}')
```
The output becomes
```bash
100,100,000,000
```

## Context Manager
Whenever you need to manually close resources, you should look for context managers that can do this for you.

For example, the following code reads from a file then closes it. There are many things that can go wrong between `open()` and `f.close`. This may result in resources not closed and can become corrupted. We want a way to automatically close the file.
```python
f = open('test.txt', 'w')

f.write('Hello from context manager!')

f.close()
```

A naive way can be to use try, except, finally to catch any exceptions. However, this is unnecessarily vervose.
```python
f = open('test.txt', 'w')
try:
    f.write('Hello from context manager!')
finally:
    f.close()
```

Use the keyword *with* to use context managers. There are also managers for database connections (mysql).
```python
with ('test.txt', 'w') as f:
    f.write('Hello from context manager!')
```

## Enumerate function
Languages like C and JavaScript have indices that you can access.  

```python
names = ['Amy', 'Bob', 'Chris', 'Dave']

for name in names:
    print(name)
```
You may want the indices to be printed with each element while looping through them in python. You might do something unpythonic like this:
```python
names = ['Amy', 'Bob', 'Chris', 'Dave']

index = 0
for name in names:
    print(index, name)
    index += 1
```
The output will be
```bash
0 Amy
1 Bob
2 Chris
3 Dave
```

The following is the correct way and involves unpacking:
```python
names = ['Amy', 'Bob', 'Chris', 'Dave']

for index, name in enumerate(names, start =1):
    print(index, name)
    index += 1
```
You can even specify the starting index number!

The output becomes:
```bash
1 Amy
2 Bob
3 Chris
4 Dave
```
## Zip function
Now, what if you want to loop across 2 or more iterable objects at the same time?

You might try something like this after learning the enumerate function:
```python
names = ['Peter Parker', 'Clark Kent', 'Wade Wilson', 'Bruce Wayne']
heroes = ['Spiderman', 'Superman', 'Deadpool', 'Batman']

for index, name in enumerate(names):
    hero = heroes[index]
    print(f'{name} is actually {hero}') 
```

The output is
```bash
Peter Parker is actually Spiderman
Clark Kent is actually Superman
Wade Wilson is actually Deadpool
Bruce Wayne is actually Batman
```

This is unpythonic. Use the *zip* function to loop through 2 lists at once!
```python
for name, hero in zip(names, heroes):
    print(f'{name} is actually {hero}') 
```

Or add another list if you'd like.
```python
names = ['Peter Parker', 'Clark Kent', 'Wade Wilson', 'Bruce Wayne']
heroes = ['Spiderman', 'Superman', 'Deadpool', 'Batman']
universes = ['MCU', 'DC', 'MCU', 'DC']

for name, hero, universe in zip(names, heroes, universes):
    print(f'{name} is actually {hero} from {universe}') 
```
The iterator will stop when the shortest list ends if you are using lists of different lengths. If you want to it to go to the end of the longest list, you'll need to use zipLongest function from the Itertools module.

## Unpacking
Assign values to multiple vars at the same time.
Verbose way:
```python
a = 1
b = 2
```
Use packing
```python
a, b = (1,2)
```

To ignore a var to eliminate annoying warnings of unused vars, the convention is to use an underscore `_`.
Say, to ignore the second element of the tuple:
```python
a, _ = (1,2)
```

The following will have errors:
```python
# Not enough values to unpack
a, b, c = (1,2)

# Too many values to unpack
a, b = (1, 2, 3)
```

You can actually assign multiple values to a single var:
```python
a, b, *c = (1, 2, 3, 4, 5)

```
c now contains `[3, 4, 5]`.

You can also ignore all vars after b:
```python
a, b, *_ = (1, 2, 3, 4, 5)
```

It also works when you want to ignore variables in the middle:
```python
# c contains 3 and 4
# d is the last element. 5
a, b, *c, d = (1, 2, 3, 4, 5)

# 3 and 4 are ignored
# d is the last element. 5
a, b, *_, d = (1, 2, 3, 4, 5)
```

## Get and set Attributes of an object
```python
# empty class
class Person():
    pass

person = Person()
```

You can add attributes like this
```python
person.first = "Timothy"
person.last = "Cheng"
```
This can get tedious when you have multiple attributes to set.
Use `setattr(object_name, key_name, value)`.

```python

personal_info = {'first': 'Timothy', 'last':'Cheng'}

for key, value in person_info.items():
    setattr(person, key, value)
```

You can also use `getattr(person, key)`.

## Sensitive secret in terminal
```python
password = input('Password':)
```
This is bad because the password is in plain view.

Use this instead:
```python
from getpass import getpass
password = getpass('Password':)
```

## Run a python module
To run a module
```bash
python -m module_name
```
`-m` searches through SYS.PATH i.e. runs the module 

I usually run python files as
```bash
python3 passowrd.py
```
But I can also run it as 
```bash
python -m password
```
This works becuase the current directory is added to SYS.PATH.

## Knowing where and how to find information
```bash
python -m smtpd -c DebuggingServer -n localhost:1025
```
This looks complicated, right? How do I know what parameters I can use?
To learn more, you can use `help`.
```python
import smtpd
help(smtpd)
```
What attributes and methods are available? Use `dir`
```python
import datetime

dir(datetime)

# to inspect whether it is a method or attribute
datetime.today
```
