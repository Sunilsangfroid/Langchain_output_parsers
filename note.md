# Langchain Output Parsers

Output parsers in Langchain help convert raw LLM responses into structured formats like JSON, CSV, Pydantic models, and more. They ensure consistency, validation, and ease of use in applications.

## Overview

- Can be used for both can and can't LLM models
- Four important output parsers:
  1. String Output Parser
  2. JSON Output Parser
  3. Structured Output Parser
  4. Pydantic Output Parser
- Additional parsers include CSV and List output parsers

## 1. String Output Parser

The simplest output parser in Langchain. It is used to parse the output of a Language Model (LLM) and return it as a plain string.

### Example:

```python
from langchain_core.output_parsers import StrOutputParser
from langchain_core.prompts import PromptTemplate
from langchain_google_genai import ChatGoogleGenerativeAI

# Initialize parser
parser = StrOutputParser()

# Create template
template = PromptTemplate(
    template="Write a short poem about {topic}.",
    input_variables=["topic"]
)

# Setup chain
model = ChatGoogleGenerativeAI(model="gemini-1.5-pro")
chain = template | model | parser

# Execute chain
result = chain.invoke({"topic": "artificial intelligence"})
print(result)  # Prints the string result directly
```

## 2. JSON Output Parser

Forces the LLM to give its output in JSON format. This is the quickest possible way to get structured data, but it doesn't enforce schema.

### Example:

```python
from langchain_core.output_parsers import JsonOutputParser
from langchain_core.prompts import PromptTemplate
from langchain_google_genai import ChatGoogleGenerativeAI

# Initialize parser
parser = JsonOutputParser()

# Create template with format instructions
template = PromptTemplate(
    template="Generate information about {topic}.\n{format_instruction}",
    input_variables=["topic"],
    partial_variables={"format_instruction": parser.get_format_instructions()}
)

# Setup chain
model = ChatGoogleGenerativeAI(model="gemini-1.5-pro")
chain = template | model | parser

# Execute chain
result = chain.invoke({"topic": "black holes"})
print(result)  # Returns parsed JSON as Python dictionary
```

## 3. Structured Output Parser

An output parser that helps extract structured JSON data from LLM responses based on predefined field schemas. It works by defining a list of fields (ResponseSchema) that the model should return.

### Key Features:
- Options available for enforcing schema
- Cannot perform data validation

### Example:

```python
from langchain_core.output_parsers import ResponseSchema, StructuredOutputParser
from langchain_core.prompts import PromptTemplate
from langchain_google_genai import ChatGoogleGenerativeAI

# Define the structure
response_schemas = [
    ResponseSchema(name="name", description="The name of the person"),
    ResponseSchema(name="age", description="The age of the person"),
    ResponseSchema(name="city", description="The city where the person lives")
]

# Initialize parser
parser = StructuredOutputParser.from_response_schemas(response_schemas)

# Create template
template = PromptTemplate(
    template="Generate information about a fictional person.\n{format_instruction}",
    input_variables=[],
    partial_variables={"format_instruction": parser.get_format_instructions()}
)

# Setup chain
model = ChatGoogleGenerativeAI(model="gemini-1.5-pro")
chain = template | model | parser

# Execute chain
result = chain.invoke({})
print(result)  # Returns structured data according to schema
```

## 4. Pydantic Output Parser

A structured output parser that uses Pydantic models to enforce schema validation when processing LLM responses.

### Why Use PydanticOutputParser?
- **Strict Schema Enforcement**: Ensures LLM responses follow a well-defined structure
- **Type Safety**: Automatically converts LLM outputs into Python objects
- **Easy Validation**: Uses Pydantic's built-in validation to catch incorrect or missing data
- **Seamless Integration**: Works well with other Langchain components

### Example:

```python
from langchain_core.output_parsers import PydanticOutputParser
from langchain_core.prompts import PromptTemplate
from langchain_google_genai import ChatGoogleGenerativeAI
from pydantic import BaseModel, Field
from typing import List

# Define Pydantic model
class Person(BaseModel):
    name: str = Field(description="The person's name")
    age: int = Field(description="The person's age")
    hobbies: List[str] = Field(description="The person's hobbies")

# Initialize parser
parser = PydanticOutputParser(pydantic_object=Person)

# Create template
template = PromptTemplate(
    template="Generate information about a fictional person.\n{format_instruction}",
    input_variables=[],
    partial_variables={"format_instruction": parser.get_format_instructions()}
)

# Setup chain
model = ChatGoogleGenerativeAI(model="gemini-1.5-pro")
chain = template | model | parser

# Execute chain
result = chain.invoke({})
print(result)  # Returns a Pydantic Person object
```

## Other Parsers

Langchain also provides CSV and List output parsers for specific formatting needs, using similar implementation patterns to those shown above.