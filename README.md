# django-idea-analyzer
[![PyPI version](https://badge.fury.io/py/django-idea-analyzer.svg)](https://badge.fury.io/py/django-idea-analyzer)
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](https://opensource.org/licenses/MIT)
[![Downloads](https://static.pepy.tech/badge/django-idea-analyzer)](https://pepy.tech/project/django-idea-analyzer)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-blue)](https://www.linkedin.com/in/eugene-evstafev-716669181/)


**django-idea-analyzer** is a tiny helper library that leverages LLM7 (or any LangChain‑compatible LLM) to evaluate textual descriptions of Django web‑application ideas.  
It returns a structured list of feedback items covering:

* Feasibility of the proposed feature  
* Typical implementation challenges  
* Recommended best practices for a Django‑centric solution  

The package lets you get a quick sanity‑check on a Django concept before you start writing code.

---

## Installation

```bash
pip install django_idea_analyzer
```

---

## Quick start

```python
from django_idea_analyzer import django_idea_analyzer

# A short description of the idea you want to evaluate
idea = """
I want a blogging platform where users can write posts,
add tags, and have a realtime comment section powered by websockets.
"""

# Use the default LLM7 backend (API key taken from LLM7_API_KEY env var)
feedback = django_idea_analyzer(user_input=idea)

print(feedback)
```

Typical output (list of strings):

```
[
  "The core blog model is straightforward in Django and can be built with the standard ORM.",
  "Using Django Channels for realtime comments is feasible, but you need to configure a channel layer (e.g., Redis).",
  "Tagging can be implemented with a ManyToMany field or a dedicated package like django‑tag‑git.",
  "Consider adding pagination and caching for performance on large comment streams.",
  "Make sure to handle authentication and permissions for comment creation."
]
```

---

## API reference

```python
django_idea_analyzer(
    user_input: str,
    llm: Optional[BaseChatModel] = None,
    api_key: Optional[str] = None
) -> List[str]
```

| Parameter   | Type                     | Description |
|-------------|--------------------------|-------------|
| **user_input** | `str` | The textual description of the Django feature or project idea you want to analyze. |
| **llm**        | `Optional[BaseChatModel]` | A LangChain LLM instance. If omitted, the function creates a `ChatLLM7` instance automatically. |
| **api_key**    | `Optional[str]` | API key for LLM7. If not supplied, the function reads `LLM7_API_KEY` from the environment. |

The function returns a list of feedback strings extracted from the LLM response.

---

## Using a custom LLM

You can plug any LangChain‑compatible chat model instead of the default LLM7. This is handy if you prefer OpenAI, Anthropic, Google Gemini, or a self‑hosted model.

### OpenAI

```python
from langchain_openai import ChatOpenAI
from django_idea_analyzer import django_idea_analyzer

my_llm = ChatOpenAI(model="gpt-4o-mini")
response = django_idea_analyzer(user_input=idea, llm=my_llm)
```

### Anthropic

```python
from langchain_anthropic import ChatAnthropic
from django_idea_analyzer import django_idea_analyzer

my_llm = ChatAnthropic(model="claude-3-haiku-20240307")
response = django_idea_analyzer(user_input=idea, llm=my_llm)
```

### Google Generative AI

```python
from langchain_google_genai import ChatGoogleGenerativeAI
from django_idea_analyzer import django_idea_analyzer

my_llm = ChatGoogleGenerativeAI(model="gemini-1.5-flash")
response = django_idea_analyzer(user_input=idea, llm=my_llm)
```

---

## LLM7 default configuration

* **Package:** `langchain_llm7` – [`pip install langchain-llm7`](https://pypi.org/project/langchain-llm7/)  
* **Default model:** The free tier of LLM7 provides generous limits suitable for most development and testing scenarios.  
* **Obtaining an API key:** Register at <https://token.llm7.io/> to receive a free key.  
* **Overriding the key:** Pass it directly via the `api_key` argument or set the environment variable `LLM7_API_KEY`.

```bash
export LLM7_API_KEY="your-llm7-api-key"
```

---

## Contributing & Support

* **Issue tracker:** <https://github.com/chigwell/django_idea_analyzer/issues>
* **Author:** Eugene Evstafev  
* **Email:** hi@euegne.plus  
* **GitHub:** <https://github.com/chigwell>

Feel free to open issues, submit pull requests, or ask questions. Happy coding!

--- 

## License

This project is licensed under the MIT License. See the `LICENSE` file for details.