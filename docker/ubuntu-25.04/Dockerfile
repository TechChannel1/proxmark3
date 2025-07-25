FROM ubuntu:25.04

ENV LANG=C
ENV DEBIAN_FRONTEND=noninteractive
# qtbase5-dev skipped
RUN apt-get update && \
    apt-get upgrade -y && \
    apt-get dist-upgrade -y && \
    apt-get install -y --no-install-recommends git ca-certificates build-essential cmake pkg-config libreadline-dev gcc-arm-none-eabi libnewlib-dev libbz2-dev liblz4-dev libbluetooth-dev libpython3-dev libssl-dev libgd-dev sudo && \
    apt-get clean

RUN apt-get install -y --no-install-recommends python3-minimal python3-pip python3-venv && \
    apt-get clean

RUN apt-get install -y opencl-dev && \
    apt-get clean

# Create rrg user => there is already a ubuntu user = 1000 that we need to move away
RUN usermod -u 999 ubuntu
RUN groupmod -g 999 ubuntu
RUN useradd -ms /bin/bash rrg
RUN passwd -d rrg
ARG UART_GID
# dialout group may already exist on another numeric ID than on host
RUN if [ -n "${UART_GID}" ]; then \
    groupadd -g ${UART_GID} mydialout || true; \
    usermod -aG ${UART_GID} rrg; \
  fi
RUN printf 'rrg ALL=(ALL) ALL\n' | tee -a /etc/sudoers

USER rrg
WORKDIR "/home/rrg"

CMD ["/bin/bash"]
