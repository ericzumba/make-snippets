# DEPENDS ON
# a target called `stack-variables-setup` and
$(if $(value ENV),,$(error "ENV is required for asn/Makefile")) 
$(if $(value ORG),,$(error "ORG is required for asn/Makefile")) 
$(if $(value PROJECT_DIR),,$(error "PROJECT_DIR is required for asn/Makefile")) 

ASNO_DIR:=$(PROJECT_DIR)/asno

HOST_OUTPUT_DIR:=$(PROJECT_DIR)/deploy/output
HOST_TEMPLATE_DIR:=$(ASNO_DIR)/templates
HOST_VARIABLES_DIR:=$(PROJECT_DIR)/deploy/variables

ASNO_OUTPUT_DIR:=/project/deploy/output
ASNO_TEMPLATE_DIR:=/project/deploy/templates
ASNO_VARIABLES_DIR:=/project/deploy/variables

NESTED_ASNO_VERSION:=$(shell cd $(ASNO_DIR) \
		&& COMMIT_HASH=`git rev-parse --short HEAD` \
		&& echo `git rev-list --count $$COMMIT_HASH`-$$COMMIT_HASH)

ASNO_IMAGE_NAME:=$(ORG)/asno:$(NESTED_ASNO_VERSION)
ASNO_SHELL=docker run \
						--rm \
						-e AWS_DEFAULT_REGION=sa-east-1 \
						-e AWS_ACCESS_KEY_ID=$(AWS_ACCESS_KEY_ID) \
						-e AWS_SECRET_ACCESS_KEY=$(AWS_SECRET_ACCESS_KEY) \
						-v $(HOST_OUTPUT_DIR):$(ASNO_OUTPUT_DIR) \
						-v $(HOST_TEMPLATE_DIR):$(ASNO_TEMPLATE_DIR) \
						-v $(HOST_VARIABLES_DIR):$(ASNO_VARIABLES_DIR) \
						--name asno \
						-ti $(ASNO_IMAGE_NAME)

asno-binary:
	cd $(PROJECT_DIR)/asno && make VERSION=$(NESTED_ASNO_VERSION) PROJECT_DIR=$(PROJECT_DIR)/asno image

STACK_DESCRIPTION:=$(TEMPLATE)-$(VARIABLES)-$(ENV)
stack-description: stack-variables stack-template name-tag asno-binary
	$(ASNO_SHELL) cfndsl \
		-D "nameTag='$(NAME_TAG)'" \
		-y $(ASNO_VARIABLES_DIR)/$(VARIABLES).yml \
		-o $(ASNO_OUTPUT_DIR)/$(STACK_DESCRIPTION).json \
		$(ASNO_TEMPLATE_DIR)/$(TEMPLATE).rb

validate-stack: stack-description
	$(ASNO_SHELL) aws cloudformation validate-template \
		--template-body file://$(ASNO_OUTPUT_DIR)/$(STACK_DESCRIPTION).json

deploy-stack: validate-stack stack-name push
	$(ASNO_SHELL) aws cloudformation create-stack \
		--stack-name $(STACK_NAME)-$(ENV) \
		--template-body file://$(ASNO_OUTPUT_DIR)/$(STACK_DESCRIPTION).json

destroy-stack: stack-name
	$(ASNO_SHELL) aws cloudformation delete-stack \
		--stack-name $(STACK_NAME)-$(ENV)

name-tag:
	$(if $(value NAME_TAG),,$(error "NAME_TAG is required for asn/Makefile")) 

stack-name: 
	$(if $(value STACK_NAME),,$(error "STACK_NAME is required for asn/Makefile")) 

stack-template: 
	$(if $(value TEMPLATE),,$(error "TEMPLATE is required for asn/Makefile")) 

stack-variables: stack-variables-setup
	$(if $(value VARIABLES),,$(error "VARIABLES is required for asn/Makefile")) 

$(HOST_OUTPUT_DIR):
	mkdir -p $(HOST_OUTPUT_DIR)

$(HOST_VARIABLES_DIR):
	mkdir -p $(HOST_VARIABLES_DIR)

$(HOST_TEMPLATE_DIR):
	mkdir -p $(HOST_TEMPLATE_DIR)
