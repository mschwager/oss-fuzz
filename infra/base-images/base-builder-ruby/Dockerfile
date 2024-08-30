# Copyright 2024 Google LLC
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#      http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.
#
################################################################################

FROM gcr.io/oss-fuzz-base/base-builder

ARG RUBY_VERSION="3.3.4"

RUN apt update && apt install -y \
    lsb-release \
    software-properties-common \
    gnupg2 \
    binutils \
    libyaml-dev

SHELL ["/bin/bash", "-lc"]

RUN gpg2 \
    --keyserver keyserver.ubuntu.com \
    --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3 7D2BAF1CF37B13E2069D6956105BD0E739499BDB && \
    curl -sSL https://get.rvm.io | bash -s stable && \
    echo "source /etc/profile.d/rvm.sh" >> /etc/bash.bashrc

RUN rvm requirements && \
    rvm install $RUBY_VERSION && \
    rvm use $RUBY_VERSION --default

RUN gem install tebako --version 0.8.4

# The MAKE variable allows overwriting the make command at runtime. This forces the
# Ruby C extension to respect ENV variables when compiling, like CC, CFLAGS, etc.
RUN MAKE="make --environment-overrides V=1" \
    RUZZY_DEBUG="1" \
    CFLAGS="$CFLAGS -fPIC" \
    CXXFLAGS="$CXXFLAGS -fPIC" \
    gem install --verbose ruzzy
