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