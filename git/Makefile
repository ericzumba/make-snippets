# exports git related variables
#
# points to a commit hash 
commit_hash:
COMMIT_HASH=$(shell git rev-parse --short HEAD)

# prepends a commit count to the commit hash 
version: commit_hash
VERSION:=$(shell git rev-list --count $(COMMIT_HASH))-$(COMMIT_HASH)
