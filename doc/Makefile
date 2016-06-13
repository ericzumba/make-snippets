# depends on
#
# build: 
# $CONTAINER_ID
# $ENV
# $PROJECT_NAME
# $ORG
# $VERSION
#
# optional values for
#
# $FORCE
# $NO_DAEMON
#
# exports docker related variables
#
# a container specific suffix
suffix:
SUFFIX:=$(shell date +'%N' | md5sum | cut -c 1-6)

# the image's name
image_name: org project_name version
IMAGE_NAME:=$(ORG)/$(PROJECT_NAME):$(VERSION)

# the container's name
container_name: project_name container_id suffix
CONTAINER_NAME:=$(PROJECT_NAME)-$(CONTAINER_ID)-$(SUFFIX)

# docker's work dir
workdir: org project_name
WORKDIR:=/$(ORG)/$(PROJECT_NAME)

# remove container or daemon flag
remove_container_flag:
daemon_flag:
ifdef FORCE
REMOVE_CONTAINER_FLAG:=--rm
else
ifndef NO_DAEMON
DAEMON_FLAG:=-d --restart="always"
endif
endif

# makes the image
image: image_name version artifact
	@if ! docker images $(IMAGE_NAME) | awk '{ print $$2 }' | grep -q -F $(VERSION); then echo 'building image'; docker build --build-arg ARTIFACT=$(ARTIFACT) -t $(IMAGE_NAME) . ; fi

# pushes the image to dockerhub
push: image_name
	docker push $(IMAGE_NAME)

# removes the container
rm: container_name
	docker rm $(CONTAINER_NAME)

# kills the container
kill: container_name
	docker kill $(CONTAINER_NAME)

attach: container_name 
	docker exec -ti $(CONTAINER_NAME) /bin/bash
