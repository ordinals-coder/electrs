# docker/Dockerfile
FROM debian:bookworm-slim as base
RUN apt-get update -qqy
RUN apt-get install -qqy librocksdb-dev curl

# Build stage
FROM base as electrs-build
RUN apt-get install -qqy cargo clang cmake
WORKDIR /build/electrs
COPY . .
ENV ROCKSDB_INCLUDE_DIR=/usr/include
ENV ROCKSDB_LIB_DIR=/usr/lib
RUN cargo install --locked --path .

# Final stage
FROM base as result
# Copy the binaries
COPY --from=electrs-build /root/.cargo/bin/electrs /usr/bin/electrs

# Create necessary directories
RUN mkdir -p /root/.bitcoin /root/.electrs /root/.electrs/db

# Set working directory
WORKDIR /root

# Start electrs
CMD ["electrs"]
