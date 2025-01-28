#
# Licensed to the Apache Software Foundation (ASF) under one or more
# contributor license agreements.  See the NOTICE file distributed with
# this work for additional information regarding copyright ownership.
# The ASF licenses this file to You under the Apache License, Version 2.0
# (the "License"); you may not use this file except in compliance with
# the License.  You may obtain a copy of the License at
#
#    http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.
#

FROM ubuntu:jammy

# configure locale
RUN apt-get update -qq > /dev/null && apt-get install -qq --yes --no-install-recommends \
    locales && \
    locale-gen en_US.UTF-8
ENV LANG="en_US.UTF-8" \
    LANGUAGE="en_US.UTF-8" \
    LC_ALL="en_US.UTF-8" \
    TZ=US \
    DEBIAN_FRONTEND=noninteractive

# Install necessary dependencies for build/test/debug
# Use `lsof -i -P -n` to find open ports
RUN apt-get install -qq \
    apt-transport-https \
    curl \
    git \
    libkrb5-dev \
    python2-dev \
    python3-pip \
    openjdk-8-jdk-headless \
    r-base \
    maven \
    software-properties-common \
    vim \
    wget \
    telnet \
    lsof

RUN update-alternatives --install /usr/bin/python python /usr/bin/python2 1

RUN wget https://bootstrap.pypa.io/pip/2.7/get-pip.py \
    && python get-pip.py

# Add build dependencies for python2
# - First we upgrade pip because that makes a lot of things better
# - Then we remove the provided version of setuptools and install a different version
# - Then we install additional dependencies
RUN python -m pip install -U "pip < 21.0" && \
	apt-get remove -y python-setuptools && \
	python -m pip install "setuptools < 36" && \
	python -m pip install \
        cloudpickle \
        codecov \
        flake8 \
        flaky \
        "future>=0.15.2" \
        "futures>=3.0.5" \
        pytest \
        pytest-runner \
        requests-kerberos \
        "requests >= 2.10.0" \
        "responses >= 0.5.1"

# Now do the same for python3
RUN python3 -m pip install -U pip

RUN apt remove -y openjdk-11-jre-headless

#RUN cd /opt && wget "https://dlcdn.apache.org/maven/maven-3/3.9.9/binaries/apache-maven-3.9.9-bin.zip" && unzip apache-maven-3.9.9-bin.zip
#ENV PATH="/opt/apache-maven-3.9.9/bin:$PATH"

WORKDIR /workspace
RUN git clone https://github.com/apache/incubator-livy.git

