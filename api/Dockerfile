# 1. Build Stage
FROM rust:1.78-slim AS builder
WORKDIR /usr/src/mediasaver
COPY Cargo.toml Cargo.lock ./
# Cache dependencies
RUN mkdir src && echo "fn main(){}" > src/main.rs && cargo build --release && rm -rf src
# Copy actual source code and build
COPY src ./src
COPY website ./website
RUN touch src/main.rs && cargo build --release

# 2. Run Stage (Smaller image)
FROM debian:bookworm-slim
RUN apt-get update && apt-get install -y ca-certificates libssl3 && rm -rf /var/lib/apt/lists/*
COPY --from=builder /usr/src/mediasaver/target/release/mediascraper /usr/local/bin/mediascraper
# Set environment defaults
ENV APP_HOST=0.0.0.0
ENV PORT=8080
EXPOSE 8080
CMD ["mediascraper"]
