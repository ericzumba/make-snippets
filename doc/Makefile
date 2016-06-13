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
SUFFIX:=$(shell date +'%N' | md5sum | cut -c 1-6)

# the image's name
IMAGE_NAME:=$(ORG)/$(PROJECT_NAME):$(VERSION)

# the container's name
CONTAINER_NAME:=$(PROJECT_NAME)-$(CONTAINER_ID)-$(SUFFIX)

# docker's work dir
WORKDIR:=/$(ORG)/$(PROJECT_NAME)

# remove container or daemon flag
ifdef FORCE
  REMOVE_CONTAINER_FLAG:=--rm
else
  ifndef NO_DAEMON
    DAEMON_FLAG:=-d --restart="always"
  endif
endif

# makes the image
image: artifact 
	@if ! docker images $(IMAGE_NAME) | awk '{ print $$2 }' | grep -q -F $(VERSION); then echo 'building image'; docker build --build-arg ARTIFACT=$(ARTIFACT) -t $(IMAGE_NAME) . ; fi

# pushes the image to dockerhub
push: image
	docker push $(IMAGE_NAME)

# removes the container
rm: 
	docker rm $(CONTAINER_NAME)

# kills the container
kill: 
	docker kill $(CONTAINER_NAME)

# attaches to the container
attach:
	docker exec -ti $(CONTAINER_NAME) /bin/bash
