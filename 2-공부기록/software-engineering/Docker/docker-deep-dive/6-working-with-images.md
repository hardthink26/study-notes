## Docker images 
- all of following terms mean the same thing, and use them interchangeably: 
	- **Image, Docker Image, container image, and OCI image** 
- def. a read-only package containing everything you need to run an application.

- The easiest way to get an image is to pull one from a registry. 
	- **Docker Hub** is the most common registry, and pulling an image downloads it to your local machine where Docker can use it to start one or more containers 
- Docker creates images by stacking independent layers and representing them as a single unified object. 
	- one layer might have the OS components 
	- another layer might have application dependencies... 

## Intro to images 
- images are **buildtime constructs**, whereas containers are **run-time** constructs
- `docker run` command 
	- the most common way to start a container from an image. 
	- once the container is running, the image and container are bound, and can't delete the image until stop and delete the container. 
- container
	- designed to run a single application or microservice. 

## Pulling images 
- A clean Docker installation has an empty local repository 
	- Local repository is jargon for an area on local machine where Docker stores images for more convenient access call it **image cache** 
- the process of getting images is called **pulling** 

## Image registries 
- We store images in centralized places called **registries**
- 역할 
	- securely store images 
	- make them easy to access from different environments.
- 구성 
	- Image registries contain one or more image repositories, and image repositories contain one or more images. 

### Official repositories 
- Docker Hub has the concept of **official repositories** that are home to images vetted and curated by Docker and the application vendor 
- 어떻게 확인?
	- They live at the top level of the Docker Hub namespace and have a green Docker Official image badge. 
### Unofficial repositories
- **you should always start with the assumption that anything from an unofficial repository is unsafe** 

## Image naming and tagging 
- you can learn a lot about an image from its name. 
- **구성** 
	- Registry + User/org + Repository + Image/Tag 
	- sometimes call the image name the **tag** 
- `docker pull` command format (in official repository)
	- `% docker pull <repository>:<tag>` 
- 알아두면 좋을 것 
		- 따로 image tag지정하지 않을 시 Docker가 알아서 lmage tagged as `latest`찾음 
		- Images tagged as `latest` aren't guaranteed to be the most up-to-date in the repository. 
- `docker pull` command format (in unofficial repository) 
	- same as pulling from official repositories but add a Docker Hub username or organization name before the repository name. 
- 만약 다른 registry서 pull을 한다면? 
	- add the registry's DNS name before the repository name. 

### Images with multiple tags 
- you can give a single image as many tags as you want. 

## Images and Layers
- Images are a collection of loosely connected read-only layers where each layer compries one or more files. 

- to inspect layer information 
	- Pull operations 
		- Each line ending with `Pull complete ` represents a layer that Docker pulled
	- the `docker inspect` command 
		- The trimmed output shows the layers. it shows their SHA256 hashes, which are different from the start IDs in the `docker pull` output 
	- the `docker history` command 
		- this command shows the build history of an image and is not a strict list of layers in the final image. 
### Base layers 
- All Docker images start with a base layer, and every time add new content, Docker adds a new layer. 
- **image is the combination of all layers stacked in the order they were built.** 
- Dockers users **storage drivers** to stack layers and persent them as a unified filesystem and image. 
	- Almost all Docker setups use the overlay2 driver. 

### Sharing image layers 
- Images can share layers, leading to efficiencies in space and performance. 
	- Layers are shared on the registry side, and the registry will save space by never storing more than a single copy of any layer. 


## Pulling images by digest 
- **image Digest**
	-  1. 등장배경 
		- Docker의 태그는 변경 가능한 참조이므로 동일한 태크가 서로 다른 이미지를 가리킬 수 있다. 
		- 따라서 태그만으로 컨테이너가 어떤 버전의 이미지를 사용하는지 정확히 식별 어렵다. 
		- 이를 해결하기 위해 Image Digest등장 
	- 2. 핵심원리 
		- Docker는 Content-addressable Storage(콘텐츠 주소 지정 저장)model을 사용 
			- Digest: 이미지 콘텐츠를 식별하는 암호학적 해시값 
			- 이미지와 관련 콘텐츠가 변경되면 새로운 Digest생성 
			- 동일한 Digest는 동일 콘텐츠 지정 
			- 따라서 태그가 변경되어도 Digest를 통해 특정 이미지를 정확히 참조 가능 
	- 3. 관련 명령어 
		- `docker images --digests alpine` 
			- --digests: 이미지의 digest표시 
		- `docker buildx imagetools inspect ...`
			- 만약 pull하기 전에 이미지의 digest를 찾는다면 사용 가능 
		- Docker HUB API통한 Digest 조회 
			- `curl "URL | jq '.results[].digest'`
				- curl: Docker Hub API에 HTTP요청을 보내 이미지 정보를 JSON으로 수신 
				- |:앞 명령어의 표준 출력을 다음 명령어의 표준 입력 전달
				- jq: JSON데이터를 조회하고 가공 
				- results[].digest: results 배열의 각 객체에서 digest필드를 추출한다. 
### Image hashes and layer hashes
- images are just a loose collection of independent layers 
	- that means an image is just a manifest file with some metadata and a list of layers 
- images and layers have their own digests 
	- Images digests are a cryto hash of the image's manifest file 
	- Layer digests are a cryto hash of the layer's contents 

### content hashes vs distribution hashes 
1. Docker는 push and pull 전후 해시값을 비교한다. 
2. 하지만 push and pull operation동안 data를 compress한다. 
	- to save network bandwidth 
	- to save storage space on the registry 
3. as a result, the before and after hashes won't match 

- each layer gets two hashes 
	- Content hash(uncompressed)
	- Distribution hash(compressed) 
- so, Every time Docker pushes or pulls a layer from a registry, it includes the layer's distribution hash and uses this to verify no tampering occurred 

## Multi-architecture images 
- 등장 배경 
	- as Docker get more complex, it started supporting different platforms and architecture .
	- developers and users had to put in significant extra work to get the right version. 
- 해결책 
	- Docker and the registry API adapted a way to hide images for multiple architecture behind a single tag. 
		- this means when you pull image on any architecture, you can get the correct version of the image. 
- **How** 
	- The Registry API supports two constructs 
		- Manifest lists 
		- Manifests 
			- lists the layers used to build it. 
- 관련 명령어 
	- `docker manifest`: get the information about manifest list 

- **Docker buildx**는 BuildKit 기반의 빌드 도구로, 서로 다른 CPU아키텍쳐를 위한 이미지를 생성할 수 있다. 
	- Emulation 
		- QEMU를 사용하여 서로 다른 CPU architecture의 명령어 실행을 에뮬레이션한다. 
		- 로컬 환경에서 다른 아키텍쳐용 이미지를 빌드 가능 
		- 에뮬레이션에 따른 성능 저하 발생 가능 
	- Docker Build Cloud 
		- 클라우드의 네이티브 하드웨어에서 이미지를 빌드 
		- 에뮬레이션이 필요치 않아 일반적으로 빠름 
		- 팀 간 빌드 캐시 공유 및 CI/CD 통합을 지원 
		- 유료 서비스일 수 있음 

## Vulnerability scanning with Docker Scout 
- tools and plugins exist that scan images for known vulnerabilities 
	- Docker Scout 
		- built into almost every level of Docker
		- require a paid subscription 
		- 관련 명령어 
			- `docker scout quickview`
				- to get a quick vulnerability overview of an image. 
				- output shows zero critical vulnerabilities(0C), one high(1H), one medium(1M), and zero low(0L) 
			- `docker scout cves` 
				- to get more detailed info, including advice. 

## Deleting Images 
- `docker rmi` image 
	- Deleting images from repository  
	- The operation deletes also all directories on local filesystem containing layer data.
	- Docker won't delete layers shared by multiple images until deleting all images that reference them. 
	- **사용법** 
		- delete images by name, short ID, or SHA, also delete multiple images with the same command. 
			- **주의**
				- 만약 여러 개 컨테이너가 한 이미지를 공유 중이라면? 
					- 일반적으로 삭제 불가 
					- 하지만, `-f` flag를 통해 강제 삭제 가능 
					- but, this will untag the image and leave it on the system as a **dangling(매달려 있는) image**.
		- 만약 이미지 싹 다 삭제하고 싶다면? 
			- `docker rmi $(docker images -q) -f`로 가능!!! 
				- pass a list of all local images IDS to the `docker rmi` command 

## Summary 
- `docker inspect`
	- give a wealth of image-related metadata in a nicely formatted view.
- `docker manifest` 
	- lets you inspect the manifest list of images stored in registries.
- `docker buildx`
	- A Docker CLI plugins that works with Docker's latest build engine features.
- `docker scout`
	- Docker plugin that integrates with the Docker Scout backend to perform image vulnerability scanning. 
	- it scans images, provides reports on vulnerabilities, and suggests remediation actions
- `docker rmi`
	- the command to delete images



