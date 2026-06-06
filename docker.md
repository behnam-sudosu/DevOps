# docker
## install docker  

	unlink /etc/resolv.conf
	touch /etc/resolv.conf
		nameserver 185.206.92.250
		nameserver 185.231.181.206
	 go to docker site

	first uninstall old version
	--command--
	for pkg in docker.io docker-doc docker-compose docker-compose-v2 podman-docker containerd runc; do sudo apt-get remove $pkg; done


######################################################################
ubuntu tested
	# install
	sudo apt-get update
	sudo apt-get install ca-certificates curl
	sudo install -m 0755 -d /etc/apt/keyrings
	sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
	sudo chmod a+r /etc/apt/keyrings/docker.asc
	
	# Add the repository to  Apt sources:
		echo \
			"deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
			$(. /etc/os-release && echo "${UBUNTU_CODENAME:-$VERSION_CODENAME}") stable" | \
			sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
	
	sudo apt-get update
	apk add update ===>> alpine update
	# install docker
	sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
	
########################################################################
	
	
	easy way install 
	
	curl -fsSL https://get.docker.com -o get-docker.sh
	sudo sh get-docker.sh
	
#######################################################################

debian tested

# Add Docker's official GPG key:
sudo apt update
sudo apt install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/debian/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Add the repository to Apt sources:
sudo tee /etc/apt/sources.list.d/docker.sources <<EOF
Types: deb
URIs: https://download.docker.com/linux/debian
Suites: $(. /etc/os-release && echo "$VERSION_CODENAME")
Components: stable
Signed-By: /etc/apt/keyrings/docker.asc
EOF

sudo apt update

# install docker
sudo apt install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

#######################################################################
		
	systemctl status docker.service
	systemctl enable docker.service
	
	ls /var/lib/docker ===>> all file save here
	must mount this directory to another disk
	
## command

	docker --version ===>>
	docker ===>> show all switch and help
	docker volume ===>> show command for volume

	
	docker login ===>> outomaticly connect to docker hub
		username
		password
	docker logout ===>> go outside
	docker login -u USER -p PASSWORD
	

	--downloa image--
	#download image (lates image)
	docker pull hello-world ===>> 
	# you shouldn't pull latest in production
	docker pull ubuntu:22.04 ===>> download image with version
	#show all images you pulled
	docker images ===>> 
	
	docker run ubuntu:22.04 ===>> run countainer image
	docker countainer run ubuntu:22.04
	
	
	docker ps ===>> show docker countainer image run
	docker ps -a ===>> show all docker countainer (show status)
	docker container ls === >> show container list
	
	
	docker run -it ubuntu:22.04 /bin/bash ===>> i = interactive, t = tty
	
	CTRL + d ===>> exit countainer
	
	docker run -dit ubuntu:22.04 === >> -d = ditach === >> go to background
	docker run -dit -e MY_VAR=behnam ubuntu:22.04 === >> -e = add variable befor run
	docker run -dit -e MY_VAR=behnam --hostname=serv1 nginx === >> host name
		root@serv1:/
	docker run -dit -e MY_VAR=behnam --hostname=serv1 --name ubuntusrv1 nginx ===>> countener name
	docker run -dit -e MY_VAR=behnam --hostname=serv1 --name ubuntusrv1 -p 80:80 nginx ===>> -p  port 
	docker run -dit -e MY_VAR=behnam --hostname=serv1 --name ubuntusrv1 -p 80:80 -p 443:443 nginx === >> open 2 port
	docker run -dit -e MY_VAR=behnam --hostname=serv1 --name ubuntusrv1 -p 7000-7010:7000-7010 nginx === >> open range ip
	docker run -dit -e MY_VAR=behnam --hostname=serv1 --name ubuntusrv1 -p 192.168.1.100:7000-7010:7000-7010 nginx === >> open ip
	docker run -dit -e MY_VAR=behnam --hostname=serv1 --name ubuntusrv1 -p 127.0.0.1:80:80 nginx === >> open ip
	docker run -dit -e MY_VAR=behnam --hostname=serv1 --name ubuntusrv1 -p 192.168.1.100/udp nginx === >> open udp
	docker run -dit -e MY_VAR=behnam --hostname=serv1 --name ubuntusrv1 --ip 192.168.1.100 nginx
	docker run -dit -e MY_VAR=behnam -e MARIADB_ROOT_PASSWORD=1234 --hostname=my-db --name db1  mariadb ===>> docker run for database

# log

	docker logs nginx
   	docker logs -f nginx ===>> -f = follow
	docker run logs -f --tail 5 nginx 
	docker exec -it (CONTAINER ID) or (NAMES) /bin/bash=== >> if you want to go inside the container
	docker exec -it 32647236476 bash
	docker exec -it 32647236476 sh
	getway docker === >> 172.168.0.1/16
	
## remove

	docker rm -f CONTAINER_ID ===>> remove container force
	docker rm CONTAINER_ID ===>> remove one container
	docker stop CONTAINER_ID ===>> stop container
	docker start CONTAINER_ID ===>> start container
	docker image rm -f helow-world ===>> delete image
	docker rm `docker ps -aq` ===>> delete history docker ps -a
	docker rm -f `docker ps -aq` ===>> delete all run
	docker rmi IMAGE_NAME ===>> delete image you pulled
	docker rmi -f IMAGE_NAME
	docker tag OLD_NAME NEW_NAME ===>> change name of image
	docker ps -aq -f status=exited
	docker rm $(docker ps -aq -f status=exited) ===>> clean exited countainer
	docker rm `docker ps -aq -f status=exited`
	docker countainer prune ===>> clean stop countainer
	

	
# backup image

	
 
	/etc/docker/deamon.json ===>> address repository put here if you don't want use docker_hub
	.env ===>> variable file
	

	docker inspect COUNTAINER_NAME ===>> more information about countainer
		network
		disk
		configuration
		oomkilled ===>> RAM
	docker inspect --format '{{ .state.status }}' COUNTAINER _NAME
	docker stats ===>> like top
	docker cp file1 COUNTAINER_NAME:/tmp/file1 ===>> copy from local to countainer
	docker cp COUNTAINER_NAME:/tmp/file1 /tmp/file1
	docker run -dit --name srv1 --restart=no ubuntu:22.04
	docker run -dit --name srv1 --restart=always ubuntu:22.04 ===>> after reboot and restart docker service countainer come up
	docker events ===>> show events
	docker run -dit --name nginx2 --restart=always -p 80:80 --dns 8.8.8.8 nginx ===>> set custom DNS servers


## make docker image

	docker save -o /tmp/IMAGE_NAME.tar IMAGE_NAME ===>> save image in local storage
	docker load -i IMAGE_PATH ===>> load images you have in storage
	docker load --import IMAGE_PATH
	docker commit COUNTAINER_NAME myubuntu:v1.0.0

## make docker file

	touch Dockerfile
		FROM ubuntu:22.04
		FROM ubuntu:lates
		FROM alpine
 
		ENV TZ="ASIA/TEHRAN"

		LABLE version="1.0.0"
		LABLE authors="behnam"

		RUN apt update && apt install -y bash vim curl
		RUN apt install nginx -y

		RUN apk update && apk add bash vim curl
		RUN apk add nginx

		COPY PATH /app ===>> copy file or directory to image

		WORKDIR /app ===>> when you login to countainer you go /app or any directory you write in docker file
		COPY . . ===>> your home is /app in WORKDIR 

		ENV API_KEY="12345"

		
		EXPOSE 80 ===>> open port 80
		EXPOSE 8080

		RUN chmod +x app.sh

		ADD . /app ===>> like copy, can download url, if you have .tar file extrac in your os
		ADD . .

		USER test ===>> database mongodb replica set athentication key need USER

		VOLUME ["/home/behnam", "/app2"] ===>> mount

		CMD ["./app.sh"] ===>> last line always
		ENTRYPOIN ["bash", "./app.sh"] ===>> last line always, and choice zsh,sh,bash
		ENTRYPOIN ["bash"]
		CMD ["app.sh"]

		ENTRYPOIN ["nginx"]
		CMD ["-g", "daemon off;]

	save the file
	docker build -t my-app .
	inside the countainer ===>> ls  ===>> delete this directory


#################################################################################


