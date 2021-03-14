FROM ubuntu:18.04

LABEL maintainer="imloama <imloama@gmail.com>"

RUN rm /bin/sh && ln -s /bin/bash /bin/sh

RUN apt-get update && \
    apt-get install -q -y \
    zip \
    unzip \
    ca-certificates \
    curl \
    gcc \
    g++ \
    libz-dev 
 
RUN mkdir /graalvm && \
    curl -fsSL "https://pd.zwc365.com/seturl/https://github.com/graalvm/graalvm-ce-builds/releases/download/vm-21.0.0.2/graalvm-ce-java8-linux-amd64-21.0.0.2.tar.gz" \
    | tar -zxC /graalvm --strip-components 1 && \
    find /graalvm -name "*src.zip"  -printf "Deleting %p\n" -exec rm {} + && \
    { test ! -d /graalvm/legal || tar czf /graalvm/legal.tgz /graalvm/legal/; } && \
    { test ! -d /graalvm/legal || rm -r /graalvm/legal; }

RUN ls -alh /graalvm

RUN mkdir /maven && \
    curl -fsSL "https://mirrors.bfsu.edu.cn/apache/maven/maven-3/3.6.3/binaries/apache-maven-3.6.3-bin.tar.gz" | tar -zxC /maven --strip-components 1

RUN rm -rf /graalvm-ce-java8-linux-amd64-21.0.0.2.tar.gz && rm -rf /apache-maven-3.6.3-bin.tar.gz

RUN echo Cleaning up... && \
    apt-get clean && rm -rf "/var/lib/apt/lists/*" && \
    echo 'PATH="/graalvm/bin:/maven/bin:$PATH"' | install --mode 0644 /dev/stdin /etc/profile.d/graal-on-path.sh

ENV JAVA_HOME /graalvm
ENV PATH $JAVA_HOME/bin:/maven/bin:$PATH

RUN    ls -alh /maven &&\
    mvn --version &&\
    java -version
RUN /graalvm/bin/gu install native-image && \
    native-image --version && \
    echo OK
