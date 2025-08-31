# Optimizing LLMs Using Quantization for Mobile Execution

This repository contains the research, implementation, and deployment workflow for compressing Large Language Models (LLMs) to run efficiently on mobile devices.  
The project focuses on **Post-Training Quantization (PTQ)** of the **Llama 3.2B** model, conversion into **GGUF format**, and successful **on-device inference on Android** using Termux + Ollama.

---

## Abstract

Large Language Models (LLMs) are powerful but resource-hungry, making direct mobile deployment infeasible.  
This project investigates **4-bit Post-Training Quantization (PTQ)** as a compression technique for Llama 3.2B:

- Applied nf4 quantization using [BitsAndBytes](https://github.com/TimDettmers/bitsandbytes) within the Hugging Face Transformers framework.  
- Converted the quantized model to GGUF format via [llama.cpp](https://github.com/ggerganov/llama.cpp).  
- Deployed the final model on an Android smartphone (OnePlus Nord CE 5G, Snapdragon 750G, 12GB RAM) using Termux and [Ollama](https://ollama.com/).  

The result:  
- Model size reduced from 6.0 GB to 1.88 GB (68.66% reduction).  
- Demonstrated offline, private, on-device inference with coherent results.

---

## Workflow

1. **Model Acquisition**  
   - Base model: Llama 3.2B (BF16 precision) from Hugging Face Hub.  

2. **Quantization**  
   - Used BitsAndBytes with nf4 4-bit PTQ to compress weights.  

3. **GGUF Conversion**  
   - Converted model to q4_k_m GGUF variant using llama.cpp tools.  

4. **Mobile Deployment**  
   - Packaged model as .gguf + ZIP archive.  
   - Deployed on Android with Termux + Ollama for local inference.  

**Workflow Summary:**  
Hugging Face Transformers → BitsAndBytes (nf4) → llama.cpp (GGUF q4_k_m) → Termux/Ollama → Android Deployment

---

## Results

### Model Size Reduction
| Model Format               | Size (GB) | Reduction (%) |
|-----------------------------|-----------|---------------|
| Original Llama 3.2B (BF16)  | 6.00      | –             |
| 4-bit Quantized (nf4)       | 2.10      | 64.92%        |
| Final GGUF q4_k_m           | 1.88      | 68.66%        |

### Performance (MMLU Benchmark)
| Model                     | Size (GB) | Params | MMLU Score (%) |
|----------------------------|-----------|--------|----------------|
| Llama 3.2B (Original)      | 6.00      | 3B     | 64.2           |
| Llama 3.2B (GGUF q4_k_m)   | 1.88      | 3B     | 61.8           |
| Phi-2                      | 2.70      | 2.7B   | 68.8           |
| Gemma 2B                   | 4.00      | 2B     | 64.3           |
| Mistral 7B (GGUF q4_k_m)   | 4.37      | 7B     | 62.5           |
| TinyLlama 1.1B             | 2.20      | 1.1B   | 54.9           |

Key Takeaway: Despite compression, the quantized model retained about 96% of baseline accuracy with drastically reduced memory footprint.

---

## Mobile Deployment

- Device: OnePlus Nord CE 5G (Snapdragon 750G, 12GB RAM, Android 13)  
- Environment: Termux + Ollama  

### Steps
```bash
# Install Termux dependencies
pkg update && pkg upgrade
pkg install git cmake clang

# Install Ollama
curl -fsSL https://ollama.com/install.sh | sh

# Run the quantized model
ollama run llama3-gguf
```

- RAM usage during inference: ~2.3 GB  
- Prompt latency: ~2.5 sec / 20 tokens  
- Capabilities: Fully offline, private inference  

Screenshots and logs from mobile inference are provided in the `results/screenshots/` folder.

---

## Repository Structure
```
llm-quantization-mobile/
├── notebooks/
│   └── quantization_nf4.ipynb      # Colab notebook (quantization workflow)
├── results/
│   ├── screenshots/                # Mobile inference screenshots
│   └── report_summary.pdf          # Summary of paper/report
├── model/                          # (optional) GGUF quantized model
├── README.md
└── requirements.txt
```

---

## Research Context

This work was conducted as part of:  
- Conference Paper: *Optimizing LLMs Using Quantization for Mobile Execution* (2025, DOI pending)  
- Capstone Project Report: B.Tech Thesis, VIT Chennai  

Supervised by: Dr. Renta Chintala Bhargavi  
Author: Agatsya Yadav (21BCE1902)  

---

## Future Work

- Benchmark more quantization techniques (GPTQ, AWQ, SmoothQuant).  
- Profile real-time performance (tokens/sec, power usage).  
- Native mobile app deployment (Android/iOS) with GUI.  
- Explore hybrid methods: quantization + pruning or distillation.  

---

## References

- Meta AI: Llama 3 (2023)  
- Dettmers et al., QLoRA: Efficient Finetuning of Quantized LLMs (2023)  
- Frantar et al., GPTQ: Accurate Post-Training Quantization for Transformers (2022)  
- [BitsAndBytes](https://github.com/TimDettmers/bitsandbytes)  
- [llama.cpp](https://github.com/ggerganov/llama.cpp)  
- [Ollama](https://ollama.com)  

---

## Citation

If you use this workflow or findings, please cite:

```
Yadav, A., & Bhargavi, R.C. (2025).
Optimizing LLMs Using Quantization for Mobile Execution.
Presented at ICT Goa 2025, DOI pending.
```

---
## Contact

* **Author:** Agatsya Yadav
* **Email:** <mailto:20agatsyadav03@gmail.com>
* **LinkedIn:** <https://linkedin.com/in/agatsya-yadav-3ab612299>
* **GitHub:** <https://github.com/Agatsyadav2003>

---
