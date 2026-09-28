# Variables and Memory in Node.js and Python

Variables give names to values. They let programs store information, work with it, and pass it between functions and other parts of an application.

## Navigation

<a href="#what-a-variable-refers-to"><kbd>What a Variable Refers To</kbd></a>
<a href="#memory-in-nodejs"><kbd>Memory in Node.js</kbd></a>
<a href="#memory-in-python"><kbd>Memory in Python</kbd></a>
<a href="#variable-and-object-lifetime"><kbd>Lifetime and Cleanup</kbd></a>
<a href="#quick-comparison"><kbd>Quick Comparison</kbd></a>

## What a Variable Refers To

In both languages, a variable is best understood as a name that refers to a value. Assigning an object to another variable usually creates another reference to that same object; it does not automatically copy the object.

```js
const user = { name: "Sam" };
```

```python
user = {"name": "Sam"}
```

JavaScript uses declarations such as `let`, `const`, and `var`. Python names are created when they are assigned, and a name can later refer to a value of a different type.

```python
value = 10
value = "ten"  # The name now refers to a string instead.
```

In JavaScript, `const` prevents assigning a different value to the binding, but it does not make an object immutable:

```js
const settings = { theme: "light" };
settings.theme = "dark"; // This is allowed.
```

## Memory in Node.js

Node.js runs JavaScript using the V8 engine. V8 manages memory automatically.

- Objects, arrays, and functions are generally stored in managed memory called the heap.
- Local variables are associated with their scope and function execution. V8 may represent or optimize them in different ways, so it is not accurate to say that every variable always occupies a fixed location on the stack.
- The garbage collector finds objects that the program can no longer reach and reclaims their memory. V8 uses generations, so objects that live longer may be handled differently from newly created objects.

These are implementation details of V8, and the exact representation can change as the engine optimizes code.

## Memory in Python

Python manages memory automatically too. In the common CPython implementation:

- Names refer to Python objects, which are generally allocated on a managed heap.
- Function calls have frames that hold local names and execution state.
- Reference counting usually allows an object to be reclaimed when no references to it remain.
- A cyclic garbage collector handles groups of objects that refer to one another but are otherwise unreachable.
- Python's allocator may reuse freed memory rather than immediately returning it to the operating system.

These details describe CPython, the most widely used Python implementation. Other Python implementations may manage memory differently.

## Variable and Object Lifetime

There is no fixed expiry time for every variable or object. Scope and object lifetime are related, but they are not the same:

- A local name is generally no longer available after its function returns.
- An object can outlive that local name if another reference remains, for example in a global variable, a list, or a closure.
- A global name can remain available while its module or program is active.
- Once an object is no longer reachable, the runtime can reclaim it. The exact timing depends on the language implementation and its memory manager.

Removing a name or reference does not guarantee that memory is immediately returned to the operating system. In Python, `del name` removes a binding. In JavaScript, assigning `null` can remove that variable's reference to an object. Neither operation forces immediate garbage collection.

## Quick Comparison

| Topic | Node.js (V8) | Python (CPython) |
| --- | --- | --- |
| How names work | Declared bindings refer to values | Names are bound to objects when assigned |
| Main memory management | Garbage collection | Reference counting and cyclic garbage collection |
| When unused memory is reclaimed | When the garbage collector determines it can be reclaimed | Usually when reference counts reach zero; cycles are handled by the cyclic collector |
| Is memory returned to the OS immediately? | Not necessarily | Not necessarily |

**Practical takeaway:** Keep references only as long as you need them. For ordinary variables, let the runtime manage allocation and cleanup; you usually do not manually allocate or free memory.