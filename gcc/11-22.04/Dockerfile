FROM kubasejdak/cpp-common:22.04

# Settings.
ARG GCC_VERSION=11
ARG CONAN_TARGET_PROFILE=gcc-${GCC_VERSION}

# Add conan profile.
COPY ${CONAN_TARGET_PROFILE} ${CONAN_HOME}/profiles
RUN ln -sf ${CONAN_HOME}/profiles/${CONAN_TARGET_PROFILE} ${CONAN_HOME}/profiles/target-default && \
    \
    # Install gcc, g++ and lcov.
    sudo apt install -y gcc gcc-${GCC_VERSION} g++ g++-${GCC_VERSION} gdb lcov && \
    \
    # Install gcovr.
    pipx install gcovr && \
    \
    # Cleanup image.
    sudo apt autoremove -y && \
    sudo apt clean && \
    sudo rm -rf /var/lib/apt/lists/*
