FROM alpine:3.8

RUN apk update \
    && apk add --no-cache \
        build-base \
        cmake \
        git \
        file \
        curl \
        yaml-cpp-dev \
        bison

# Google Test
ENV GTEST_ROOT=/opt/gtest
ARG GTEST_VER=release-1.8.1
RUN mkdir -p ${GTEST_ROOT} \
    && cd /tmp \
    && curl -o gtest.tar.gz \
      -L https://github.com/google/googletest/archive/${GTEST_VER}.tar.gz \
    && tar -zxvf gtest.tar.gz --strip-components=1 -C ${GTEST_ROOT} \
    && rm gtest.tar.gz

# YAEP
ARG YAEP_VERSION=550de4cc5600d5f6109c7ebcfbacec51bf80d8d3
RUN cd /tmp \
    && mkdir yaep \
    && curl -o yaep.tar.gz \
       -L https://github.com/vnmakarov/yaep/archive/${YAEP_VERSION}.tar.gz \
    && tar -zxvf yaep.tar.gz --strip-components=1 -C yaep \
    && cd yaep \
    && mkdir build \
    && cd build \
    && env CFLAGS='-fPIC' CXXFLAGS='-fPIC' cmake -DCMAKE_BUILD_TYPE=Release .. \
    && make install \
    && cd ../.. \
    && rm -r yaep yaep.tar.gz

# Create User:Group
# The id is important as jenkins docker agents use the same id that is running
# on the slaves to execute containers
ARG JENKINS_USERID
RUN adduser \
    -u ${JENKINS_USERID} \
    -D \
    jenkins
USER ${JENKINS_USERID}

RUN git config --global user.email 'Jenkins <autobuilder@libelektra.org>' \
    && git config --global user.name 'Jenkins'
