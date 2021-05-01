# About Rust Nickel.rs

<a target="_blank" rel="noopener noreferrer" href="https://rust-lang.org">
    <img alt="Rust Logo" align="right" src="/rust-nickel/logo.svg" width="200px" />
</a>

[Rust](https://rust-lang.org) is a multi-paradigm programming language designed for performance and safety, especially safe concurrency. Rust is syntactically similar to C++, but can guarantee memory safety by using a borrow checker to validate references. Rust achieves memory safety without garbage collection, and reference counting is optional.

## About Nickel.rs

[Nickel.rs](https://nickel-org.github.io/) is a simple and lightweight foundation for web applications written in Rust. Its API is inspired by the popular express framework for JavaScript.

## Sample Dockerfile

```Dockerfile
FROM ekidd/rust-musl-builder:stable as builder

WORKDIR /home/rust
RUN USER=root cargo new --bin rust-docker-web
WORKDIR /home/rust/rust-docker-web

COPY ./Cargo.toml ./Cargo.toml

RUN cargo build --release

ADD . ./

RUN cargo build --release

FROM alpine:latest

ARG APP=/usr/src/app

EXPOSE 6767

ENV TZ=Etc/UTC \
    APP_USER=appuser

RUN addgroup -S $APP_USER \
    && adduser -S -g $APP_USER $APP_USER

RUN apk update \
    && apk add --no-cache ca-certificates tzdata \
    && rm -rf /var/cache/apk/*

COPY --from=builder /home/rust/rust-docker-web/target/x86_64-unknown-linux-musl/release/rust-docker-web ${APP}/rust-docker-web
COPY ./static ${APP}/static

RUN chown -R $APP_USER:$APP_USER ${APP}
USER $APP_USER
WORKDIR ${APP}

CMD ["./rust-docker-web"]
```
