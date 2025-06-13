# To run this code you need to install the following dependencies:
# pip install google-genai

import base64
import os
from google import genai
from google.genai import types


def generate():
    client = genai.Client(
        api_key=os.environ.get("GEMINI_API_KEY"),
    )

    model = "gemini-2.0-flash"
    contents = [
        types.Content(
            role="user",
            parts=[
                types.Part.from_text(text="""README.md"""),
            ],
        ),
        types.Content(
            role="model",
            parts=[
                types.Part.from_text(text="""Okay, I'm ready to help you create a `README.md` file.  To give you the best possible starting point, I need a little more information.  Please tell me about your project:

**What kind of project is this?**  (e.g., a Python library, a JavaScript web app, a command-line tool, a data analysis project, a game, etc.)

**What is the project's purpose?** What problem does it solve? What does it do?

**What are the key features?**

**Who is the target audience?** (e.g., other developers, end-users, data scientists)

**What are the installation instructions?** (if applicable)

**What are the usage instructions?** (How do people use your project?)  Include code examples if possible.

**What are the dependencies?** (e.g., libraries, frameworks, specific software versions)

**How can people contribute?** (e.g., reporting bugs, submitting pull requests)

**What is the license?** (e.g., MIT, Apache 2.0, GPL)

**Is there any other important information I should include?** (e.g., links to documentation, tutorials, demos, related projects, or contact information.)

**Example Scenario and Response:**

Let's say you tell me:

*   **Project Type:** Python Library
*   **Purpose:**  A simple library for calculating the Fibonacci sequence.
*   **Key Features:**  `fibonacci(n)` function returns the nth Fibonacci number; `fibonacci_sequence(n)` returns a list of the first n Fibonacci numbers.
*   **Target Audience:** Python developers.
*   **Installation:** `pip install my-fibonacci-lib`
*   **Usage:**
    ```python
    from my_fibonacci_lib import fibonacci, fibonacci_sequence

    print(fibonacci(10))  # Output: 55
    print(fibonacci_sequence(5)) # Output: [0, 1, 1, 2, 3]
    ```
*   **Dependencies:** None
*   **Contribution:** Bug reports and pull requests welcome.
*   **License:** MIT
*   **Other:**  Link to online documentation.

Then I would generate something like this:

```markdown
# My Fibonacci Library

A simple Python library for calculating Fibonacci numbers.

## Features

*   `fibonacci(n)`: Returns the nth Fibonacci number.
*   `fibonacci_sequence(n)`: Returns a list of the first n Fibonacci numbers.

## Installation

```bash
pip install my-fibonacci-lib
```

## Usage

```python
from my_fibonacci_lib import fibonacci, fibonacci_sequence

print(fibonacci(10))  # Output: 55
print(fibonacci_sequence(5)) # Output: [0, 1, 1, 2, 3]
```

## Dependencies

None.

## Contributing

Bug reports and pull requests are welcome!

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Documentation

For more detailed documentation, please visit [https://example.com/my-fibonacci-lib-docs](https://example.com/my-fibonacci-lib-docs).
```

**Now, please provide me with the details of your project so I can create a `README.md` for you.**
"""),
            ],
        ),
        types.Content(
            role="user",
            parts=[
                types.Part.from_text(text="""INSERT_INPUT_HERE"""),
            ],
        ),
    ]
    generate_content_config = types.GenerateContentConfig(
        response_mime_type="text/plain",
    )

    for chunk in client.models.generate_content_stream(
        model=model,
        contents=contents,
        config=generate_content_config,
    ):
        print(chunk.text, end="")

if __name__ == "__main__":
    generate()
