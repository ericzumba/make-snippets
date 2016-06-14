# DEPENDS ON
$(if $(value LOG),,$(error "LOG is required for log/Makefile")) 

# EXPORTS a log directory target
#
# $(LOG) target
$(LOG):
	mkdir -p $(LOG)

log: $(LOG)
