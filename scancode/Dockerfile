# Copyright (c) Microsoft Corporation and others. Made available under the MIT license.
# SPDX-License-Identifier: MIT

# Docker image for nexB/scancode-toolkit
# Usage example (use default SCANCODE_OPTIONS, mount output folder):
# docker run -e SOURCE_URL=https://github.com/clearlydefined/tool-images.git -e SOURCE_COMMIT=5be1d13 -v $AGENT_BUILDDIRECTORY:/output container_name
# Usage example (override SCANCODE_OPTIONS, do not mount output folder):
# docker run -e SCANCODE_OPTIONS="--copyright --license" -e SOURCE_URL=https://github.com/clearlydefined/tool-images.git -e COMMIT=5be1d13 container_name

FROM python:2.7

ENV SCANCODE_OPTIONS "--copyright --license --info --package --license-diag --only-findings --strip-root --quiet --timeout 1000 -n 2"
ENV SCANCODE_HOME /opt/scancode-toolkit-2.9.0b1
ENV SCANCODE_OUTPUT_PATH /output
ENV SCANCODE_OUTPUT_FILENAME scancode.json

RUN curl -sL https://github.com/nexB/scancode-toolkit/releases/download/v2.9.0b1/scancode-toolkit-2.9.0b1.tar.bz2 | tar -C /opt -jx \
&& ${SCANCODE_HOME}/scancode --version

RUN mkdir /repo ${SCANCODE_OUTPUT_PATH}
WORKDIR /repo

ENTRYPOINT git clone ${SOURCE_URL} --quiet && cd */ && git reset --hard ${SOURCE_COMMIT} --quiet \
  && ${SCANCODE_HOME}/scancode ${SCANCODE_OPTIONS} --json-pp ${SCANCODE_OUTPUT_PATH}/${SCANCODE_OUTPUT_FILENAME} ./ && cat ${SCANCODE_OUTPUT_PATH}/${SCANCODE_OUTPUT_FILENAME}
