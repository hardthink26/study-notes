- Wasm(WebAssembly) is driving the Third wave of cloud computing. 
- **공부 목표**
	- write a simple Wasm application and use Docker to containerize & run it in a container. 

### Configure Docker Desktop for Wasm 
- docker desktop에서 Enable wasm 을 체크합니다. 

### Install Rust and configure for Wasm 
- 과정 
	1. rust를 설치합니다. 
	2. `rustup target add wasm32-wasipl`
### Install spin 
- Spin 
	- A Wasm framework and runtime that makes building and running Wasm apps easy. 

## Intro to Wasm & Wasm containers
- Wasm 
	- A new type of application that is smaller, faster, and more portable than traditional Linux containers. 
	- but, traditional Linux containers can do a lot more than Wasm apps 
	- A new virtual machine architecture that programming languages complies to. 
	- compile them to Wasm and they'll run on any system with a Wasm machine. 
	- Docker Desktop ships with several Wasm runtimes. 
- Wasm container
	- allow containerd to deploy and manage Wasm containers.
	- A Wasm binary running inside a minimal scratch container 

## Write a Wasm app 

## Containerize a Wasm app 
- Docker Desktop lets you containerize Wasm apps so you can use familiar Docker tools to push and pull them to OCI registries and run them inside containers. 

## Run a Wasm container 


--- 
## Summary 
- Docker Wasm 
	- 다양한 환경에서 실행할 수 있도록 설계된 이식성 높은 바이너리 명령어 형식
	- 브라우저뿐 아니라 서버에서도 Wasm runtime을 통해 실행 가능 
	- 샌드박스 기반 실행을 지원하며, 일반적인 컨테이너와는 실행 모델이 다르다. 
- Docker & Wasm 
	- Docker는 Wasm runtime 과 연동하여 Wasm 앱을 실행 가능하다. 
	- 일반적인 Linux container는 호스트 커널을 공유하며 프로세스를 실행하지만, Wasm모듈은 Wasm runtime 을 통해 실행된다. 
	- Docker는 containerd와 적절한 런타임 통합을 이용해 Wasm워크로드를 관리할 수 있다. 
- 활용 및 특징 
	- 이식성, 샌드박스 실행, 작은 배포 단위 등이 장점이 될 수 있다. 
	- 서버리스, 플러그인 시스템, 엣지 컴퓨팅 등에 활용할 수 있다. 
	- 단, 모든 워크로드에서 일반 컨테이너보다 효율적인 것은 아님 