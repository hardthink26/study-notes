- **goal**
	- how to deploy a multi-node Swarm cluster and how to run apps on it. 
- 현재 사용 빈도 
	- no longer core to modern Docker workflows,

## Swarm primer 

- Swarm is two things
	- A secure cluster of Docker nodes
	- An intelligent application orchestrator 
- a Swarm is a cluster of Docker nodes with one or more manager nodes and optional worker nodes.
	- managers run the control plane services that secure the cluster and provide the orchestration intelligence. 
	- Workers run user apps.
	- Both types of nodes can be physical machines, VMs, cloud instances ,... 

## Build a swarm 


---
## Summary 
- Docker Swarm 
	- 정의 
		- docker swarm은 여러 Docker 호스트를 하나의 클러스터로 구성하고, 컨테이너 기반 서비스를 배치·관리하는 오케스트레이션 기능
	- 핵심 구성 요소 
		- node: Swarm 클러스터를 구성하는 Docker 호스트 
		- Manager: 클러스터 상태를 관리하고 작업을 스케쥴링한다. 
		- Worker: Manager가 할당한 Task를 실행 
		- Service: 실행할 이미지, 복제본 수 등 원하는 서비스 구성을 정의한다. 
		- Task: Swarm이 관리하는 개별 실행 작업으로, 컨테이너를 실행하는 단위이다. 
	- Desired & Reconciliation
		- Desired State: 시스템이 유지해야 할 목표 상태 
		- Observed state: 실제 클러스터의 현재 상태 
		- Reconciliation: 실제 상태를 목표 상태에 맞추기 위해 지속적으로 조정하는 과정 
	- Declarative Management 
		- compose 파일에 서비스의 목표 상태를 선언한다.
		- docker stack deploy로 선언된 구성을 swarm에 적용 가능
		- 명령어로 직접 변경한 설정이 compose파일과 다르면 이후 재배포 시 파일의 설정으로 덮어질 수 있다. 
		- 따라서 Compose 파일의 설정의 기준으로 관리하는 것이 중요 
	- Rolling Update 
		- 서비스를 업데이트할 때, 기존 복제본을 단계적으로 교체하는 방식 
		- deploy.update_config.parallelism: 한 번에 업데이트할 Task 수 
		- deploy.update_config.delay: 업데이트 묶음 사이의 대기 시간 
		- 서비스 전체를 한꺼번에 중단하지 않고 업데이트하는 데 활용할 수 있다. 
	- **핵심**
		- Docker Swarm은 선언된 목표 상태를 기준으로 여러 도커 호스트의 서비슬르 배치하고 ,실제 상태를 지속적으로 조정하는 오케스트레이션 시스템이다. 
	- 명령어 
		- `docker swarm init`: Swarm 클러스터 초기화 
		- `docker node ls`: 클러스터 노드 조회 
		- `docker service scale`: 서비스 복제본 수 변경 
		- `docker stack deploy`: 선언된 서비스 구성 배포 
