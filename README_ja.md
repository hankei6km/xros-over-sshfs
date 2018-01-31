# xros-over-sshfs

このコンテナを起動しておき、Dockerホスト側から sshfs で `/containers` をマウントしておくと、稼働中の Docker コンテナのファイルに容易にアクセスできるようになります.

例)

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
    tty: true
    # cap_add:
    #   - SYS_ADMIN
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

以下の環境変数が利用できます.

* `ROOTPW`: root ユーザーのパスワード.
* `BUMP_AT_CONTAINER_AWAKENS`: `1` を指定すると、コンテナが開始される毎に自動マウントします(デフォルトではコンテナのファイルにアクセスするまでマウントされません).
* `AUTOFS_TIMEOUT`: アクセスが無い場合に umount するまでの時刻(秒). `0` で自動 umount しません.


## Security

xros-over-sshfs を起動してしまうと、接続先 Docker ホストや各コンテナに容易にアクセスできるようになってしまうので、注意してください.


## SSH host keys

xros-over-sshfs のイメージにホスト鍵は含まれていません.
コンテナ稼働時に自動的に作成します.
`docker-compose up` / `down` を行う場合、その都度鍵が再作成されるので注意してください.


## License

Copyright (c) 2018 hankei6km

Licensed under the MIT License. See LICENSE.txt in the project root.
