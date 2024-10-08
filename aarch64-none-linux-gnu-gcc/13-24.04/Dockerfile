FROM kubasejdak/cpp-common:24.04

# Settings.
ARG GCC_URL=https://developer.arm.com/-/media/Files/downloads/gnu/13.3.rel1/binrel/arm-gnu-toolchain-13.3.rel1-x86_64-aarch64-none-linux-gnu.tar.xz
ARG GCC_DIR=arm-gnu-toolchain-13.3.rel1-x86_64-aarch64-none-linux-gnu
ARG CONAN_TARGET_PROFILE=aarch64-none-linux-gnu-gcc-13

# Add conan profile.
COPY ${CONAN_TARGET_PROFILE} ${CONAN_HOME}/profiles
COPY gcc.jinja ${CONAN_HOME}/profiles
RUN ln -sf ${CONAN_HOME}/profiles/${CONAN_TARGET_PROFILE} ${CONAN_HOME}/profiles/target-default && \
    \
    # Download aarch64-none-linux-gnu-gcc.
    wget ${GCC_URL} && \
    sudo mkdir -p /opt/toolchains && \
    sudo tar -xf $(basename ${GCC_URL}) -C /opt/toolchains && \
    rm $(basename ${GCC_URL}) && \
    sudo ln -s /opt/toolchains/${GCC_DIR}/bin/aarch64-none-linux-gnu-ar        /opt/toolchains/${GCC_DIR}/bin/aarch64-linux-gnu-ar && \
    sudo ln -s /opt/toolchains/${GCC_DIR}/bin/aarch64-none-linux-gnu-gcc       /opt/toolchains/${GCC_DIR}/bin/aarch64-linux-gnu-gcc && \
    sudo ln -s /opt/toolchains/${GCC_DIR}/bin/aarch64-none-linux-gnu-g++       /opt/toolchains/${GCC_DIR}/bin/aarch64-linux-gnu-g++ && \
    sudo ln -s /opt/toolchains/${GCC_DIR}/bin/aarch64-none-linux-gnu-ld        /opt/toolchains/${GCC_DIR}/bin/aarch64-linux-gnu-ld && \
    sudo ln -s /opt/toolchains/${GCC_DIR}/bin/aarch64-none-linux-gnu-objcopy   /opt/toolchains/${GCC_DIR}/bin/aarch64-linux-gnu-objcopy && \
    sudo ln -s /opt/toolchains/${GCC_DIR}/bin/aarch64-none-linux-gnu-ranlib    /opt/toolchains/${GCC_DIR}/bin/aarch64-linux-gnu-ranlib && \
    sudo ln -s /opt/toolchains/${GCC_DIR}/bin/aarch64-none-linux-gnu-size      /opt/toolchains/${GCC_DIR}/bin/aarch64-linux-gnu-size && \
    sudo ln -s /opt/toolchains/${GCC_DIR}/bin/aarch64-none-linux-gnu-strip     /opt/toolchains/${GCC_DIR}/bin/aarch64-linux-gnu-strip && \
    sudo ln -s /opt/toolchains/${GCC_DIR}/bin/aarch64-none-linux-gnu-gcov      /opt/toolchains/${GCC_DIR}/bin/aarch64-linux-gnu-gcov && \
    \
    # Cleanup image.
    sudo apt autoremove -y && \
    sudo apt clean && \
    sudo rm -rf /var/lib/apt/lists/*

ENV PATH=/opt/toolchains/${GCC_DIR}/bin:${PATH}
