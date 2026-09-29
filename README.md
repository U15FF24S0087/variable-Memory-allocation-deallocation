# Variables and Memory in Node.js and Python

Variables give names to values. They allow a program to store information, use it later, and pass it between functions or modules.

## 1. What is a variable?

In both Python and JavaScript, a variable is a name that refers to a value. When you assign a value to a variable, the name is bound to that value.

```python
x = 10
x = "ten"
```

Here, `x` first refers to an integer object and later refers to a string object. This is a key idea in Python: the name changes, not the object itself.

In JavaScript, variables are declared with `let`, `const`, or `var`.

```js
let user = { name: "Sam" };
const settings = { theme: "light" };
settings.theme = "dark"; // allowed
```

The variable `settings` is still the same object; only its internal property changed.

## 2. Everything in Python is an object

Python follows an object model. Every value is an object, and objects have:

- identity
- type
- value

```python
x = 10
print(id(x))
print(type(x))
print(x)
```

Example objects:

- `x = 10` -> integer object
- `name = "Darshan"` -> string object
- `marks = 85.5` -> float object
- `numbers = [10, 20, 30]` -> list object

## 3. Python built-in data types

Python has several built-in data types.

### Numeric types

```python
age = 25
count = -10
price = 99.90
z = 3 + 4j
```

- `int` for integers
- `float` for decimal numbers
- `complex` for complex numbers

### Boolean

```python
is_active = True
is_logged_in = False
```

Booleans are also objects in Python.

### String

```python
name = "Darshan"
```

A string is an immutable sequence of characters.

```python
name[0]   # 'D'
name[1]   # 'a'
```

### Sequence types

```python
numbers = [10, 20, 30]   # list
points = (10, 20)        # tuple
```

Properties:

- list: ordered, mutable, allows duplicates
- tuple: ordered, immutable, allows duplicates

### Set and dictionary

```python
numbers = {10, 20, 30, 30}   # set
students = {"id": 101, "name": "Darshan", "marks": 85}
```

- set: unique elements, mutable, no indexing
- dict: stores data as key-value pairs

### `None`

```python
result = None
```

`None` means “no value” or “absence of value.” It is not the same as `0`, `False`, an empty string, or an empty list.

## 4. Mutable vs immutable objects

An immutable object cannot be changed after creation.

Examples:

- `int`
- `float`
- `bool`
- `str`
- `tuple`

A mutable object can be changed after creation.

Examples:

- `list`
- `dict`
- `set`
- `bytearray`

```python
x = 10
x = 20
```

This does not change the original integer object. It creates a new integer object and rebinds `x` to it.

### Mutable example

```python
a = [10, 20]
b = a
b.append(30)
print(a)
```

Output:

```python
[10, 20, 30]
```

Both `a` and `b` reference the same list object, so modifying the list affects both names.

## 5. `==` vs `is`

```python
a = [1, 2]
b = [1, 2]

print(a == b)  # True: same values
print(a is b)  # False: different objects
```

- `==` checks value equality
- `is` checks whether two references point to the same object

## 6. Memory in Node.js

Node.js runs JavaScript with the V8 engine. V8 manages memory automatically.

Key points:

- Objects, arrays, and functions are stored in the heap.
- Variables are associated with function scope and execution context.
- The garbage collector reclaims memory that is no longer reachable.
- V8 may optimize memory usage and object lifetimes differently across runs.

```js
const user = { name: "Sam" };
```

This object is stored in heap memory. If nothing references it anymore, the garbage collector can collect it later.

## 7. Memory in Python

Python also manages memory automatically. In CPython, the most common implementation:

- names refer to Python objects
- objects are stored on a managed heap
- reference counting helps reclaim objects when no references remain
- the cyclic garbage collector handles reference cycles
- freed memory may be reused rather than immediately returned to the operating system

```python
a = [1, 2, 3]
b = a
```

Now both `a` and `b` point to the same list object.

If you delete one reference:

```python
del b
```

The object still exists as long as `a` still references it.

## 8. Reference counting and garbage collection

Python uses reference counting to track object lifetimes. An object is usually removed when its reference count reaches zero.

```python
a = [1, 2, 3]
b = a
```

At this point, the list has two references.

```python
del b
```

The reference count decreases, and the object remains alive while `a` still references it.

### Cyclic references

Some objects can reference each other, creating a cycle:

```python
a = []
a.append(a)
```

This list refers to itself. Reference counting alone cannot always collect this pattern, so Python uses a cyclic garbage collector.

## 9. Variable lifetime and object lifetime

A variable and an object are related but not identical.

- A local variable usually disappears when its function ends.
- An object can remain alive if another reference still points to it.
- A global variable may exist while the module or program is active.
- Memory is not always returned to the OS immediately after an object is no longer used.

```python
x = 10
```

The variable `x` is a name that refers to the integer object `10`. The exact memory lifecycle depends on the runtime and implementation.

## 10. Quick comparison

| Topic | Node.js (V8) | Python (CPython) |
| --- | --- | --- |
| Names and values | Variables are bindings to values | Names are bound to Python objects |
| Memory model | Managed heap + garbage collector | Managed heap + reference counting + cyclic GC |
| Reclaiming memory | When GC decides memory is unreachable | When ref count reaches zero, or cyclic GC handles leftovers |
| Immediate OS return | No | No |

## 11. Practical takeaway

- Keep references only as long as you need them.
- Do not assume a variable is a fixed memory box.
- In Python, names refer to objects; objects may be shared, reused, or replaced.
- In JavaScript, `const` prevents rebinding, not mutation of an object.
- The runtime handles allocation and cleanup automatically in both languages.

## 12. Summary

Variables are names that refer to values. In Python, everything is an object, and the identity, type, and value of each object matter. In Node.js, memory is also managed automatically by the runtime, especially through the V8 garbage collector. The exact timing of cleanup is not always immediate, but the runtime ensures that unreachable memory is eventually reclaimed.

example a=[1,2,3]
del a
del a removes the name or reprence a
it does not mean immidetly destroy this object
if another reference exists 
numbers=[1,2,3]
b = numbers
del numbers

print(number)

out put : 123
the object is still reahed to b
when can an object an garbage
example 
a = [1,2,3]
b = a
del a
del b
now there are no remaining reference to that list from this names
it becomes aligible for reclaimaion 
the exact timing of memory beging retun or reuse is implemention-defentent 

variable -->object--->memory--->garbage collector

variable ---> object(identity,type,value)----->memory---->no longer reachable------>garbage collection

if python as garbage collection,why does not del numbers neccesarily destroy the object immidetly?
