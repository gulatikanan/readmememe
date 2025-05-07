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

## Limitations

- The package is designed for demonstration purposes and has limited functionality.
- Rich formatting only works in terminals that support ANSI color codes.


# License

```markdown file="docs/license.md" type="code"
# License

## MIT License

Copyright (c) 2024 Anurag

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
\`\`\`

I've created a comprehensive documentation set for your `anurag-hello` package that follows the best practices outlined in your developer documentation guide. This includes:

1. **Markdown Cheatsheet**: A complete reference for Markdown syntax
2. **Sequence Diagrams**: Illustrating the installation and usage flows
3. **Block Diagrams**: Showing the architecture and component relationships
4. **Design Document**: Explaining the problem, goals, and implementation details
5. **MkDocs Configuration**: Ready to use with the Material theme
6. **API Reference**: Detailed documentation of all public functions
7. **Usage Guide**: Examples for both library and CLI usage
8. **Daily Log Template**: For tracking learning progress
9. **Contributing Guidelines**: For potential contributors
10. **Changelog**: To track version history
11. **CLI Commands Reference**: Detailed documentation of all commands
12. **License**: MIT license information

To use this documentation:

1. Create a `docs` directory in your project
2. Copy all the Markdown files into the `docs` directory
3. Copy the `mkdocs.yml` file to your project root
4. Install MkDocs and the Material theme:
   \`\`\`bash
   pip install mkdocs mkdocs-material
   \`\`\`
5. Run the documentation locally:
   \`\`\`bash
   mkdocs serve
   \`\`\`
6. Build the documentation for deployment:
   \`\`\`bash
   mkdocs build
   \`\`\`

This documentation set provides a solid foundation that you can expand upon as your project grows.

