# Kafka Connect on a glibc base.
#
# The v4 connector loads the Snowpipe Streaming Rust core over JNI, which needs glibc >= 2.26.
# The apache/kafka image is Alpine/musl, so the task dies at startup with
#   Error loading shared library libgcc_s.so.1: No such file or directory
# and the connector's own diagnosis says exactly that. Same constraint as production: the Strimzi
# image is UBI based, and an Alpine one would fail identically.
#
# Kafka itself still comes from the apache/kafka image - 4.1, the version the connector compiles
# against - only the runtime underneath changes.
FROM apache/kafka:4.1.0 AS kafka

FROM eclipse-temurin:21-jre
COPY --from=kafka /opt/kafka /opt/kafka
ENV PATH="/opt/kafka/bin:${PATH}"
WORKDIR /opt/kafka
CMD ["/opt/kafka/bin/connect-distributed.sh", "/opt/kafka/config/connect-distributed-local.properties"]
