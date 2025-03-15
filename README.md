# Langchain Output Parsers

A repository demonstrating various output parsers in Langchain to structure LLM responses.

## Setup Instructions

Follow these steps to get started with the examples:

```bash
# 1. Clone the repository (if you haven't already)
git clone https://github.com/sunilsangfroid/Langchain_output_parsers.git
cd Langchain_output_parsers

# 2. Create a virtual environment
python -m venv venv

# 3. Activate the virtual environment
# On Windows:
venv\Scripts\activate
# On macOS/Linux:
# source venv/bin/activate

# 4. Install required packages
pip install langchain langchain-google-genai pydantic python-dotenv

# 5. Create a .env file with your API key
echo GOOGLE_API_KEY=your_api_key_here > .env

# 6. Run the examples
# String Output Parser
python [stroutputparser.py](http://_vscodecontentref_/0)

# JSON Output Parser
python [jsonoutputparser.py](http://_vscodecontentref_/1)

# Structured Output Parser
python [structuredoutputparser.py](http://_vscodecontentref_/2)

# Pydantic Output Parser
python [pydanticoutputparser.py](http://_vscodecontentref_/3)

# 7. To create your own examples
# Create a new file
echo "from langchain_core.output_parsers import StrOutputParser
from langchain_core.prompts import PromptTemplate
from langchain_google_genai import ChatGoogleGenerativeAI
import os
from dotenv import load_dotenv

# Load environment variables
load_dotenv()
google_api_key = os.getenv('GOOGLE_API_KEY')

# Initialize model and parser
model = ChatGoogleGenerativeAI(model='gemini-1.5-pro', api_key=google_api_key)
parser = StrOutputParser()

# Create a prompt template
template = PromptTemplate(
    template='Write about {topic}',
    input_variables=['topic']
)

# Create and run the chain
chain = template | model | parser
result = chain.invoke({'topic': 'artificial intelligence'})
print(result)" > myexample.py

# 8. Run your custom example
python myexample.py

# 9. Modify for different parsers
# For JSON Parser:
# Replace StrOutputParser with JsonOutputParser
# Add format_instruction to the template

# For Structured Parser:
# Use ResponseSchema and StructuredOutputParser
# Define the fields you want to extract

# For Pydantic Parser:
# Create a Pydantic model class
# Use PydanticOutputParser with your model

# 10. Deactivate virtual environment when done
deactivate