FROM kubasejdak/base:24.04

# Settings.
ARG CLANG_FORMAT_VERSION=18
ARG CMAKE_VERSION=4.1.2
ARG CONAN_VERSION=2.22.2
ARG CMAKE_URL=https://github.com/Kitware/CMake/releases/download/v${CMAKE_VERSION}/cmake-${CMAKE_VERSION}-linux-x86_64.sh
ARG LIBFAKETIME_URL=https://github.com/wolfcw/libfaketime.git

ENV CONAN_HOME=/opt/conan/.conan2
ENV UV_INSTALL_DIR=/usr/local/bin
ENV UV_TOOL_BIN_DIR=/usr/local/bin
ENV UV_TOOL_DIR=/usr/local/share/uv/tools

# Install common tools.
RUN sudo apt update && \
    sudo apt install -y build-essential make pkg-config bison flex && \
    \
    # Download CMake.
    wget --no-check-certificate ${CMAKE_URL} && \
    sudo sh $(basename ${CMAKE_URL}) --skip-license --prefix=/usr && \
    rm $(basename ${CMAKE_URL}) && \
    \
    # Install clang-format.
    sudo apt install -y software-properties-common gnupg && \
    wget https://apt.llvm.org/llvm.sh && \
    chmod +x llvm.sh && \
    sudo ./llvm.sh ${CLANG_FORMAT_VERSION} && \
    sudo apt install -y clang-format && \
    sudo apt remove -y software-properties-common gnupg && \
    \
    # Install python3 and uv.
    sudo apt install -y python3 python3-pip python3-venv python-is-python3 && \
    sudo -H pip config set global.break-system-packages true && \
    curl -LsSf https://astral.sh/uv/install.sh | sudo env UV_INSTALL_DIR=${UV_INSTALL_DIR} sh && \
    sudo mkdir -p ${UV_TOOL_DIR} && \
    echo "UV_TOOL_DIR=${UV_TOOL_DIR}" | sudo tee -a /etc/environment && \
    echo "UV_TOOL_BIN_DIR=${UV_TOOL_BIN_DIR}" | sudo tee -a /etc/environment && \
    \
    # Install conan.
    sudo mkdir -p -m 777 ${CONAN_HOME} && \
    sudo uv tool install conan==${CONAN_VERSION} && \
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
