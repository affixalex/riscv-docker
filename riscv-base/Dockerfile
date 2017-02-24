FROM debian:jessie

RUN apt-get -yq update && \
    apt-get -yq dist-upgrade && \
    apt-get -yq install build-essential git-core \
                autoconf automake autotools-dev libtool patchutils bc gperf \
                libgmp-dev libmpfr-dev libmpc-dev libglib2.0 libglib2.0-dev \
                gettext flex bison gawk vim curl texinfo zlib1g zlib1g-dev && \
    rm -rf /var/cache/apt/archives/*
                
RUN git clone https://github.com/riscv/riscv-qemu && \
    cd riscv-qemu && \
    git submodule update --init pixman && \
    ./configure --audio-drv-list="" \
                --target-list=riscv64-softmmu,riscv32-softmmu \
                --prefix=/opt/riscv && \
    NPROC=$(grep -c ^processor /proc/cpuinfo 2>/dev/null || 1) && \
    make -j${NPROC} && \
    make install && \
    cd / && rm -rf riscv-qemu

RUN git clone https://github.com/riscv/riscv-tools.git && \
    cd riscv-tools && \
    git submodule update --init --recursive && \
    RISCV=/opt/riscv ./build.sh && \
    cd / && rm -rf riscv-tools
    
ENV PATH "$PATH:/opt/riscv/bin"    