Local AI Stack: Open WebUI + Kokoro TTS + SearXNG + Stable Diffusion
This repository provides a production-ready Docker Compose configuration to deploy a private, local AI ecosystem. It integrates a sophisticated web interface with LLM capabilities, real-time web search, high-fidelity TTS, and AI image generation.

🚀 Project Architecture
The stack is designed for power users, utilizing an optimized multi-container architecture that separates heavy inference workloads from service management.
Frontend: Open WebUI - The central hub for chat, RAG, and media management.
Image Generation: Stable Diffusion (Automatic1111) - High-performance image synthesis via API.
TTS Engine: Kokoro TTS - Fast, natural GPU-accelerated speech.
Web Search: SearXNG - Anonymous, real-time internet access for the LLM.
Reverse Proxy: NGINX with SSL termination for secure local/remote access.
Inference Backend: External connection to llama.cpp (OpenAI API compatible).

💻 Hardware & Environment
The stack is fine-tuned for high-end local hardware:
OS: Ubuntu on WSL2.
GPU: NVIDIA RTX 3080 Ti (12GB VRAM) - Utilized for Kokoro TTS and Stable Diffusion.
System RAM: 32GB (Optimized for context windows up to 24k tokens).
Connectivity: Cross-stack communication via host.docker.internal.

🛠️ Setup & Installation
1. Prerequisites
Docker & Docker Compose installed.
NVIDIA Container Toolkit configured for GPU pass-through.
A running instance of llama.cpp on port 8080.
A running instance of Stable Diffusion (Automatic1111) on port 7860 with the --api flag active.

🛠️ Installation & Setup
To get the full ecosystem running, you need to deploy two separate projects: the Stable Diffusion Backend and the LocalAIStack-llama.cpp.
1. Install Stable Diffusion (Backend)
First, you must set up the image generation engine. Clone and deploy the repository specifically configured for SDXL:
git clone https://github.com/cuccurese2010/stable-diffusion-xl-docker-setup.git
cd stable-diffusion-xl-docker-setup
docker compose --profile download up --build
# wait until its done, then:
docker compose --profile auto up --build
# Web address
http://localhost:7860
# Stable Diffusion Settings
Txt2img
Sampling steps=35
Hires fix=512x512
Start up the container:
docker compose --profile auto up
Shutdown the container:
docker compose --profile auto down


2. Install Local AI Stack (Frontend & Services)
Now, clone and deploy this project to set up Open WebUI, Kokoro TTS, and SearXNG:
git clone https://github.com/cuccurese2010/Local-AI-Stack-llama.cpp
cd Local-AI-Stack-llama.cpp
docker compose up -d
3. Access the Interface
Once both stacks are running, open your browser and navigate to:
URL: https://localhost

Note: The interface is served over HTTPS via the NGINX proxy. You may need to accept the self-signed certificate warning upon first access.
⚙️ How it works
The Local AI Stack is pre-configured to communicate with the Stable Diffusion Stack using the host.docker.internal gateway.

Image API: http://host.docker.internal:7860/sdapi/v1

LLM API: Ensure your llama.cpp is running on the host at port 8080.
