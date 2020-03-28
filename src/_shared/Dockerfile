ARG os
FROM buildpack-deps:$os

ARG os_cache_date
RUN apt-get update &&\
    apt-get upgrade -y &&\
    apt-get install -y debhelper devscripts po-debconf dpkg-dev cmake &&\
    rm -rf /var/lib/apt/lists/*

WORKDIR /build/
SHELL ["/bin/bash", "-c"]
ARG os
ARG git_tag
RUN git clone https://github.com/AviSynth/AviSynthPlus.git avs &&\
    cd avs &&\
    git checkout $git_tag

ARG rev=1
COPY debian avs/debian
RUN cd /build/avs &&\
    dch --distribution $os -l +$os "Porting to $os" -m -t &&\
    for ((i=1; i<$rev; i++)); do dch --distribution $os -l +$os "Rebuild" -m -t; done &&\
    DEB_BUILD_OPTIONS="parallel=4" debian/rules binary &&\
    mkdir /build/reg &&\
    mv /build/*.deb /build/reg
