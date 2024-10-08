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

# FROM golang:1.20 AS builder
FROM golang:1.21-alpine as builder


ARG GO111MODULE=on

WORKDIR /openim/openim-server

ENV GO111MODULE=$GO111MODULE
ENV GOPROXY=https://goproxy.io,direct 
#$GOPROXY

COPY go.mod go.sum ./
RUN go mod download

COPY . .

# RUN make build BINS=openim-api
RUN go mod tidy && go build -o openim-api ./cmd/openim-api/main.go

# RUN cp /openim/openim-server/_output/bin/platforms/$(go env GOOS)/$(go env GOARCH)/openim-api /usr/bin/openim-api
RUN cp openim-api /usr/bin/openim-api

# FROM ghcr.io/openim-sigs/openim-bash-image:latest
FROM ghcr.io/openim-sigs/openim-bash-image:latest

WORKDIR /openim/openim-server

COPY --from=builder /usr/bin/openim-api ./bin/openim-api

ENTRYPOINT ["./bin/openim-api"]
