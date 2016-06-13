# DEPENDS ON
$(if $(value LOG),,$(error "LOG is required for log/Makefile")) 

# EXPORTS a log directory target
#
# $(LOG) target
$(LOG): log
	mkdir -p $(LOG)
