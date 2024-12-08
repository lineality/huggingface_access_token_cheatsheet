# huggingface_access_token_cheatsheet

see: 
https://huggingface.co/docs/hub/security-tokens

```python
from transformers import AutoModel
access_token = "hf_..."
model = AutoModel.from_pretrained("private/model", token=access_token)
```

# Use environment variable
As a rule of thumb, always, even for local tinkering and R&D, use ~more-secure environment variables
and do not ever hard-code a key, auth, password, etc., into a file

## Example using google colab:
```python
from google.colab import userdata
hugging_face_auth_access_token = userdata.get('hugging_face_auth')

"""
not this:
model = AutoModelForCausalLM.from_pretrained("mistralai/Mistral-7B-Instruct-v0.2")

use this:
model = AutoModelForCausalLM.from_pretrained("mistralai/Mistral-7B-Instruct-v0.2", token=hugging_face_auth_access_token)
"""

from transformers import AutoModelForCausalLM, AutoTokenizer
import torch

device = "cuda"  # the device to load the model onto

# Load the model and tokenizer
model = AutoModelForCausalLM.from_pretrained("mistralai/Mistral-7B-Instruct-v0.2", token=hugging_face_auth_access_token)
tokenizer = AutoTokenizer.from_pretrained("mistralai/Mistral-7B-Instruct-v0.2")

etc.
```


## Example using .env and python-dotenv

