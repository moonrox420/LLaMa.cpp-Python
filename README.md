# LLaMa.cpp-Python for Windows (RTX 50-series / Blackwell GPUs)

Pre-configured setup for running llama-cpp-python with CUDA support on Windows, specifically tested with **NVIDIA RTX 5070** and other Blackwell architecture GPUs.

## The Problem

As of early 2026, the official llama-cpp-python pre-built CUDA wheels for Windows don't support the newest GPU architectures (compute capability 12.0+). Building from source often fails or produces binaries without proper CUDA support.

## The Solution

Use **llama-cpp-python v0.3.4** which has pre-built Windows CUDA 12.4 wheels that work with newer GPUs.

## Quick Install

```powershell
# Create virtual environment (optional but recommended)
python -m venv venv
.\venv\Scripts\Activate.ps1

# Install llama-cpp-python with CUDA support
pip install llama-cpp-python==0.3.4 --extra-index-url https://abetlen.github.io/llama-cpp-python/whl/cu124
```

## Verify Installation

```python
import llama_cpp
print('Version:', llama_cpp.__version__)
info = llama_cpp.llama_print_system_info()
print(info.decode() if isinstance(info, bytes) else info)
```

You should see output like:
```
Version: 0.3.4
ggml_cuda_init: GGML_CUDA_FORCE_MMQ:    yes
ggml_cuda_init: found 1 CUDA devices:
  Device 0: NVIDIA GeForce RTX 5070 Laptop GPU, compute capability 12.0, VMM: yes
```

## Requirements

- Windows 10/11
- Python 3.10, 3.11, or 3.12
- NVIDIA GPU with CUDA support
- CUDA Toolkit 12.4+ installed

## Why v0.3.4?

Later versions (0.3.5+) don't have pre-built Windows CUDA wheels on the extra-index-url. Version 0.3.4 is the latest with Windows + Python 3.12 + CUDA 12.4 support as pre-built wheels.

## Usage Example

```python
from llama_cpp import Llama

# Load a GGUF model (download separately)
llm = Llama(
    model_path="path/to/your/model.gguf",
    n_gpu_layers=-1,  # Use all GPU layers
    n_ctx=4096,       # Context size
)

# Generate text
output = llm("Hello, how are you?", max_tokens=100)
print(output['choices'][0]['text'])
```

## Tested Hardware

- ✅ NVIDIA GeForce RTX 5070 Laptop GPU (compute capability 12.0)
- Should work with RTX 40-series and RTX 50-series GPUs

## Troubleshooting

### "function 'llama_sampler_init_softmax' not found"
You installed a version that's too new. Use v0.3.4:
```powershell
pip uninstall llama_cpp_python -y
pip install llama-cpp-python==0.3.4 --extra-index-url https://abetlen.github.io/llama-cpp-python/whl/cu124
```

### Built from source instead of using wheel
Clear pip cache and force download:
```powershell
pip cache purge
pip install llama-cpp-python==0.3.4 --extra-index-url https://abetlen.github.io/llama-cpp-python/whl/cu124 --no-cache-dir
```

The wheel should be ~443 MB. If it's only ~7 MB, it built from source without CUDA.

## License

MIT
