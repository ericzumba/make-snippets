# depends on
#
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
