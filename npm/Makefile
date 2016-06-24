# DEPENDS ON
$(if $(value ARTIFACT_NAME),,$(error "ARTIFACT_NAME is required for npm/Makefile")) 
$(if $(value PROJECT_DIR),,$(error "PROJECT_DIR is required for npm/Makefile")) 

# OPTIONAL
# $BUILD_DIR

# build directory
BUILD_DIR?=$(PROJECT_DIR)/dist

NODE_VERSION:=6.2.0

NPM_IMAGE:=vivareal/npm:$(NODE_VERSION)

# checks npm binary already exists target 
npm:
	@if ! docker images $(NPM_IMAGE) | awk '{ print $$2 }' | grep -q -F $(NODE_VERSION); then echo "building $(NPM_IMAGE)"; docker build -t $(NPM_IMAGE) --build-arg NODE_VERSION=$(NODE_VERSION) builder ; fi

NPM_BINARY:=docker run -v $(PROJECT_DIR):/src -it --rm $(NPM_IMAGE) npm

# path to artifact
NPM_ARTIFACT:=$(BUILD_DIR)/$(ARTIFACT_NAME)

build: npm $(BUILD_DIR)
	$(NPM_BINARY) install && tar -cf $(NPM_ARTIFACT) node_modules app.js lib

$(NPM_ARTIFACT): build

artifact: $(NPM_ARTIFACT)

$(BUILD_DIR):
	mkdir -p $(BUILD_DIR)
