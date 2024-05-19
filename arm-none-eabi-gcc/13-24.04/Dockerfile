FROM kubasejdak/cpp-common:24.04

# Settings.
ARG GCC_URL=https://developer.arm.com/-/media/Files/downloads/gnu/13.2.rel1/binrel/arm-gnu-toolchain-13.2.rel1-x86_64-arm-none-eabi.tar.xz
ARG GCC_DIR=arm-gnu-toolchain-13.2.Rel1-x86_64-arm-none-eabi
ARG CONAN_TARGET_PROFILE=arm-none-eabi-gcc-13

# Add conan profile.
COPY ${CONAN_TARGET_PROFILE} ${CONAN_USER_HOME}/.conan/profiles
COPY ${CONAN_TARGET_PROFILE}_2 ${CONAN_USER_HOME}/.conan2/profiles/${CONAN_TARGET_PROFILE}
COPY gcc.jinja ${CONAN_USER_HOME}/.conan2/profiles
RUN ln -sf ${CONAN_USER_HOME}/.conan/profiles/${CONAN_TARGET_PROFILE} ${CONAN_USER_HOME}/.conan/profiles/target-default && \
    ln -sf ${CONAN_USER_HOME}/.conan2/profiles/${CONAN_TARGET_PROFILE} ${CONAN_USER_HOME}/.conan2/profiles/target-default && \
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
