FROM ubuntu:20.04

COPY scripts/cross-apt-packages.sh /scripts/
RUN sh /scripts/cross-apt-packages.sh

COPY scripts/crosstool-ng-1.24.sh /scripts/
RUN sh /scripts/crosstool-ng-1.24.sh

COPY scripts/rustbuild-setup.sh /scripts/
RUN sh /scripts/rustbuild-setup.sh
USER rustbuild
WORKDIR /tmp

COPY host-x86_64/dist-riscv64-linux/build-toolchains.sh host-x86_64/dist-riscv64-linux/riscv64-unknown-linux-gnu.config /tmp/
RUN ./build-toolchains.sh

USER root

COPY scripts/sccache.sh /scripts/
RUN sh /scripts/sccache.sh

ENV PATH=$PATH:/x-tools/riscv64-unknown-linux-gnu/bin

ENV CC_riscv64gc_unknown_linux_gnu=riscv64-unknown-linux-gnu-gcc \
    AR_riscv64gc_unknown_linux_gnu=riscv64-unknown-linux-gnu-ar \
    CXX_riscv64gc_unknown_linux_gnu=riscv64-unknown-linux-gnu-g++

ENV HOSTS=riscv64gc-unknown-linux-gnu

ENV RUST_CONFIGURE_ARGS --enable-extended --enable-profiler --disable-docs
ENV SCRIPT python3 ../x.py dist --target $HOSTS --host $HOSTS
