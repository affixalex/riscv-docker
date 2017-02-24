FROM hypoalex/riscv-base:latest

RUN echo "deb http://ppa.launchpad.net/webupd8team/java/ubuntu trusty main" | tee /etc/apt/sources.list.d/webupd8team-java.list  && \
    echo "deb-src http://ppa.launchpad.net/webupd8team/java/ubuntu trusty main" | tee -a /etc/apt/sources.list.d/webupd8team-java.list  && \
    apt-key adv --keyserver keyserver.ubuntu.com --recv-keys EEA14886  && \
    apt-get update

RUN echo debconf shared/accepted-oracle-license-v1-1 select true | debconf-set-selections  && \
    echo debconf shared/accepted-oracle-license-v1-1 seen true | debconf-set-selections  && \
    DEBIAN_FRONTEND=noninteractive apt-get install -y --force-yes oracle-java8-installer oracle-java8-set-default

RUN rm -rf /var/cache/oracle-jdk8-installer  && \
    apt-get clean  && \
    rm -rf /var/lib/apt/lists/*
    
RUN wget https://repo1.maven.org/maven2/org/scala-sbt/sbt-launch/1.0.0-M4/sbt-launch.jar && \
    mv sbt-launch.jar /opt/riscv/bin && \
    echo '#!/bin/bash \n\
    SBT_OPTS="-Xms512M -Xmx1024M -Xss1M -XX:+CMSClassUnloadingEnabled" \n\
    java $SBT_OPTS -jar `dirname $0`/sbt-launch.jar '"'$@'"' \n\
    ' > /opt/riscv/bin/sbt && \
    chmod +x /opt/riscv/bin/sbt && \
    sbt && \
    wget http://downloads.lightbend.com/scala/2.12.1/scala-2.12.1.tgz && \
    tar -xzf scala-2.12.1.tgz -C /opt/riscv --strip-components=1 && \
    rm scala-2.12.1.tgz
    
RUN git clone http://git.veripool.org/git/verilator && \
    cd verilator && \
    git pull && git checkout verilator_3_886 && \
    unset VERILATOR_ROOT && \
    autoconf && \
    ./configure --prefix=/opt/riscv && \
    make && \
    make install && \
    cd / && \
    git clone https://github.com/ucb-bar/chisel3.git && \
    sbt publish-local && \
    cd / && rm -rf chisel3 && rm -rf verilator
    
    