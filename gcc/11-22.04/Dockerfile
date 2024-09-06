FROM kubasejdak/cpp-common:22.04

# Settings.
ARG GCC_VERSION=11
ARG CONAN_TARGET_PROFILE=gcc-${GCC_VERSION}

# Add conan profile.
COPY ${CONAN_TARGET_PROFILE} ${CONAN_HOME}/profiles
RUN ln -sf ${CONAN_HOME}/profiles/${CONAN_TARGET_PROFILE} ${CONAN_HOME}/profiles/target-default && \
    \
    # Add repository with latest compilers.
    sudo apt install -y software-properties-common gnupg && \
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test && \
    sudo apt update && \
    \
    # Install gcc, g++ and lcov.
    sudo apt install -y gcc-${GCC_VERSION} g++-${GCC_VERSION} gdb lcov && \
    sudo update-alternatives --install /usr/bin/gcc-ar   gcc-ar      /usr/bin/x86_64-linux-gnu-gcc-ar-${GCC_VERSION} 10 && \
    sudo update-alternatives --install /usr/bin/gcc      gcc         /usr/bin/x86_64-linux-gnu-gcc-${GCC_VERSION} 10 && \
    sudo update-alternatives --install /usr/bin/g++      g++         /usr/bin/x86_64-linux-gnu-g++-${GCC_VERSION} 10 && \
    sudo update-alternatives --install /usr/bin/ld       ld          /usr/bin/x86_64-linux-gnu-ld 10 && \
    sudo update-alternatives --install /usr/bin/objcopy  objcopy     /usr/bin/x86_64-linux-gnu-objcopy 10 && \
    sudo update-alternatives --install /usr/bin/ranlib   ranlib      /usr/bin/x86_64-linux-gnu-ranlib 10 && \
    sudo update-alternatives --install /usr/bin/size     size        /usr/bin/x86_64-linux-gnu-size 10 && \
    sudo update-alternatives --install /usr/bin/strip    strip       /usr/bin/x86_64-linux-gnu-strip 10 && \
    sudo update-alternatives --install /usr/bin/gcov     gcov        /usr/bin/x86_64-linux-gnu-gcov-${GCC_VERSION} 10 && \
    \
    # Cleanup image.
    sudo apt remove -y software-properties-common gnupg && \
    sudo apt autoremove -y && \
    sudo apt clean && \
    sudo rm -rf /var/lib/apt/lists/*
