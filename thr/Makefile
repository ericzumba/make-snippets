# EXPORTS variables related to the number of cpus/threads 
#
# number of processors
NUM_CPUS=$(shell getconf _NPROCESSORS_ONLN)

# 1 thread or num-cpus-1 threads
ifeq ($(NUM_CPUS), 1)
 THREADS?=$(NUM_CPUS)
endif
THREADS?=$(shell expr $(NUM_CPUS) - 1)

# signals more threads than cpus
THREADS_GT_CPUS:=$(shell echo ${THREADS}\>${NUM_CPUS} | bc)

# issues a warning when using more threads than available cpus
threads:
ifeq ($(THREADS_GT_CPUS), 1)
	$(warning '$(THREADS) threads greater than $(NUM_CPUS) CPUs')
endif
