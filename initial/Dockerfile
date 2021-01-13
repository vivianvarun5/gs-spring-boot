ARG JAVA_VERSION="11u9-zulu-debian10"
ARG DEBIAN_VERSION="10.5"

FROM debian:${DEBIAN_VERSION} as debian

RUN apt-get -qq update && \
    apt-get -qq -y install libjpeg62-turbo && \
    apt-get -qq -y install zlib1g && \
    apt-get -qq -y install libpng16-16 && \
    apt-get -qq -y install liblcms2-2 && \
    apt-get -qq -y install libfreetype6 && \
    apt-get -qq -y install libexpat1 && \
    apt-get -qq -y install libfontconfig1 && \
    apt-get -qq -y install libuuid1

FROM mcr.microsoft.com/java/jre-headless:${JAVA_VERSION} as zulu

FROM gcr.io/distroless/cc-debian10

# copy the same libraries, excluding fonts, as would be included in the Distroless Java build from Google
COPY --from=debian /usr/lib/x86_64-linux-gnu/libjpeg.so.62 /usr/lib/x86_64-linux-gnu/libjpeg.so.62
COPY --from=debian /lib/x86_64-linux-gnu/libz.so.1 /lib/x86_64-linux-gnu/libz.so.1
COPY --from=debian /usr/lib/x86_64-linux-gnu/libpng16.so.16 /usr/lib/x86_64-linux-gnu/libpng16.so.16
COPY --from=debian /usr/lib/x86_64-linux-gnu/liblcms2.so.2 /usr/lib/x86_64-linux-gnu/liblcms2.so.2
COPY --from=debian /usr/lib/x86_64-linux-gnu/libfreetype.so.6 /usr/lib/x86_64-linux-gnu/libfreetype.so.6
COPY --from=debian /lib/x86_64-linux-gnu/libexpat.so.1 /lib/x86_64-linux-gnu/libexpat.so.1
COPY --from=debian /usr/lib/x86_64-linux-gnu/libfontconfig.so.1 /usr/lib/x86_64-linux-gnu/libfontconfig.so.1
COPY --from=debian /lib/x86_64-linux-gnu/libuuid.so.1 /lib/x86_64-linux-gnu/libuuid.so.1

ENV JAVA_HOME=/usr/lib/jvm/zre-hl-11-azure-amd64

COPY --from=zulu ${JAVA_HOME} ${JAVA_HOME}


# default Java to UTF-8
COPY --from=zulu /usr/lib/locale /usr/lib/locale
ENV LANG=C.UTF-8

# exec format doesn't support env variable expansion
# no default arguments are specified for java
ENTRYPOINT ["/usr/lib/jvm/zre-hl-11-azure-amd64/bin/java"]