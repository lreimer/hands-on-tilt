FROM gcr.io/distroless/base-debian11

ENV GIN_MODE=release
ENV PORT=9090

COPY favicon.ico /
COPY hello-tilt-go /

CMD ["/hello-tilt-go"]