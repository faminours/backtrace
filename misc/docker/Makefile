all: backtrace

MKRPMS_DIR = /mnt/mkrpms
TARGET_DOCKER_DIR = /tmp/backtrace.docker

backtrace: 
	echo "Building backtrace rpm"
	${PWD}/../shared/docker/pdp/mkrpms backtrace
	echo "Copying backtrace rpm"
	mkdir -p ${TARGET_DOCKER_DIR}/RPMS/x86_64
	cp Dockerfile.centos-backtrace ${TARGET_DOCKER_DIR}
	cp -a ${MKRPMS_DIR}/RPMS/x86_64/backtrace* ${TARGET_DOCKER_DIR}/RPMS/x86_64/
	echo "Creating the YUM repo"
	createrepo ${TARGET_DOCKER_DIR}/RPMS
	echo "Creating the backtrace docker"
	docker build --tag local/centos-backtrace \
		     --rm=true \
		     --file ${TARGET_DOCKER_DIR}/Dockerfile.centos-backtrace \
		     --build-arg HTTP_PROXY=$(http_proxy)\
		     --build-arg HTTPS_PROXY=${https_proxy}\
		     --build-arg FTP_PROXY=${ftp_proxy}\
		     --build-arg NO_PROXY=${no_proxy}\
		     ${TARGET_DOCKER_DIR}