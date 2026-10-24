- Ai app 배포 및 운영에 필요한 방식들 
	- Docker Model Runner(preferred)
	- Container
- Both methods run AI apps locally, making them suitable for companies with privacy concerns.
- 동의어 
	- LLMs == models == AI models == AI apps 

## Docker model Runner background 
- Docker Model Runner(DMR) is a new technology, fully integrated with the Docker toolchain, that executes AI models directly on **host machines** rather than inside containers. 

## Docker model Runner Architecture 
- DMR is a host process separate from the Docker Engine. 
- Docker Engine과 별개의 프로세스로 실행될 수 있음 
- `llama.cpp`등의 Runtime을 통해 실제 모델 추론을 수행 
- 모델 관리 API와 OpenAI호환 추론 API를 제공 
- Docker CLI의 `docker model`명령어로 관련 기능 사용 가능 

### Docker Model Runner vs Ollama vs LM studio 
- all tools can use `llama.cpp` as their core inference engine and can expose models via OpenAi- style endpoints 
	- this means all offer similar core functionality & performance. 
- But, Docker model offers seamless integration with Docker tools and the wider cloud native ecosystem. 

## Installing Docker Model Runner 
- 실행 방법 
	1. Docker Desktop의 설정에서 docker model runner를 활성화 
	2. 실행 확인 
		- `docker model status`
## Explore Docker Model Runner 

### Pull Models from Docker Hub
- 과정 
	1. `docker model pull ai/gemma3:4B-Q4_k_m`
		- download a quantized version of a Gemma3 4B model 
		- Quantization is a way to reduce model sizw without losing too much model accuracy of performance. 

### inspecting models 
- DMR automatically pulls images from Docker Hub where it stores and distributes them as a new type of OCI artifact called a model. 

### Testing your model 
- DMR offers two ways to test your models 
	- The CLI
		- very basic testing capabilities with Zero conversational history 
	- Docker Desktop UI 

### Inspect the DMR API
- Docker Model Runner exposes a set of native endpoints for model management and a set of OpenAi-compatible endpoints for model interaction. 
- DMR은 모델 관리용 API와 모델 추론용 OpenAI호환 API를 분리해 제공한다. 

## Use Docker Model Runner with compose 
- the app is a multi-tier application with three services
	- frontend: Chat interface
	- Backend: Backend API
	- dmr: Model Server(DMR)

## Use Docker Model Runner with Open WebUI
- 과정 
	1. check DMR status & pull models 
	2. Install Open WebUI as a container 
		- Open WebUI is a popular AI platform that integrates with Ollama and OpenAI-compatible model serves to create powerful chatbot experiences. 
	3. Connect to Open WebUI and use it. 
		- Open WebUI is a powerful tool , and encourage you to investigate its feature 

### Running models in containers 
- no longer recommanded 