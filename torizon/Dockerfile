# This docker file builds Slint in a Torizon Cross-compilation container, along with the application
# the file comes with.

ARG TOOLCHAIN_ARCH=arm64
ARG IMAGE_ARCH=linux/arm64
ARG BASE_NAME=wayland-base
FROM torizon/debian-cross-toolchain-$TOOLCHAIN_ARCH:2-bullseye AS build
ARG TOOLCHAIN_ARCH
ARG RUST_TOOLCHAIN_ARCH=aarch64-unknown-linx-gnu

# Install an up-to-date cmake as well as ninja and make
RUN apt-get update && \
    apt-get install --assume-yes gpg wget && \
    wget -O - https://apt.kitware.com/keys/kitware-archive-latest.asc 2>/dev/null | gpg --dearmor - | tee /usr/share/keyrings/kitware-archive-keyring.gpg >/dev/null && \
    echo 'deb [signed-by=/usr/share/keyrings/kitware-archive-keyring.gpg] https://apt.kitware.com/ubuntu/ focal main' | tee /etc/apt/sources.list.d/kitware.list >/dev/null && \
    apt-get update && \
    apt-get install --assume-yes cmake ninja-build make && \
    rm -rf /var/lib/apt/lists/*

# Install Rust
ENV RUSTUP_HOME=/rust
ENV CARGO_HOME=/cargo 
ENV PATH=/cargo/bin:/rust/bin:$PATH

RUN curl https://sh.rustup.rs -sSf | sh -s -- -y
RUN rustup target add $RUST_TOOLCHAIN_ARCH

# Install Slint build dependencies (libxcb, etc.)
RUN apt-get update && \
    apt-get install --assume-yes libfontconfig1-dev:$TOOLCHAIN_ARCH libxcb1-dev:$TOOLCHAIN_ARCH libxcb-render0-dev:$TOOLCHAIN_ARCH libxcb-shape0-dev:$TOOLCHAIN_ARCH libxcb-xfixes0-dev:$TOOLCHAIN_ARCH libxkbcommon-dev:$TOOLCHAIN_ARCH python3 clang libstdc++-10-dev:$TOOLCHAIN_ARCH && \
    rm -rf /var/lib/apt/lists/*

# Don't require font-config when the compiler runs
ENV RUST_FONTCONFIG_DLOPEN=on

COPY torizon/$TOOLCHAIN_ARCH-toolchain.cmake $TOOLCHAIN_ARCH-toolchain.cmake

# Build Slint
RUN git clone https://github.com/slint-ui/slint --depth 1
RUN cd slint \
    && mkdir build \
    && cd build \
    && cmake -GNinja -DRust_CARGO_TARGET=$RUST_TOOLCHAIN_ARCH -DCMAKE_TOOLCHAIN_FILE=/$TOOLCHAIN_ARCH-toolchain.cmake .. \
       -DCMAKE_INSTALL_PREFIX=/slint \
       -DCMAKE_BUILD_TYPE=Release \
       -DSLINT_FEATURE_RENDERER_WINIT_SKIA=ON -DSLINT_FEATURE_RENDERER_WINIT_FEMTOVG=OFF \
       -DSLINT_FEATURE_BACKEND_WINIT=OFF -DSLINT_FEATURE_BACKEND_WINIT_WAYLAND=ON \
    && cmake --build . \
    && cmake --install .

COPY . /app
WORKDIR /app

RUN mkdir build \
    && cd build \
    && cmake -GNinja -DCMAKE_TOOLCHAIN_FILE=/$TOOLCHAIN_ARCH-toolchain.cmake -DCMAKE_PREFIX_PATH=/slint .. \
    && cmake --build .

# Create container for target
FROM --platform=$IMAGE_ARCH torizon/$BASE_NAME:2

RUN apt-get update \
    && apt-get install libfontconfig1 \
    && rm -rf /var/lib/apt/lists/*


COPY --from=build /slint/lib/libslint* /usr/lib
COPY --from=build /app/build/my_application /usr/bin

CMD /usr/bin/my_application