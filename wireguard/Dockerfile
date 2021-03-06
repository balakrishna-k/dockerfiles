# Usage:
#
# (this uses a custom kernel module hence the mounts)
# docker run --rm -it \
# 	--name wireguard \
# 	--privileged \
# 	-v /lib/modules:/lib/modules:ro \
# 	-v /usr/src:/usr/src:ro \
# 	r.j3ss.co/wireguard
#
FROM alpine:latest
LABEL maintainer "Jessie Frazelle <jess@linux.com>"

RUN apk add --no-cache \
	build-base \
	ca-certificates \
	libmnl-dev

# https://git.zx2c4.com/WireGuard/refs/
ENV WIREGUARD_VERSION 0.0.2018030

RUN set -x \
	&& apk add --no-cache --virtual .build-deps \
		git \
	&& git clone --depth 1 --branch "${WIREGUARD_VERSION}" https://git.zx2c4.com/WireGuard.git /wireguard \
	&& ( \
		cd /wireguard/src \
		&& make tools \
		&& make -C tools install \
		&& make -C tools clean \
	) \
	&& apk del .build-deps

COPY entrypoint.sh /usr/local/bin/entrypoint.sh

ENTRYPOINT [ "/usr/local/bin/entrypoint.sh" ]
CMD [ "wg" ]
