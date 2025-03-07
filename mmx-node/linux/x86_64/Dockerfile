FROM ubuntu:22.04 AS base
ARG DEBIAN_FRONTEND=noninteractive
RUN apt-get update && apt-get -y upgrade \
		&& apt-get install -y \
			apt-utils \
			libsecp256k1-0 \
			librocksdb6.11 \
			libsodium23 \
			libminiupnpc17 \
			libjemalloc2 \
			zlib1g \
			libgmp10 \
			libgomp1 \
			ocl-icd-libopencl1 \
			curl \
			clinfo \
			&& rm -rf /var/lib/apt/lists/*
WORKDIR /app
COPY . .

ENV MMX_HOME="/data/"
VOLUME /data

# node p2p port
EXPOSE 12339/tcp
# http api port
EXPOSE 11380/tcp

ENTRYPOINT ["./docker-entrypoint.sh"]
CMD ["./run_node.sh"]

FROM base AS amd
ARG AMD_DRIVER=amdgpu-install_5.4.50400-1_all.deb
ARG AMD_DRIVER_URL=https://repo.radeon.com/amdgpu-install/5.4/ubuntu/jammy
RUN mkdir -p /tmp/opencl-driver-amd \
    && cd /tmp/opencl-driver-amd \
    && curl --referer $AMD_DRIVER_URL -O $AMD_DRIVER_URL/$AMD_DRIVER \
	&& dpkg -i $AMD_DRIVER \
	&& rm -rf /tmp/opencl-driver-amd
RUN apt-get update && apt-get -y upgrade \
		&& apt-get install -y \
			rocm-opencl \
			&& rm -rf /var/lib/apt/lists/*

FROM base AS nvidia
RUN apt-get update && apt-get -y upgrade \
		&& apt-get install -y \
			nvidia-driver-470 \
			&& rm -rf /var/lib/apt/lists/*
