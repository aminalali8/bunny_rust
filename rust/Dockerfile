FROM ruanbekker/vscode-server-base:3.7.4
USER root

# start of your custom modifications

# Rust as the base image
ENV RUSTUP_HOME=/usr/local/rustup CARGO_HOME=/usr/local/cargo PATH=/usr/local/cargo/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin RUST_VERSION=1.51.0
RUN /bin/sh -c set -eux;     
RUN apt-get update;     apt-get install -y --no-install-recommends         ca-certificates         gcc         libc6-dev         wget         ;     
#RUN dpkgArch="$(dpkg --print-architecture)";     case "${dpkgArch##*-}" in         amd64) rustArch='x86_64-unknown-linux-gnu'; rustupSha256='ed7773edaf1d289656bdec2aacad12413b38ad0193fff54b2231f5140a4b07c5' ;;         armhf) rustArch='armv7-unknown-linux-gnueabihf'; rustupSha256='7a7b9d246ad63358705d8d4a7d5c2ef1adfec24525d1d5c44a7739e1b867e84d' ;;         arm64) rustArch='aarch64-unknown-linux-gnu'; rustupSha256='f80a0a792b3ab905ab4919474daf4d3f60e574fc6987e69bfba2fd877241a8de' ;;         i386) rustArch='i686-unknown-linux-gnu'; rustupSha256='4473c18286aa1831683a772706d9a5c98b87a61cc014d38063e00a63a480afef' ;;         *) echo >&2 "unsupported architecture: ${dpkgArch}"; exit 1 ;;     esac;    
ENV rustArch=x86_64-unknown-linux-gnu rustupSha256=ed7773edaf1d289656bdec2aacad12413b38ad0193fff54b2231f5140a4b07c5
RUN url="https://static.rust-lang.org/rustup/archive/1.23.1/${rustArch}/rustup-init";     wget "$url";     echo "${rustupSha256} *rustup-init" | sha256sum -c -; 
RUN chmod +x rustup-init;     ./rustup-init -y --no-modify-path --profile minimal --default-toolchain $RUST_VERSION --default-host ${rustArch};  
RUN rm rustup-init;     chmod -R a+w $RUSTUP_HOME $CARGO_HOME;    
RUN rustup --version 
RUN cargo --version
RUN rustc --version
RUN apt-get remove -y --auto-remove wget
RUN rm -rf /var/lib/apt/lists/*
RUN apt-get install -y vim

RUN USER=root cargo new --bin home/coder/workspace/holodeck
WORKDIR /home/coder/workspace/holodeck

# # 2. Copy our manifests
COPY ./Cargo.toml ./Cargo.toml

# # # 4. Now that the dependency is built, copy your source code
COPY ./src ./src

RUN cargo install --path .
RUN chown -R 1000:1000 /home/coder/

# end of your custom modifications

ENTRYPOINT ["/usr/bin/entrypoint.sh"]