FROM alpine:latest as downloader

ARG VERSION=0000

RUN apk add curl unzip \
    && mkdir /temp/ \
    && curl -sSL 'https://www.terraria.org/system/dedicated_servers/archives/000/000/039/original/terraria-server-1405' --create-dirs -o /temp/terraria-server-${VERSION}.zip \
    && unzip -oqq /temp/terraria-server-${VERSION}.zip -d /temp/terraria-server-${VERSION} \
    && mkdir -p /out/ \
    && cp -r /temp/terraria-server-${VERSION}/${VERSION}/Linux/ /out/

COPY /config/ /out/Linux/config/

FROM ubuntu:18.04 as terraria

COPY --from=downloader /out/ /terraria/

RUN apt update \
    && apt -y install screen \
    && chmod u+x /terraria/Linux/TerrariaServer.bin.x86_64

VOLUME /terraria/config/

EXPOSE 7777/TCP

ENTRYPOINT cp --no-clobber /terraria/Linux/config/serverconfig.txt /terraria/config/serverconfig.txt \
    && screen /terraria/Linux/TerrariaServer.bin.x86_64 -config /terraria/config/serverconfig.txt
