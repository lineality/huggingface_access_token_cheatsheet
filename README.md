# huggingface_access_token_cheatsheet

## add "token" (auth-token) to BOTH the model AND the tokenizer.
Otherwise, tokenizer error will be vague and not say that you have to use an auth-token:
```error message
OSError: Can't load tokenizer for 'mistralai/Mistral-7B-Instruct-v0.2'.
If you were trying to load it from 'https://huggingface.co/models',
make sure you don't have a local directory with the same name.
Otherwise, make sure 'mistralai/Mistral-7B-Instruct-v0.2' is
the correct path to a directory containing all relevant files
for a LlamaTokenizerFast tokenizer.
```

see: 
https://huggingface.co/docs/hub/security-tokens

```python
from transformers import AutoModel
access_token = ...
model = AutoModel.from_pretrained("NAME_OF_HUGGINFACETHING/NAME_OF_MODEL", token=access_token)
```

# Use environment variable
As a rule of thumb, always, even for local tinkering and R&D, use ~more-secure environment variables
and do not ever hard-code a key, auth, password, etc., into a file

# Method 1: Adding to 'from_...' line:
## Example using google colab:
```python
from google.colab import userdata
hugging_face_auth_access_token = userdata.get('hugging_face_auth')

"""
not this:
model = AutoModelForCausalLM.from_pretrained("mistralai/Mistral-7B-Instruct-v0.2")
tokenizer = AutoTokenizer.from_pretrained("mistralai/Mistral-7B-Instruct-v0.2")

use this:
tokenizer = AutoTokenizer.from_pretrained("mistralai/Mistral-7B-Instruct-v0.2", token=hugging_face_auth_access_token)
model = AutoModelForCausalLM.from_pretrained("mistralai/Mistral-7B-Instruct-v0.2", token=hugging_face_auth_access_token)

"""

from transformers import AutoModelForCausalLM, AutoTokenizer
import torch

device = "cuda"  # the device to load the model onto

# Load the model and tokenizer
model = AutoModelForCausalLM.from_pretrained("mistralai/Mistral-7B-Instruct-v0.2", token=hugging_face_auth_access_token)
tokenizer = AutoTokenizer.from_pretrained("mistralai/Mistral-7B-Instruct-v0.2", token=hugging_face_auth_access_token)

etc.
```
## Example using .env and python-dotenv
example of .env file:
```
hugging_face_auth="082g0hwg0uhg0hsg2h-832t8-3whgwhe"
```

install library with:
```
pip install python-dotenv
```
```python
"""
Python Dot-env
"""
from dotenv import load_dotenv  # (pip install python-dotenv)
import os

load_dotenv()
api_key = os.getenv("hugging_face_auth")

```

# Method 2: Using 'from huggingface_hub import login'
This may show a 'warning' saying a token is not found.
```python
# get your value from whatever environment-variable config system (e.g. python dot-env, or yaml, or toml)
from google.colab import userdata
hugging_face_auth_access_token = userdata.get('hugging_face_auth')

# put that auth-value into the huggingface login function
from huggingface_hub import login
login(token=hugging_face_auth_access_token)
```
