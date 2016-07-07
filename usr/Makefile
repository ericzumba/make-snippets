# DEPENDS ON
# a target called `user-data-setup`
$(if $(value DOCKER_LOGIN),,$(error "DOCKER_LOGIN is required for usr/Makefile")) 
$(if $(value RUN_CMD),,$(error "RUN_CMD is required for usr/Makefile")) 
$(if $(value PROJECT_DIR),,$(error "PROJECT_DIR is required for usr/Makefile")) 

USER_DATA_DIR:=$(PROJECT_DIR)/deploy/variables
USER_DATA:=$(USER_DATA_DIR)/user-data.sh

user-data: LOG=/var/log
user-data: user-data-setup $(USER_DATA_DIR) 
	echo "#!/bin/bash" > $(USER_DATA)
	echo $(DOCKER_LOGIN) >> $(USER_DATA)
	echo $(RUN_CMD) >> $(USER_DATA) 

$(USER_DATA_DIR):
	mkdir -p $(USER_DATA_DIR)
