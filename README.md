![OpenWrt logo](include/logo.png)

OpenWrt Project is a Linux operating system targeting embedded devices. Instead
of trying to create a single, static firmware, OpenWrt provides a fully
writable filesystem with package management. This frees you from the
application selection and configuration provided by the vendor and allows you
to customize the device through the use of packages to suit any application.
For developers, OpenWrt is the framework to build an application without having
to build a complete firmware around it; for users this means the ability for
full customization, to use the device in ways never envisioned.

Sunshine!

## Stock OpenWrt firmware; only the LuCI WebUI has been added, with no other modifications.

---

Original project: https://github.com/openwrt/openwrt | [Official Documentation](https://github.com/upleung/openwrt-armv7/blob/main/Official%20Documentation.md)

This project: https://github.com/upleung/openwrt-armv7 | [DockerHub](https://hub.docker.com/repository/docker/mcgtekwrt/openwrt-armv7/)

---

## Instructions for Use


### Create a `macvlan` network
Execute the following command in the Armbian host terminal to bridge OpenWrt to your physical network interface (assuming your host interface is `eth0` and the main router's subnet is `192.168.1.x`):

```bash
docker network create -d macvlan \
  --subnet=192.168.1.0/24 \
  --gateway=192.168.1.1 \
  -o parent=eth0 openwrt_macvlan
```

### Docker run

```bash
docker run -d \
  --name openwrt \
  --restart always \
  --network openwrt_macvlan \
  --privileged \
  -v /lib/modules:/lib/modules:ro \
  -v /etc/localtime:/etc/localtime:ro \
  -v openwrt_overlay:/overlay \
  mcgtekwrt/openwrt-armv7:v25.12.5
```

---

### Docker-compose

```yaml
version: '3.8'

services:
  openwrt:
    image: mcgtekwrt/openwrt-armv7:v25.12.5
    container_name: openwrt
    restart: always
    privileged: true
    networks:
      - openwrt_macvlan
    volumes:
      - openwrt_overlay:/overlay
      - /lib/modules:/lib/modules:ro
      - /etc/localtime:/etc/localtime:ro
    entrypoint: ["/sbin/init"]

networks:
  openwrt_macvlan:
    external: true

volumes:
  openwrt_overlay:
```


## License

OpenWrt is licensed under GPL-2.0
