ARG REMAGE_BASE_FLAVOR="latest"

FROM gipert/remage-base:latest

LABEL maintainer.name="Luigi Pertoldi"
LABEL maintainer.email="gipert@pm.me"

ARG REMAGE_VERSION="v0.1.0"
ARG CMAKE_BUILD_TYPE="Release"

USER root
WORKDIR /root

RUN mkdir -p src build /opt/remage && \
    if [ "${REMAGE_VERSION}" = "git" ]; then \
        git clone "https://github.com/legend-exp/remage" src; \
    else \
        wget -q -O- "https://github.com/legend-exp/remage/archive/${REMAGE_VERSION}.tar.gz" | tar --strip-components 1 -C src --strip=1 -x -z; \
    fi && \
    cd build && \
    cmake \
        -DCMAKE_INSTALL_PREFIX="/opt/remage" \
        -DCMAKE_BUILD_TYPE="${CMAKE_BUILD_TYPE}" \
        ../src && \
    make -j"$(nproc)" install && \
    if [ "${REMAGE_BASE_FLAVOR}" = "slim" ]; then \
        ctest --output-on-failure --label-exclude 'extra|vis'; \
    else \
        ctest --output-on-failure --label-exclude 'vis'; \
    fi && \
    rm -rf build src

ENV PATH="/opt/remage/bin:$PATH" \
    LD_LIBRARY_PATH="/opt/remage/lib:$LD_LIBRARY_PATH"

ENTRYPOINT ["/opt/remage/bin/remage"]

# vim: ft=dockerfile
