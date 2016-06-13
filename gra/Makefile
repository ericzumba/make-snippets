# DEPENDS ON
$(if $(value ARTIFACT_NAME),,$(error "ARTIFACT_NAME is required for gra/Makefile")) 
$(if $(value ORG),,$(error "ORG is required for gra/Makefile")) 
$(if $(value PROJECT_DIR),,$(error "PROJECT_DIR is required for gra/Makefile")) 

# EXPORTS gradle related targets and variables
#
# gradle's version
GRADLE_VERSION:=2.13

# gradle's docker image name
GRADLE_IMAGE:=$(ORG)/gradle:$(GRADLE_VERSION)

# checks gradle binary already exists target 
gradle:
	@if ! docker images $(GRADLE_IMAGE) | awk '{ print $$2 }' | grep -q -F $(GRADLE_VERSION); then echo "building $(GRADLE_IMAGE)"; docker build -t $(GRADLE_IMAGE) --build-arg GRADLE_VERSION=$(GRADLE_VERSION) builder ; fi

# gradle's binary
GRADLE_BINARY:=docker run --net host --rm -v "$$HOME/.gradle/caches":/root/.gradle/caches -v $(PROJECT_DIR):/project -i $(GRADLE_IMAGE) 

# build directory
BUILD_DIR:=$(PROJECT_DIR)/build/libs

# java artifact
JAVA_ARTIFACT:=$(BUILD_DIR)/$(ARTIFACT_NAME)

# absolute path java artifact target
$(JAVA_ARTIFACT): 
	$(GRADLE_BINARY) -ParchiveName=$(ARTIFACT_NAME) clean build

# build target
build: gradle $(JAVA_ARTIFACT)

# artifact target
artifact: build
