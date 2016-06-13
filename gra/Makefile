# depends on
#
# $ARTIFACT_NAME
# $ORG
# $PROJECT_DIR
# $PROJECT_NAME
# $VERSION
#
# exports
#
# gradle's version
gradle_version:
GRADLE_VERSION:=2.13

# gradle's docker image name
gradle_image: org gradle_version
GRADLE_IMAGE:=$(ORG)/gradle:$(GRADLE_VERSION)

# gradles's binary
gradle_binary: 
GRADLE_BINARY:=docker run --net host --rm -v "$$HOME/.gradle/caches":/root/.gradle/caches -v $(PROJECT_DIR):/project -i $(GRADLE_IMAGE) 

# build directory
build_dir: project_dir
BUILD_DIR:=$(PROJECT_DIR)/build/libs

# java artifact
java_artifact: build_dir artifact_name 
JAVA_ARTIFACT:=$(BUILD_DIR)/$(ARTIFACT_NAME)

$(JAVA_ARTIFACT): gradle_binary version java_artifact
	$(GRADLE_BINARY) -ParchiveName=$(ARTIFACT_NAME) clean build

# gradle's target 
gradle: gradle_image gradle_version
	@if ! docker images $(GRADLE_IMAGE) | awk '{ print $$2 }' | grep -q -F $(GRADLE_VERSION); then echo "building $(GRADLE_IMAGE)"; docker build -t $(GRADLE_IMAGE) --build-arg GRADLE_VERSION=$(GRADLE_VERSION) builder ; fi

# build target
build: gradle $(JAVA_ARTIFACT)