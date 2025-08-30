# Optimizing LLMs Using Quantization for Mobile Execution

This repository contains the research, implementation, and deployment workflow for compressing Large Language Models (LLMs) to run efficiently on mobile devices.  
The project focuses on **Post-Training Quantization (PTQ)** of the **Llama 3.2B** model, conversion into **GGUF format**, and successful **on-device inference on Android** using Termux + Ollama.

---

## Abstract

Large Language Models (LLMs) are powerful but resource-hungry, making direct mobile deployment infeasible.  
This project investigates **4-bit Post-Training Quantization (PTQ)** as a compression technique for Llama 3.2B:

- Applied **nf4 quantization** using [BitsAndBytes](https://github.com/TimDettmers/bitsandbytes) within the Hugging Face Transformers framework.  
- Converted the quantized model to **GGUF** format via [llama.cpp](https://github.com/ggerganov/llama.cpp).  
- Deployed the final model on an **Android smartphone (OnePlus Nord CE 5G, Snapdragon 750G, 12GB RAM)** using Termux and [Ollama](https://ollama.com/).  

The result:  
- Model size reduced from **6.0 GB → 1.88 GB (68.66% reduction)** while maintaining competitive performance.  
- Demonstrated **offline, private, on-device inference** with coherent results.

---

## Workflow

**1. Model Acquisition**  
- Base model: Llama 3.2B (BF16 precision) from Hugging Face Hub.  

**2. Quantization**  
- Used `BitsAndBytes` with **nf4 4-bit PTQ** to compress weights.  

**3. GGUF Conversion**  
- Converted model to `q4_k_m` GGUF variant using `llama.cpp` tools.  

**4. Mobile Deployment**  
- Packaged model as `.gguf` + ZIP archive.  
- Deployed on Android with **Termux + Ollama** for local inference.  

**Workflow Summary:**  
`Hugging Face Transformers → BitsAndBytes (nf4) → llama.cpp (GGUF q4_k_m) → Termux/Ollama → Android Deployment`

---

##  Results

### 🔹 Model Size Reduction
| Model Format               | Size (GB) | Reduction (%) |
|-----------------------------|-----------|---------------|
| Original Llama 3.2B (BF16)  | 6.00      | –             |
| 4-bit Quantized (nf4)       | 2.10      | 64.92%        |
| Final GGUF q4_k_m           | 1.88      | 68.66%        |

### 🔹 Performance (MMLU Benchmark)
| Model                     | Size (GB) | Params | MMLU Score (%) |
|----------------------------|-----------|--------|----------------|
| Llama 3.2B (Original)      | 6.00      | 3B     | 64.2           |
| Llama 3.2B (GGUF q4_k_m)   | 1.88      | 3B     | 61.8           |
| Phi-2                      | 2.70      | 2.7B   | 68.8           |
| Gemma 2B                   | 4.00      | 2B     | 64.3           |
| Mistral 7B (GGUF q4_k_m)   | 4.37      | 7B     | 62.5           |
| TinyLlama 1.1B             | 2.20      | 1.1B   | 54.9           |

**Key Takeaway:** Despite compression, the quantized model retained **96% of baseline accuracy** with drastically reduced memory footprint.

---

##  Mobile Deployment

- Device: OnePlus Nord CE 5G (Snapdragon 750G, 12GB RAM, Android 13)  
- Environment: Termux + Ollama  

### Steps:
```bash
# Install Termux dependencies
pkg update && pkg upgrade
pkg install git cmake clang

# Install Ollama
curl -fsSL https://ollama.com/install.sh | sh

# Run the quantized model
ollama run llama3-gguf
