FROM golang:1.14.7-stretch

# https://docs.docker.com/samples/library/debian/#locales
# Preset Locale to en_US.UTF-8
RUN apt-get update && apt-get install -y locales && apt-get clean && rm -rf /var/lib/apt/lists/* \
    && localedef -i en_US -c -f UTF-8 -A /usr/share/locale/locale.alias en_US.UTF-8
ENV LANG en_US.UTF-8
ENV LANGUAGE en_US:en
ENV LC_ALL en_US.UTF-8

RUN apt-get update \
    && apt-get install -y git wget bash \
    && apt-get clean && rm -rf /var/lib/apt/lists/*

ENV GOENV_ROOT /opt/goenv
ENV PATH "$GOENV_ROOT/shims:$GOENV_ROOT/bin:$PATH"
ENV TOOLS_ROOT /opt/golang/tool/versions

# Install goenv.
RUN goenv_version="1.23.3" \
    && gobuild_version="a94d7d7594699f612ef7d0d3412a12db84c1274c" \
    && mkdir -p "${GOENV_ROOT}" \
    && git clone https://github.com/syndbg/goenv.git "${GOENV_ROOT}" \
    && cd "${GOENV_ROOT}" \
    && git checkout "${goenv_version}" \
    && git checkout "${gobuild_version}" ./plugins/go-build \
    && rm -rf " ${GOENV_ROOT}/.git"

# Install golang versions.
# NOTE:　go_versions does not include the system version 1.14.7.
RUN go_versions=" \
        1.11.13 \
        1.12.17 \
        1.13.15 \
        1.15.0 \
        " \
    && for go in $go_versions;do \
        goenv install $go \
        && mkdir -p "${TOOLS_ROOT}/$go" \
        && ls -1d "${GOENV_ROOT}/versions/$go/pkg/linux_amd64" "${GOENV_ROOT}/versions/$go/pkg/linux_amd64_race" "${GOENV_ROOT}/versions/$go/doc" "${GOENV_ROOT}/versions/$go/test" | xargs rm -rf;done \
    && goenv versions

# NOTE: System version 1.14.7 uses a symbolic link in /usr/local/go.
RUN ln -sf "/usr/local/go" "${GOENV_ROOT}/versions/1.14.7"

