# Fixing Hugging Face Model Conversion Issues with Unsloth and Llama.cpp

## Overview
Many users have reported issues when trying to convert Hugging Face models to the GGUF format using **Unsloth** and **Llama.cpp**. The main problem occurs when **Llama.cpp fails to locate necessary files**, resulting in errors such as:

```
RuntimeError: *** Unsloth: Failed compiling llama.cpp using os.system(...) with error 32512. Please report this ASAP!
```

Another common error occurs when saving a loaded model to GGUF format using:

```python
model.save_pretrained_gguf("model_gguf", tokenizer)
```

This results in:

```
packages/unsloth/save.py:1054) if check != 0: raise RuntimeError("Failed compiling llama.cpp. Please report this ASAP!")

RuntimeError: Failed compiling llama.cpp. Please report this ASAP!
```

In short, **Llama.cpp or Unsloth were broken** during this process. To resolve these issues, this repository provides a **notebook** detailing the steps and all the necessary code to fix the errors.

---

## Fix: Properly Rebuilding Llama.cpp
Before converting the model, **one must rebuild Llama.cpp from scratch** using the following steps:

```bash
# Clone the Llama.cpp repository
git clone https://github.com/ggml-org/llama.cpp.git

# Navigate into the directory
cd llama.cpp

# Checkout a stable version (adjust as needed)
git checkout b3345

# Initialize submodules
git submodule update --init --recursive

# Clean previous builds
make clean

# Rebuild everything
make all -j

# Verify the last commit
git log -1
```

Once these steps are completed, you should be able to convert the model without encountering the **Llama.cpp or Unsloth errors**.

---

## Repository Contents
- **Fix Notebook**: A Jupyter notebook that provides detailed steps and explanations.
- **Example Code**: Scripts to help automate the conversion process.

## Contribution
If you encounter any further issues, feel free to open an issue or submit a pull request.

