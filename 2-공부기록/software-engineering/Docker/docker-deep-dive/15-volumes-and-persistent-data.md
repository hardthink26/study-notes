- There are two main types of data - persistent & non-persistent 
	- **persistent data**
		- the stuff that you care about and need to keep 
			- such as customer records, financial data, ... 
		- call applications that create & manage persistent data **stateful apps**
	- **non-persistent data**
		- the stuff that you don't care about and don't need to keep 
		- call applications that don't create or manage persistent data **stateless apps**
- For stateless apps 
	- Docker create every container with an area of non-persistent local storage that's tied to the container lifecycle. 
- For stateful apps 
	- has volumes that create & manage important data. 
	- **Volumes**
		- separate objects that mount into containers. 
		- have their own lifecycles. 

## Containers without Volumes 
- 초창기에, containers were only good for stateless applications that didn't generate important data. 
- 그래도, applications still needed a place to write temporary scratch data .
- 도커의 해결책 
	- Docker creates containers by stacking read-only image layers and placing a thin layer of local storage on top. 
	- but, the local storage is coupled to the container's lifecycle, meaning it gets created when you create the container, and delete when you delete it. 
- 언제 사용? 
	- if containers don't create persistent data, this thin writable layer of local storage will be fine 

## Containers with Volumes 
- persistent data를 다루는 데 volume을 써야하는 이유
	- volumes are independent objects that are not tied to the lifecycle of a container 
	- can map volumes to specialized external storage systems 
	- Multiple containers on different Docker hosts can use volumes to access & share the same data. 
- 개발 순서 
	- create a volume -> create a container - > finally mount the volume into the container. 
		- when mount volume into the container, you mount it into a directory in the container's filesystem. 

### Creating and managing Docker volumes
- Volumes are first-class objects in Docker. 
	- This means there's a docker volume sub-command, and a volume resource in the API. 