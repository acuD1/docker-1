# docker build -t ex00 .
# docker run -it --rm ex00

FROM alpine:3.9.3
RUN apk update && apk upgrade
RUN apk add vim
ENTRYPOINT vim
