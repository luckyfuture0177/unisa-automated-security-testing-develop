# syntax=docker/dockerfile:1
FROM mcr.microsoft.com/dotnet/sdk:5.0 AS build-env
WORKDIR /app

ARG VERSION=0.4.254

# Install Docker CE CLI
RUN apt-get update \
    && apt-get install -y apt-transport-https ca-certificates curl gnupg2 lsb-release \
    && curl -fsSL https://download.docker.com/linux/$(lsb_release -is | tr '[:upper:]' '[:lower:]')/gpg | apt-key add - 2>/dev/null \
    && echo "deb [arch=amd64] https://download.docker.com/linux/$(lsb_release -is | tr '[:upper:]' '[:lower:]') $(lsb_release -cs) stable" | tee /etc/apt/sources.list.d/docker.list \
    && apt-get update \
    && apt-get install -y docker-ce-cli

# Copy csproj and restore as distinct layers
COPY . .

# Install dependencies
# Java (for Dependency Check)
RUN apt-get update && \
    DEBIAN_FRONTEND=noninteractive \
    apt-get -y install default-jre-headless && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/* 
# node / npm
RUN apt-get update && \
    apt-get install -y nodejs npm
# DevSkim
RUN dotnet tool install Microsoft.CST.DevSkim.CLI --version 0.4.254 --tool-path /bin
# Act
RUN curl https://raw.githubusercontent.com/nektos/act/master/install.sh -o /tmp/install.sh && \
    chmod +x /tmp/install.sh && \
    /bin/bash /tmp/install.sh
