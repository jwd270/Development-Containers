ARG USERNAME=vscode
ARG USER_UID=1000
ARG USER_GID=$USER_UID

FROM ghdl/ext:broadway AS builder
RUN apt-get -y update && apt-get -y install git autoconf automake \
    autotools-dev curl python3 libmpc-dev libmpfr-dev libgmp-dev gawk \
    build-essential bison flex texinfo gperf libtool patchutils bc \
    zlib1g-dev libexpat-dev tcl-dev clang libreadline-dev pkg-config \
    libftdi-dev libboost-program-options-dev cmake gnat libffi-dev \
    graphviz xdot libboost-python-dev libboost-filesystem-dev \
    libboost-system-dev

WORKDIR /tmp/
RUN git clone https://github.com/riscv/riscv-gnu-toolchain
RUN git clone https://github.com/YosysHQ/yosys
RUN git clone https://github.com/YosysHQ/SymbiYosys
RUN git clone https://github.com/SRI-CSL/yices2
RUN git clone https://github.com/ghdl/ghdl-yosys-plugin
# RISC-V gcc build
WORKDIR /tmp/riscv-gnu-toolchain/build
RUN ../configure -q --with-arch=rv32i --with-abi=ilp32 && make
# FORMAL Tools build
WORKDIR /tmp/yosys
RUN make && make install
WORKDIR /tmp/SymbiYosys
RUN make && make install
WORKDIR /tmp/yices2
RUN autoconf && ./configure && make && make install
WORKDIR /tmp/ghdl-yosys-plugin
RUN make && make install

FROM ghdl/ext:broadway
COPY --from=builder /usr/local/ /usr/local/
COPY gui.sh /usr/local/share
RUN apt-get -y update && apt-get -y upgrade && apt-get -y install git
RUN pip install click
ENV DISPLAY=host.docker.internal:0.0

RUN groupadd --gid $USER_GID $USERNAME \
    && useradd --uid $USER_UID --gid $USER_GID -m $USERNAME \
    && apt-get update \
    && apt-get install -y sudo \
    && echo $USERNAME ALL=\(root\) NOPASSWD:ALL > /etc/sudoers.d/$USERNAME \
    && chmod 0440 /etc/sudoers.d/$USERNAME

USER $USERNAME
CMD ["bash"]