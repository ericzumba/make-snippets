# DEPENDS ON
$(if $(value ARTIFACT),,$(error "ARTIFACT is required for doc/Makefile")) 
$(if $(value CONTAINER_ID),,$(error "CONTAINER_ID is required for doc/Makefile")) 
$(if $(value ORG),,$(error "ORG is required for doc/Makefile")) 
$(if $(value PROJECT_NAME),,$(error "PROJECT_NAME is required for doc/Makefile")) 
$(if $(value VERSION),,$(error "VERSION is required for doc/Makefile")) 

# OPTIONAL VALUES FOR
# $FORCE
# $NO_DAEMON
#
# EXPORTS docker related variables and targets
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
