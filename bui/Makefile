# depends on
#
# $ORG
# $PROJECT_DIR
# $PROJECT_NAME
# $VERSION
#
# exports
#
# gradle's version
GRADLE_VERSION:=2.13

# gradle's docker image name
GRADLE_IMAGE:=$(ORG)/gradle:$(GRADLE_VERSION)

# gradles's binary
GRADLE:=docker run --net host --rm -v "$$HOME/.gradle/caches":/root/.gradle/caches -v $(PROJECT_DIR):/project -i $(GRADLE_IMAGE) 

# build directory
BUILD_DIR:=$(PROJECT_DIR)/build/libs

# jar name 
JAR_NAME:=$(ORG)-$(PROJECT_NAME)-$(VERSION).jar

# java artifact
JAVA_ARTIFACT:=$(BUILD_DIR)/$(JAR_NAME)

$(JAVA_ARTIFACT):
	$(GRADLE) clean build

# gradle's binary target
gradle:
	@if ! docker images $(GRADLE_IMAGE) | awk '{ print $$2 }' | grep -q -F $(GRADLE_VERSION); then echo 'building image'; docker build -t $(GRADLE_IMAGE) --build-arg GRADLE_VERSION=$(GRADLE_VERSION) builder ; fi

# build target
build: gradle $(JAVA_ARTIFACT)

