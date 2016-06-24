# DEPENDS ON
$(if $(value ARTIFACT_NAME),,$(error "ARTIFACT_NAME is required for npm/Makefile")) 
$(if $(value PROJECT_DIR),,$(error "PROJECT_DIR is required for npm/Makefile")) 

# OPTIONAL
# $BUILD_DIR

# build directory
BUILD_DIR?=$(PROJECT_DIR)/dist

# path to artifact
NPM_ARTIFACT:=$(BUILD_DIR)/$(ARTIFACT_NAME)

build:
	docker run -v $(PROJECT_DIR):/src -it --rm mkenney/npm:alpine npm install && \
		tar -cf $(NPM_ARTIFACT) node_modules app.js lib

$(NPM_ARTIFACT): build

artifact: $(NPM_ARTIFACT)

$(BUILD_DIR):
	mkdir -p $(BUILD_DIR)
