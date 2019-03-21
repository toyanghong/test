### 包含docker dotnet kubectl 镜像 [ydocker-app/xxx:latest ]

```dockerfile
FROM alpine:3.9

RUN apk add --no-cache \
		ca-certificates

# set up nsswitch.conf for Go's "netgo" implementation (which Docker explicitly uses)
# - https://github.com/docker/docker-ce/blob/v17.09.0-ce/components/engine/hack/make.sh#L149
# - https://github.com/golang/go/blob/go1.9.1/src/net/conf.go#L194-L275
# - docker run --rm debian:stretch grep '^hosts:' /etc/nsswitch.conf
RUN [ ! -e /etc/nsswitch.conf ] && echo 'hosts: files dns' > /etc/nsswitch.conf

ENV DOCKER_CHANNEL stable
ENV DOCKER_VERSION 18.09.3
# TODO ENV DOCKER_SHA256
# https://github.com/docker/docker-ce/blob/5b073ee2cf564edee5adca05eee574142f7627bb/components/packaging/static/hash_files !!
# (no SHA file artifacts on download.docker.com yet as of 2017-06-07 though)

RUN set -eux; \
	\
# this "case" statement is generated via "update.sh"
	apkArch="$(apk --print-arch)"; \
	case "$apkArch" in \
		x86_64) dockerArch='x86_64' ;; \
		armhf) dockerArch='armel' ;; \
		aarch64) dockerArch='aarch64' ;; \
		ppc64le) dockerArch='ppc64le' ;; \
		s390x) dockerArch='s390x' ;; \
		*) echo >&2 "error: unsupported architecture ($apkArch)"; exit 1 ;;\
	esac; \
	\
	if ! wget -O docker.tgz "https://download.docker.com/linux/static/${DOCKER_CHANNEL}/${dockerArch}/docker-${DOCKER_VERSION}.tgz"; then \
		echo >&2 "error: failed to download 'docker-${DOCKER_VERSION}' from '${DOCKER_CHANNEL}' for '${dockerArch}'"; \
		exit 1; \
	fi; \
	\
	tar --extract \
		--file docker.tgz \
		--strip-components 1 \
		--directory /usr/local/bin/ \
	; \
	rm docker.tgz; \
	\
	dockerd --version; \
	docker --version

COPY modprobe.sh /usr/local/bin/modprobe
COPY docker-entrypoint.sh /usr/local/bin/

ENV DOTNET_SYSTEM_GLOBALIZATION_INVARIANT=false \
    LC_ALL=en_US.UTF-8 \
    LANG=en_US.UTF-8

# netcore 依赖
RUN apk add --no-cache \
    ca-certificates \
    \
    # .NET Core dependencies
    krb5-libs \
    libgcc \
    libintl \
    libssl1.1 \
    libstdc++ \
    tzdata \
    userspace-rcu \
    zlib \
    lttng-ust

# Configure web servers to bind to port 80 when present
ENV ASPNETCORE_URLS=http://+:80 \
    # Enable detection of running in a container
    DOTNET_RUNNING_IN_CONTAINER=true \
    # Set the invariant mode since icu_libs isn't included (see https://github.com/dotnet/announcements/issues/20)
    DOTNET_SYSTEM_GLOBALIZATION_INVARIANT=true




# Install .NET Core SDK
ENV DOTNET_SDK_VERSION 2.2.105

RUN wget -O dotnet.tar.gz https://dotnetcli.blob.core.windows.net/dotnet/Sdk/$DOTNET_SDK_VERSION/dotnet-sdk-$DOTNET_SDK_VERSION-linux-musl-x64.tar.gz \
    && dotnet_sha512='04045f20440fca38bc517cf6ae1c6bea48e773629ada8b86f3ce0394784b1372a180aa829fa2bcc5abc184a0e41babe7c5ff0ef376c2b89aad271a0cfb3d75e4' \
    && echo "$dotnet_sha512  dotnet.tar.gz" | sha512sum -c - \
    && mkdir -p /usr/share/dotnet \
    && tar -C /usr/share/dotnet -xzf dotnet.tar.gz \
    && ln -s /usr/share/dotnet/dotnet /usr/bin/dotnet \
    && rm dotnet.tar.gz

# Enable correct mode for dotnet watch (only mode supported in a container)
ENV DOTNET_USE_POLLING_FILE_WATCHER=true \ 
    # Skip extraction of XML docs - generally not useful within an image/container - helps performance
    NUGET_XMLDOC_MODE=skip




# Install kubectl
# Note: Latest version may be found on:
# https://aur.archlinux.org/packages/kubectl-bin/
ADD https://storage.googleapis.com/kubernetes-release/release/v1.6.4/bin/linux/amd64/kubectl /usr/local/bin/kubectl

ENV HOME=/config

RUN set -x && \
    apk add --no-cache curl ca-certificates && \
    chmod +x /usr/local/bin/kubectl && \
    \
    # Create non-root user (with a randomly chosen UID/GUI).
    adduser kubectl -Du 2342 -h /config && \
    \
    # Basic check it works.
    kubectl version --client

#USER kubectl 使用会导致无法cp文件

ENTRYPOINT ["docker-entrypoint.sh"]
CMD ["sh"]
 

```

### 包含 docker kubectl 镜像 [ydocker-app/dockerinkubectl:latest]

```dockerfile


FROM alpine:3.9
 
RUN apk add --no-cache \
		ca-certificates

# set up nsswitch.conf for Go's "netgo" implementation (which Docker explicitly uses)
# - https://github.com/docker/docker-ce/blob/v17.09.0-ce/components/engine/hack/make.sh#L149
# - https://github.com/golang/go/blob/go1.9.1/src/net/conf.go#L194-L275
# - docker run --rm debian:stretch grep '^hosts:' /etc/nsswitch.conf
RUN [ ! -e /etc/nsswitch.conf ] && echo 'hosts: files dns' > /etc/nsswitch.conf

ENV DOCKER_CHANNEL stable
ENV DOCKER_VERSION 18.09.3
# TODO ENV DOCKER_SHA256
# https://github.com/docker/docker-ce/blob/5b073ee2cf564edee5adca05eee574142f7627bb/components/packaging/static/hash_files !!
# (no SHA file artifacts on download.docker.com yet as of 2017-06-07 though)

RUN set -eux; \
	\
# this "case" statement is generated via "update.sh"
	apkArch="$(apk --print-arch)"; \
	case "$apkArch" in \
		x86_64) dockerArch='x86_64' ;; \
		armhf) dockerArch='armel' ;; \
		aarch64) dockerArch='aarch64' ;; \
		ppc64le) dockerArch='ppc64le' ;; \
		s390x) dockerArch='s390x' ;; \
		*) echo >&2 "error: unsupported architecture ($apkArch)"; exit 1 ;;\
	esac; \
	\
	if ! wget -O docker.tgz "https://download.docker.com/linux/static/${DOCKER_CHANNEL}/${dockerArch}/docker-${DOCKER_VERSION}.tgz"; then \
		echo >&2 "error: failed to download 'docker-${DOCKER_VERSION}' from '${DOCKER_CHANNEL}' for '${dockerArch}'"; \
		exit 1; \
	fi; \
	\
	tar --extract \
		--file docker.tgz \
		--strip-components 1 \
		--directory /usr/local/bin/ \
	; \
	rm docker.tgz; \
	\
	dockerd --version; \
	docker --version

COPY modprobe.sh /usr/local/bin/modprobe
COPY docker-entrypoint.sh /usr/local/bin/
 

# Install kubectl
# Note: Latest version may be found on:
# https://aur.archlinux.org/packages/kubectl-bin/
ADD https://storage.googleapis.com/kubernetes-release/release/v1.6.4/bin/linux/amd64/kubectl /usr/local/bin/kubectl

ENV HOME=/config

RUN set -x && \
    apk add --no-cache curl ca-certificates && \
    chmod +x /usr/local/bin/kubectl && \
    \
    # Create non-root user (with a randomly chosen UID/GUI).
    adduser kubectl -Du 2342 -h /config && \
    \
    # Basic check it works.
    kubectl version --client

#USER kubectl 否则无法拷贝cp文件

ENTRYPOINT ["docker-entrypoint.sh"]
CMD ["sh"]



```
