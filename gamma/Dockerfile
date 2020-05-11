FROM debian:buster-20200422-slim as builder

COPY alpha src/alpha
COPY beta src/beta
COPY gamma src/gamma
WORKDIR src/gamma
RUN ls -L -R
RUN ./build.sh > out.txt
RUN cat out.txt
