FROM mcr.microsoft.com/devcontainers/python:1-3.11

RUN if [[ -z "$devcontainercli" ]] ; then printf "\nERROR: This Dockerfile needs to be built with VS Code Dev Containers!" && exit 1; fi

RUN apt-get update -qq \
    && apt-get install --no-install-recommends -y \
        build-essential \
        gcc \
        cmake \
        libcairo2-dev \
        libffi-dev \
        libpango1.0-dev \
        freeglut3-dev \
        ffmpeg \
        pkg-config \
        make \
        wget \
        ghostscript \
        fonts-noto \
    && apt-get clean \
    && apt-get autoclean \
    && apt-get autoremove --purge  -y \
    && rm -rf /var/lib/apt/lists/*

RUN fc-cache -fv

# Setup a minimal texlive installation
COPY docker/texlive-profile.txt /tmp/
ENV PATH=/usr/local/texlive/bin/armhf-linux:/usr/local/texlive/bin/aarch64-linux:/usr/local/texlive/bin/x86_64-linux:$PATH

RUN wget -O /tmp/install-tl-unx.tar.gz http://mirror.ctan.org/systems/texlive/tlnet/install-tl-unx.tar.gz && \
    mkdir /tmp/install-tl && \
    tar -xzf /tmp/install-tl-unx.tar.gz -C /tmp/install-tl --strip-components=1 && \
    /tmp/install-tl/install-tl --profile=/tmp/texlive-profile.txt && \
    rm -rf /tmp/*

RUN tlmgr install \
    amsmath babel-english cbfonts-fd cm-super count1to ctex doublestroke \
    dvisvgm everysel fontspec frcursive fundus-calligra gnu-freefont jknapltx \
    latex-bin mathastext microtype multitoc physics prelim2e preview ragged2e \
    relsize rsfs setspace standalone tipa wasy wasysym xcolor xetex xkeyval

CMD [ "/bin/bash" ]
