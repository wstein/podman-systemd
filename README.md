# Run systemd inside a Podman Container

Podman supports systemd support out of the box. Here are 4 examples using Fedora, RedHat, Oracle and OpenSUSE based images.

## Host configuration

The container_manage_cgroup boolean must be enabled for this to be allowed on an SELinux separated system.

```bash
sudo setsebool -P container_manage_cgroup true
```

## Build container

```Dockerfile
RUN curl -fsSL https://api.github.com/users/${USERNAME}/keys | jq --raw-output '.[] | .key' > /tmp/authorized_keys &&\
    chmod 600 /tmp/authorized_keys 
```

### Fedora

```bash
podman build --tag systemd \
             --build-arg BASEIMAGE=docker.io/fedora:35 \
             --build-arg USERNAME=$USERNAME redhat
```

### ORACLE Linux

```bash
podman build --tag systemd \
             --build-arg BASEIMAGE=docker.io/oraclelinux:8.5 \
             --build-arg USERNAME=$USERNAME redhat
```

### RHEL ubi

```bash
podman build --tag systemd \
             --build-arg BASEIMAGE=registry.access.redhat.com/ubi8/ubi:8.5 \
             --build-arg USERNAME=$USERNAME redhat
```

### OpenSUSE

```bash
podman build --tag systemd \
             --build-arg BASEIMAGE=docker.io/opensuse/leap:15.3 \
             --build-arg USERNAME=$USERNAME opensuse
```

## Run container

```bash
podman run -d --rm -p 2022:22 --systemd=true --name systemd systemd
```

## Connect container via ssh

```bash
 ssh -p 2022 localhost
 ```

## Links

- [How to run systemd in a container](https://developers.redhat.com/blog/2019/04/24/how-to-run-systemd-in-a-container)
