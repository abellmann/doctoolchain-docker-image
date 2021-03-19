FROM openjdk:14-jdk-alpine

ENV SDKMAN_DIR=/root/.sdkman

# see https://github.com/docker-library/openjdk/issues/73
ENV LC_CTYPE en_US.UTF-8

RUN	echo "add needed tools" && \
    apk add --no-cache curl wget zip unzip git bash \
    git \
    graphviz \
    py-pip \
    ruby \
    py-pygments \
    libc6-compat \
    ttf-dejavu && \
    gem install rdoc --no-document && \
    gem install pygments.rb

# Add pandoc

# Reinstall any system packages required for runtime of pandoc.
RUN apk --no-cache add \
        gmp \
        libffi \
        lua5.3 \
        lua5.3-lpeg

COPY --from=pandoc/core:2.9 \
    /usr/bin/pandoc \
    /usr/bin/pandoc-citeproc \
    /usr/bin


SHELL ["/bin/bash", "-c"]

RUN echo "Install sdkman" &&\
    curl -s "https://get.sdkman.io" | bash && \
    chmod +x $HOME/.sdkman/bin/sdkman-init.sh && \
    $HOME/.sdkman/bin/sdkman-init.sh


RUN	echo "Install java, groovy" && \
    source $HOME/.sdkman/bin/sdkman-init.sh
#    sdk install groovy 2.5.5

RUN     git clone --depth 10 https://github.com/docToolchain/docToolchain.git && \
        cd docToolchain && \
        git checkout v1.3.1 && \
        git submodule update -i && \
        # remove .git folders
        rm -rf `find -type d -name .git` && \
        ./gradlew tasks && \
        ./gradlew && \
        PATH="/docToolchain/bin:${PATH}"

ENV PATH="/docToolchain/bin:${PATH}"

RUN mkdir /project

WORKDIR /project

VOLUME /project

ENTRYPOINT /bin/bash



