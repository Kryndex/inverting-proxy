# Copyright 2017 Google Inc. All rights reserved.
# 
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
# 
# http://www.apache.org/licenses/LICENSE-2.0
# 
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

FROM debian

RUN apt-get update && apt-get upgrade -y && \
    apt-get install -y -qq --no-install-recommends \
      ca-certificates \
      git \
      wget && \
    mkdir -p /opt/bin && \
    mkdir -p /opt/src/github.com/google/inverting-proxy

ADD ./ /opt/src/github.com/google/inverting-proxy/agent

RUN wget -O /opt/go1.6.2.linux-amd64.tar.gz \
      https://storage.googleapis.com/golang/go1.6.2.linux-amd64.tar.gz && \
    tar -C /usr/local -xzf /opt/go1.6.2.linux-amd64.tar.gz && \
    export PATH=${PATH}:/usr/local/go/bin/:/opt/bin/ && \
    export GOPATH=/opt/ && \
    go get github.com/golang/groupcache/lru && \
    go get golang.org/x/net/context && \
    go get golang.org/x/oauth2/google && \
    go get google.golang.org/api/compute/v1 && \
    go build -o ${GOPATH}/bin/proxy-forwarding-agent /opt/src/github.com/google/inverting-proxy/agent/agent.go && \
    rm -rf /opt/go1.4.2.linux-amd64.tar.gz && \
    rm -rf /usr/local

ENV PROXY ""
ENV BACKEND ""
ENV HOSTNAME "localhost"
ENV PORT "8080"

CMD ["/bin/sh", "-c", "/opt/bin/proxy-forwarding-agent --proxy=${PROXY} --backend=${BACKEND} --host=${HOSTNAME}:${PORT}"]
