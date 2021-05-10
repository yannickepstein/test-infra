# Copyright 2020 gRPC authors
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#     https://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

FROM golang:1.14

RUN mkdir -p /executable
WORKDIR /executable

ARG REPOSITORY=grpc/grpc-go
ARG GITREF=master

RUN git clone https://github.com/$REPOSITORY.git .
RUN git submodule update --init
RUN git checkout $GITREF

# Save commit sha for debug use
RUN echo 'COMMIT SHA' > GRPC_GIT_COMMIT.txt
RUN git rev-parse $GITREF >> GRPC_GIT_COMMIT.txt

# Build go excutables
RUN go build -o /executable/bin/worker ./benchmark/worker

# Copy go excuatable to a new image
FROM debian:buster

RUN mkdir -p /executable
WORKDIR /executable
COPY --from=0 /executable/GRPC_GIT_COMMIT.txt /executable/GRPC_GIT_COMMIT.txt
COPY --from=0 /executable/bin/worker /executable/bin/worker
COPY --from=0 /executable/testdata /executable/testdata

CMD ["bash"]