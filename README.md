# Debian 11 (Bullseye) Ansible Test Image with Docker inside

Debian 11 (Bullseye) Docker container for Ansible playbook and role testing.

This repository is a fork from the amazing [geerlingguy](https://github.com/geerlingguy) [docker-debian11-ansible](https://github.com/geerlingguy/docker-debian11-ansible)

## Tags

- `latest`: Latest stable version of Ansible, with Python 3.x and Docker

## How to Build

This image is built on Docker Hub automatically any time the upstream OS container is rebuilt, and any time a commit is made or merged to the `main` branch. But if you need to build the image on your own locally, do the following:

1. [Install Docker](https://docs.docker.com/engine/installation/).
2. `cd` into this directory.
3. Run `docker build -t debian11-dind-ansible .`

## How to Use

### Raw image

1. [Install Docker](https://docs.docker.com/engine/installation/).
2. Pull this image from Docker Hub: `docker pull h33n0k/docker-debian11-ansible-dind:latest` (or use the image you built earlier, e.g. `debian11-dind-ansible`).
3. Run a container from the image

```bash
docker run \
    --detach \
    --privileged \
    --volume=/sys/fs/cgroup:/sys/fs/cgroup:rw \
    --cgroupns=host \
    h33n0k/docker-debian11-ansible-dind:latest
```

4. Use Ansible inside the container:

```bash
docker exec --tty \
    [container_id] \
    env TERM=xterm \
    ansible --version
```

5. Use Shell inside the container:

```bash
docker exec -it \
    [container_id] \
    env TERM=xterm \
    /bin/bash
```

### Ansible Molecule

In your `molecule.yml`, add the following platform:

```yaml
- name: Debian11
  image: h33n0k/docker-debian11-ansible-dind:latest
  pre_build_image: true
  command: /lib/systemd/systemd
  environment:
    container: docker
  volumes:
    - /sys/fs/cgroup:/sys/fs/cgroup:rw
    - /run
    - /run/lock
  privileged: true
  cgroupns_mode: host
  ansible_variables:
    ansible_python_interpreter: /usr/bin/python3.9
```

### Notes

I use this image for testing in an isolated environment—not for production—and the settings and configuration used may not be suitable for a secure and performant production environment. Use on production servers/in the wild at your own risk!

### Author

Created in 2025 by [h33n0k](https://github.com/h33n0k)

### License

This project is licensed under the [MIT License](./LICENSE).
