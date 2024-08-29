# Copyright © 2023 OpenIM. All rights reserved.
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#     http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

# OpenIM base image: https://github.com/openim-sigs/openim-base-image

# Set go mod installation source and proxy

FROM golang:1.21-alpine as builder

ARG GO111MODULE=on

WORKDIR /openim/openim-server

ENV GO111MODULE=$GO111MODULE
ENV GOPROXY=https://goproxy.io,direct 

COPY go.mod go.sum ./
RUN go mod download

COPY . .

RUN go mod tidy && go build -o openim-rpc-group ./cmd/openim-rpc/openim-rpc-group/main.go

RUN cp openim-rpc-group /usr/bin/openim-rpc-group

# FROM ghcr.io/openim-sigs/openim-bash-image:latest
FROM ghcr.io/openim-sigs/openim-bash-image:latest

WORKDIR /openim/openim-server

COPY --from=builder /usr/bin/openim-rpc-group ./bin/openim-rpc-group

ENTRYPOINT ["./bin/openim-rpc-group"]
