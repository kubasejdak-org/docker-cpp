FROM kubasejdak/cpp-common:24.04

# Settings.
ARG GCC_URL=https://developer.arm.com/-/media/Files/downloads/gnu/13.3.rel1/binrel/arm-gnu-toolchain-13.3.rel1-x86_64-arm-none-eabi.tar.xz
ARG GCC_DIR=arm-gnu-toolchain-13.3.rel1-x86_64-arm-none-eabi
ARG CONAN_TARGET_PROFILE=arm-none-eabi-gcc-13

# Add conan profile.
COPY ${CONAN_TARGET_PROFILE} ${CONAN_HOME}/profiles
COPY gcc.jinja ${CONAN_HOME}/profiles
RUN ln -sf ${CONAN_HOME}/profiles/${CONAN_TARGET_PROFILE} ${CONAN_HOME}/profiles/target-default && \
    \
    # Download arm-none-eabi-gcc.
    wget ${GCC_URL} && \
    sudo mkdir -p /opt/toolchains && \
    sudo tar -xf $(basename ${GCC_URL}) -C /opt/toolchains && \
    rm $(basename ${GCC_URL}) && \
    \
    # Cleanup image.
    sudo apt autoremove -y && \
    sudo apt clean && \
    sudo rm -rf /var/lib/apt/lists/*

ENV PATH=/opt/toolchains/${GCC_DIR}/bin:${PATH}
