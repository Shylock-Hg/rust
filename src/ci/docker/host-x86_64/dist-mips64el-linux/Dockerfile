FROM ubuntu:22.04

COPY scripts/cross-apt-packages.sh /scripts/
RUN sh /scripts/cross-apt-packages.sh

COPY scripts/crosstool-ng.sh /scripts/
RUN sh /scripts/crosstool-ng.sh

COPY scripts/rustbuild-setup.sh /scripts/
RUN sh /scripts/rustbuild-setup.sh
WORKDIR /tmp

COPY scripts/crosstool-ng-build.sh /scripts/
COPY host-x86_64/dist-mips64el-linux/mips64el-linux-gnu.defconfig /tmp/crosstool.defconfig
RUN /scripts/crosstool-ng-build.sh

COPY scripts/sccache.sh /scripts/
RUN sh /scripts/sccache.sh

ENV PATH=$PATH:/x-tools/mips64el-unknown-linux-gnu/bin

ENV \
    CC_mips64el_unknown_linux_gnuabi64=mips64el-unknown-linux-gnu-gcc \
    AR_mips64el_unknown_linux_gnuabi64=mips64el-unknown-linux-gnu-ar \
    CXX_mips64el_unknown_linux_gnuabi64=mips64el-unknown-linux-gnu-g++

ENV HOSTS=mips64el-unknown-linux-gnuabi64

ENV RUST_CONFIGURE_ARGS --enable-extended --disable-docs
ENV SCRIPT python3 ../x.py dist --host $HOSTS --target $HOSTS
