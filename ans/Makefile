# DEPENDS ON
# a target called `ec2_run_deps` and
$(if $(value EC2_LOG_DIR),,$(error "EC2_LOG_DIR is required for ans/Makefile")) 
$(if $(value EC2_NAME_TAG),,$(error "EC2_NAME_TAG is required for ans/Makefile")) 
$(if $(value RUN_CMD),,$(error "RUN_CMD is required for ans/Makefile")) 

ANSIBLE_DIR:=$(PROJECT_DIR)/ansible
ANSIBLE_ROLES_DIR:=$(ANSIBLE_DIR)/roles
ANSIBLE_TEMPLATES_DIR:=$(ANSIBLE_DIR)/template
ANSIBLE_USER_DATA=$(ANSIBLE_TEMPLATES_DIR)/user-data.j2
ANSIBLE_BASE_USER_DATA=$(ANSIBLE_TEMPLATES_DIR)/base-user-data.j2

user_data: LOG=$(EC2_LOG_DIR)
user_data: ec2_run_deps 
	cat $(ANSIBLE_BASE_USER_DATA) > $(ANSIBLE_USER_DATA)
	echo $(RUN_CMD) >> $(ANSIBLE_USER_DATA) 

deploy: $(ANSIBLE_ROLES_DIR) user_data
	ansible-playbook \
		-vvvv \
		-e vivareal_build_version=$(VERSION) \
		-e vivareal_project_name=$(EC2_NAME_TAG) \
		-i ansible/inventory \
		ansible/deploy.yml 
	rm $(ANSIBLE_USER_DATA)

undeploy: $(ANSIBLE_ROLES_DIR)
	ansible-playbook \
		-vvvv \
		-e vivareal_build_version=$(VERSION) \
		-e vivareal_project_name=$(EC2_NAME_TAG) \
		-i ansible/inventory \
		ansible/undeploy.yml

$(ANSIBLE_ROLES_DIR): ansible_roles

ansible_roles:
	mkdir -p $(ANSIBLE_ROLES_DIR)
	ansible-galaxy install -r ansible/requirements.yml -p $(ANSIBLE_ROLES_DIR) 
