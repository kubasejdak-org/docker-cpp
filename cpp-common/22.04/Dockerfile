FROM kubasejdak/base:22.04

# Settings.
ARG CMAKE_VERSION=3.30.4
ARG CONAN_VERSION=2.7.1
ARG CMAKE_URL=https://github.com/Kitware/CMake/releases/download/v${CMAKE_VERSION}/cmake-${CMAKE_VERSION}-linux-x86_64.sh
ARG LIBFAKETIME_URL=https://github.com/wolfcw/libfaketime.git

ENV CONAN_HOME=/opt/conan/.conan2
ENV PIPX_HOME=/opt/pipx
ENV PIPX_BIN_DIR=${PIPX_HOME}/bin
ENV PATH=${PIPX_BIN_DIR}:${PATH}

# Install common tools.
RUN sudo apt update && \
    sudo apt install -y build-essential make pkg-config bison flex && \
    \
    # Download CMake.
    wget --no-check-certificate ${CMAKE_URL} && \
    sudo sh $(basename ${CMAKE_URL}) --skip-license --prefix=/usr && \
    rm $(basename ${CMAKE_URL}) && \
    \
    # Install python3 and pipx.
    sudo apt install -y python3 python3-pip python3-venv python-is-python3 && \
    sudo -H pip config set global.break-system-packages true && \
    sudo mkdir -p -m 777 ${PIPX_HOME} && \
    sudo mkdir -p -m 777 ${PIPX_BIN_DIR} && \
    sudo -H pip install pipx && \
    \
    # Install conan.
    sudo mkdir -p -m 777 ${CONAN_HOME} && \
    pipx install conan==${CONAN_VERSION} && \
    conan profile detect && \
    sudo ln -s $(which conan) /usr/bin/conan && \
    ln -s ${CONAN_HOME} ${HOME} && \
    \
    # Install libfaketime.
    git clone ${LIBFAKETIME_URL} && \
    cd libfaketime && \
    FAKETIME_COMPILE_CFLAGS="-DFAKE_SETTIME" make -j$(nproc) && \
    sudo make install && \
    cd - && \
    rm -rf libfaketime
