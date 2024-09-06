FROM kubasejdak/cpp-common:24.04

# Settings.
ARG CLANG_VERSION=18
ARG CONAN_TARGET_PROFILE=clang-${CLANG_VERSION}

# Add conan profile.
COPY ${CONAN_TARGET_PROFILE} ${CONAN_HOME}/profiles
RUN ln -sf ${CONAN_HOME}/profiles/${CONAN_TARGET_PROFILE} ${CONAN_HOME}/profiles/target-default && \
    \
    # Install clang, clang-tools, clang-format, clang-tidy and llvm.
    sudo apt update && \
    sudo apt install -y software-properties-common gnupg && \
    wget https://apt.llvm.org/llvm.sh && \
    chmod +x llvm.sh && \
    sudo ./llvm.sh ${CLANG_VERSION} && \
    sudo apt install -y clang-${CLANG_VERSION} \
    clang-tools-${CLANG_VERSION} \
    clang-format-${CLANG_VERSION} \
    clang-tidy-${CLANG_VERSION} \
    llvm-${CLANG_VERSION} \
    lld-${CLANG_VERSION} \
    lldb-${CLANG_VERSION} && \
    rm llvm.sh && \
    \
    # Cleanup image.
    sudo apt remove -y software-properties-common gnupg && \
    sudo apt autoremove -y && \
    sudo apt clean && \
    sudo rm -rf /var/lib/apt/lists/*

ENV PATH=/lib/llvm-${CLANG_VERSION}/bin:${PATH}
