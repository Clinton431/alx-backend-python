# Python Variable Annotations

A comprehensive guide to type hinting and variable annotations in Python.

## Overview

Python Variable Annotations provides a structured approach to adding type information to Python code. Introduced in [PEP 484](https://www.python.org/dev/peps/pep-0484/) and enhanced in [PEP 526](https://www.python.org/dev/peps/pep-0526/), variable annotations help developers express the expected types of variables, function parameters, and return values, improving code readability and enabling better tooling support.

## Features

- **Static Type Checking**: Integrate with tools like mypy for compile-time type verification
- **Enhanced Documentation**: Clearly communicate variable purposes and expected values
- **IDE Support**: Enable better code completion and error detection in editors
- **Runtime Type Checking**: Optional runtime verification using third-party libraries
- **Generics and Special Types**: Express complex type relationships
- **Compatibility**: Works with all modern Python versions (3.6+)

## Installation

Python variable annotations are built into Python 3.6 and above. To use static type checking, install mypy:

```bash
pip install mypy
```

## Basic Usage

### Variable Annotations

```python
# Basic variable annotations
name: str = "Python"
version: float = 3.9
enabled: bool = True
tags: list[str] = ["programming", "language"]

# Variables without initial values
future_value: int  # Declare type without assignment
```

### Function Annotations

```python
def calculate_area(length: float, width: float) -> float:
    """Calculate the area of a rectangle."""
    return length * width

def greet(name: str) -> None:
    """Greet a user without returning a value."""
    print(f"Hello, {name}!")
```

### Type Aliases

```python
from typing import Dict, List, Tuple

# Create type aliases for complex types
Point = Tuple[float, float]
Polygon = List[Point]
UserData = Dict[str, str]

# Use the aliases
def translate_point(point: Point, x_offset: float, y_offset: float) -> Point:
    x, y = point
    return (x + x_offset, y + y_offset)

def get_user_info() -> UserData:
    return {"name": "Alice", "email": "alice@example.com"}
```

## Advanced Features

### Optional and Union Types

```python
from typing import Optional, Union

# Optional parameters (value or None)
def find_user(user_id: Optional[int] = None) -> Optional[dict]:
    # Implementation
    pass

# Union types (multiple possible types)
def process_identifier(identifier: Union[int, str]) -> None:
    # Implementation
    pass
```

### Generic Types

```python
from typing import TypeVar, Generic, List

T = TypeVar('T')

class Stack(Generic[T]):
    def __init__(self) -> None:
        self.items: List[T] = []
    
    def push(self, item: T) -> None:
        self.items.append(item)
    
    def pop(self) -> T:
        return self.items.pop()
    
    def empty(self) -> bool:
        return not self.items

# Usage
int_stack: Stack[int] = Stack()
str_stack: Stack[str] = Stack()
```

### Type Comments (for Python 3.5 and below)

```python
# For older Python versions that don't support annotations
def legacy_function(param):
    # type: (str) -> str
    return param.upper()

# Variable type comments
x = 3  # type: int
```

## Static Type Checking with mypy

Create a Python file:

```python
# example.py
def add(x: int, y: int) -> int:
    return x + y

result = add("2", "3")  # Type error
```

Run mypy:

```bash
mypy example.py
```

Output:
```
example.py:4: error: Argument 1 to "add" has incompatible type "str"; expected "int"
example.py:4: error: Argument 2 to "add" has incompatible type "str"; expected "int"
```

## Type Checking in IDEs

Modern IDEs like PyCharm, VS Code with Python extension, and others can use annotations to provide:

- Error highlighting
- Smart code completion
- Documentation hints
- Refactoring assistance

## Best Practices

- Be consistent with annotations throughout your codebase
- Focus on public APIs and complex functions first
- Use descriptive type aliases for complex types
- Consider gradual typing for larger projects
- Document edge cases and special type requirements
- Run static type checkers as part of your CI/CD pipeline

## Common Annotations

```python
from typing import (
    List, Dict, Tuple, Set,
    Optional, Union,
    Callable, Generator, Iterator,
    Any, TypeVar
)

# Collections
numbers: List[int] = [1, 2, 3]
user_scores: Dict[str, int] = {"Alice": 95, "Bob": 87}
coordinates: Tuple[int, int, int] = (10, 20, 30)
unique_tags: Set[str] = {"python", "programming"}

# Functions
callback: Callable[[int, str], bool] = lambda x, s: len(s) == x

# Generators
def count_up_to(n: int) -> Generator[int, None, None]:
    i = 0
    while i < n:
        yield i
        i += 1
```

## Additional Resources

- [PEP 484 - Type Hints](https://www.python.org/dev/peps/pep-0484/)
- [PEP 526 - Syntax for Variable Annotations](https://www.python.org/dev/peps/pep-0526/)
- [mypy Documentation](https://mypy.readthedocs.io/)
- [typing Module Documentation](https://docs.python.org/3/library/typing.html)

## Compatibility Notes

- Full annotation support requires Python 3.6+
- Type comments can be used in Python 3.5 and earlier
- Runtime type checking requires third-party libraries like `typeguard` or `enforce`

