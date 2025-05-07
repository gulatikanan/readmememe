# Usage Guide

This guide explains how to use the `anurag-hello` package, both as a library in your Python code and as a command-line tool.

## Using as a Library

### Basic Usage

```python
from anurag_hello import say_hello

# Default greeting
result = say_hello()
print(result)  # Output: Hello, World!

# Custom greeting
result = say_hello("Anurag")
print(result)  # Output: Hello, Anurag!
### Rich Formatted Output

```python
from anurag_hello import print_rich_hello

# Default greeting with rich formatting
print_rich_hello()  # Displays a fancy greeting panel for "World"

# Custom greeting with rich formatting
print_rich_hello("Anurag")  # Displays a fancy greeting panel for "Anurag"
```

### Complete Example

```python
from anurag_hello import say_hello, print_rich_hello

def main():
    # Simple greeting
    message = say_hello("User")
    print(f"Simple greeting: {message}")
    
    # Rich formatted greeting
    print("Rich formatted greeting:")
    print_rich_hello("User")

if __name__ == "__main__":
    main()
```

## Using as a Command-Line Tool

After installing the package, you can use it from the command line.

### Getting Help

```bash
anurag-hello --help

```plaintext

This will display the available commands and options.

### Rich Formatted Greeting

\`\`\`bash
# Greet the world
anurag-hello hello

# Greet a specific person
anurag-hello hello Anurag
```

### Simple Text Greeting

```bash

# Greet the world

anurag-hello simple

# Greet a specific person

anurag-hello simple Anurag

```plaintext

## Integration Examples

### Using in a Script

```python
#!/usr/bin/env python3
import sys
from anurag_hello import say_hello, print_rich_hello

def main():
    if len(sys.argv) > 1:
        name = sys.argv[1]
    else:
        name = "World"
    
    print(f"Simple greeting: {say_hello(name)}")
    print("\nRich greeting:")
    print_rich_hello(name)

if __name__ == "__main__":
    main()
```

### Using in a Larger Application

```python
from anurag_hello import print_rich_hello

class GreetingService:
    def __init__(self, default_name="Guest"):
        self.default_name = default_name
    
    def greet_user(self, name=None):
        """Greet a user with a rich formatted message."""
        if name is None:
            name = self.default_name
        print_rich_hello(name)

# Usage
service = GreetingService("Default User")
service.greet_user()  # Greets "Default User"
service.greet_user("Anurag")  # Greets "Anurag"
```

## Best Practices

1. **Import Only What You Need**: Import specific functions rather than the entire package.
2. **Handle Exceptions**: Although the current functions don't raise exceptions, it's good practice to handle potential errors.
3. **Use Type Hints**: When extending the functionality, continue using type hints for better IDE support.
4. **Document Your Code**: Add docstrings to your functions that use this package.
