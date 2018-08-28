NAME=maven-local
COMMAND=bash

.SILENT:

all: build maven

build:
	docker build \
		--build-arg UID=$$(id -u) --build-arg GID=$$(id -g) --build-arg USER=$$(whoami) \
		--build-arg http_proxy=$$http_proxy --build-arg https_proxy=$$https_proxy --build-arg no_proxy=$$no_proxy \
		-t $(NAME) .

maven:
	docker run -it --rm \
		-u $(shell echo `id -u`:`id -g`) \
		-v dev_home:/home \
		-e MAVEN_CONFIG=$$HOME \
		-e MAVEN_OPTS="-Duser.home=$$HOME -Dmaven.repo.local=$$HOME/.m2/repository" \
		-e GIT_SSH_COMMAND="ssh -i $$HOME/.ssh/id_rsa_jenkins_nopass -F /dev/null" \
		-w $(shell pwd) \
		$(NAME) $(COMMAND)

compile:
	$(MAKE) COMMAND="mvn compile"

clean:
	$(MAKE) COMMAND="mvn clean"

prepare:
	$(MAKE) COMMAND="mvn --batch-mode release:prepare"
