FROM ubuntu:22.04

ENV DEBIAN_FRONTEND=noninteractive

# Install base build tools and LLVM 19.
RUN apt-get update && apt-get install -y \
    build-essential cmake curl git gnupg lsb-release ninja-build \
    patchelf pkg-config software-properties-common wget \
    debootstrap qemu-user-static binfmt-support \
    && rm -rf /var/lib/apt/lists/*

# Add LLVM 19 repository.
RUN wget -qO- https://apt.llvm.org/llvm-snapshot.gpg.key | gpg --dearmor -o /usr/share/keyrings/llvm.gpg \
    && echo "deb [signed-by=/usr/share/keyrings/llvm.gpg] http://apt.llvm.org/jammy/ llvm-toolchain-jammy-19 main" \
    > /etc/apt/sources.list.d/llvm-19.list \
    && apt-get update && apt-get install -y \
    clang-19 lld-19 llvm-19 \
    && rm -rf /var/lib/apt/lists/*

# Install riscv64 cross-compiler.
RUN apt-get update && apt-get install -y \
    gcc-riscv64-linux-gnu g++-riscv64-linux-gnu \
    && rm -rf /var/lib/apt/lists/*

# Create riscv64 chroot with dev packages needed for building.
RUN qemu-debootstrap --arch=riscv64 --include=\
libasound2-dev,\
libcurl4-openssl-dev,\
libdbus-1-dev,\
libdecor-0-dev,\
libegl-dev,\
libevdev-dev,\
libfontconfig-dev,\
libfreetype-dev,\
libgudev-1.0-dev,\
libharfbuzz-dev,\
libinput-dev,\
libopengl-dev,\
libpipewire-0.3-dev,\
libpulse-dev,\
libssl-dev,\
libudev-dev,\
libwayland-dev,\
libx11-dev,\
libx11-xcb-dev,\
libxcb1-dev,\
libxcursor-dev,\
libxcb-composite0-dev,\
libxcb-damage0-dev,\
libxcb-glx0-dev,\
libxcb-icccm4-dev,\
libxcb-image0-dev,\
libxcb-keysyms1-dev,\
libxcb-present-dev,\
libxcb-randr0-dev,\
libxcb-render0-dev,\
libxcb-render-util0-dev,\
libxcb-shape0-dev,\
libxcb-shm0-dev,\
libxcb-sync-dev,\
libxcb-util-dev,\
libxcb-xfixes0-dev,\
libxcb-xinput-dev,\
libxcb-xkb-dev,\
libxext-dev,\
libxfixes-dev,\
libxi-dev,\
libxkbcommon-dev,\
libxkbcommon-x11-dev,\
libxrandr-dev,\
libxss-dev,\
libxtst-dev,\
zlib1g-dev \
    jammy /riscv64-chroot http://ports.ubuntu.com/ubuntu-ports

# Clean up chroot caches.
RUN rm -rf /riscv64-chroot/var/cache/apt/archives/*.deb \
    /riscv64-chroot/var/lib/apt/lists/*
