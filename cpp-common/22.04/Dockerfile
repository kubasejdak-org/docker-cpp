FROM kubasejdak/base:22.04

# Settings.
ARG CMAKE_VERSION=3.30.0
ARG CONAN_VERSION=1.64.1
ARG CMAKE_URL=https://github.com/Kitware/CMake/releases/download/v${CMAKE_VERSION}/cmake-${CMAKE_VERSION}-linux-x86_64.sh
ARG LIBFAKETIME_URL=https://github.com/wolfcw/libfaketime.git

ENV CONAN_HOME=/opt/conan/.conan2
ENV CONAN_USER_HOME=/opt/conan
ENV PIPX_HOME=/opt/pipx
ENV PIPX_BIN_DIR=${PIPX_HOME}/bin
ENV PATH=${PIPX_BIN_DIR}:${PATH}

# Install common tools.
RUN sudo apt update && \
    sudo apt install -y wget build-essential make pkg-config bison flex file bash-completion && \
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
    sudo mkdir -p -m 777 ${CONAN_USER_HOME} && \
    pipx install conan && \
    conan profile detect && \
    pipx uninstall conan && \
    pipx install conan==${CONAN_VERSION} && \
    conan profile new default --detect && \
    conan profile update settings.compiler.libcxx=libstdc++11 default && \
    conan profile update env.CC=gcc default && \
    conan profile update env.CXX=g++ default && \
    sudo ln -s $(which conan) /usr/bin/conan && \
    sudo chmod -R 777 ${CONAN_USER_HOME} && \
    ln -s ${CONAN_USER_HOME}/.conan ${HOME}/.conan && \
    ln -s ${CONAN_HOME} ${HOME}/.conan2 && \
    \
    # Install libfaketime.
    git clone ${LIBFAKETIME_URL} && \
    cd libfaketime && \
    FAKETIME_COMPILE_CFLAGS="-DFAKE_SETTIME" make -j$(nproc) && \
    sudo make install && \
    cd - && \
    rm -rf libfaketime
