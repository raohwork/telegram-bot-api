# download qemu
FROM debian as qemu

RUN mkdir /qemu
WORKDIR /qemu
RUN apt-get update
RUN apt-get install -y wget
RUN wget -q -O - 'https://github.com/balena-io/qemu/releases/download/v3.0.0%2Bresin/qemu-3.0.0+resin-arm.tar.gz' | tar zxf - --strip-components 1

FROM arm32v7/debian as builder

COPY --from=qemu /qemu/qemu-arm-static /usr/bin
RUN apt-get update
RUN apt-get install -y make git zlib1g-dev libssl-dev gperf cmake clang libc++-dev libc++abi-dev wget
RUN git clone --recursive https://github.com/tdlib/telegram-bot-api.git build
WORKDIR /build
RUN rm -rf build && mkdir build
WORKDIR /build/build
RUN CXXFLAGS="-stdlib=libc++" CC=/usr/bin/clang CXX=/usr/bin/clang++ cmake -DCMAKE_BUILD_TYPE=Release -DCMAKE_INSTALL_PREFIX:PATH=.. ..
RUN cmake --build . --target install

FROM arm32v7/debian:stable-slim

COPY --from=qemu /qemu/qemu-arm-static /usr/bin
RUN apt-get update && \
        apt-get install -y libssl1.1 zlib1g libc++-x.y libc++abi-x.y && \
        apt-get clean && \
        rm -fr /var/lob/apt/lists/*
RUN mkdir /data
COPY --from=builder /build/bin/telegram-bot-api /usr/bin/telegram-bot-api
WORKDIR /data
VOLUME /data
ENTRYPOINT ["/usr/bin/telegram-bot-api"]

