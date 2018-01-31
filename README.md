# xros-over-sshfs

By run xros-over-sshfs and mount it's `/containers` via sshfs on docker host,
You will be able to access running docker containers files easily.

Run new containers and access their files:

```bash
# run xrso-over-sshfs
$ cd xros-over-sshfs
$ docker-compose up

# to access containers file via `xros-over-sshfs:/containers`
$ sshfs root@172.18.0.2:/containers /path/to/containers
$ sudo docker run --rm -it -d --name hal9000 alpine
$ cat /path/to/containers/hal9000/etc/os-release
$ sudo docker run --rm -it -d --name sal9000 debian
$ cat /path/to/containers/sal9000/etc/os-release
```

## Usage

```YAML
version: '2'
services:
  xros-over:
    image: hankei6km/xros-over-sshfs:latest
    hostname: xros-over
    container_name: xros-over
    tty: false
    privileged: true
    command: ["bash", "/tmp/start.sh"]
    environment:
      - ROOTPW=<ultra supuer secret>
      - BUMP_AT_CONTAINER_AWAKENS=0
      - AUTOFS_TIMEOUT=300
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
```


## Customize

Effective environment variables.

* `ROOTPW`: root password
* `BUMP_AT_CONTAINER_AWAKENS`: `0`(default) - Mount container at to access to container's files. `1` - Mount automatically at the container wakens(`run` `start` etc.).
* `AUTOFS_TIMEOUT`: set timeout to autofs.


## Security

It will be possible to access each running containers files and docker host connected, be careful at launch xros-over-sshfs.


## SSH host keys

No SSH host keys was included on xros-over-sshfs image.
SSH host keys are generated at launch xros-over-sshfs container.


## License

Copyright (c) 2018 hankei6km

Licensed under the MIT License. See LICENSE.txt in the project root.
