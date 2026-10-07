# Docker Basic Commands

Basic Docker commands with real output: list, stop, remove, images, pull, run and prune.

## Contents

1. [List running containers](#1-list-running-containers)
2. [Stop and remove containers](#2-stop-and-remove-containers)
3. [List and remove images](#3-list-and-remove-images)
4. [Pull an image and run a container](#4-pull-an-image-and-run-a-container)
5. [Prune command](#5-prune-command)

---

## 1. List running containers

The `docker ps` command lists all running containers. It shows basic info such as the container ID, image, status, ports and name.

```bash
docker ps
```

```text
root@postgres:~# docker ps
CONTAINER ID   IMAGE         COMMAND                  CREATED             STATUS          PORTS                                         NAMES
8831386aedbc   nginx         "/docker-entrypoint.…"   3 seconds ago       Up 2 seconds    80/tcp                                        relaxed_murdock
117be7f58337   postgres:17   "docker-entrypoint.s…"   3 minutes ago       Up 3 minutes    5432/tcp                                      pg-restored
5815112a434e   postgres:17   "docker-entrypoint.s…"   About an hour ago   Up 25 minutes   0.0.0.0:5431->5432/tcp, [::]:5431->5432/tcp   pg1-persist
f7fe355ea11e   postgres:16   "docker-entrypoint.s…"   3 hours ago         Up 43 minutes   0.0.0.0:5434->5432/tcp, [::]:5434->5432/tcp   pg1-poc
```

To also see stopped containers, use `docker ps -a`.

---

## 2. Stop and remove containers

### Stop a running container

Use `docker stop` with the container ID or container name.

```bash
docker ps                      # find the container ID or name
docker stop <container-name>
```

```text
root@postgres:~# docker ps
CONTAINER ID   IMAGE         COMMAND                  CREATED             STATUS          PORTS                                         NAMES
8831386aedbc   nginx         "/docker-entrypoint.…"   2 minutes ago       Up 2 minutes    80/tcp                                        relaxed_murdock
117be7f58337   postgres:17   "docker-entrypoint.s…"   5 minutes ago       Up 5 minutes    5432/tcp                                      pg-restored
5815112a434e   postgres:17   "docker-entrypoint.s…"   About an hour ago   Up 27 minutes   0.0.0.0:5431->5432/tcp, [::]:5431->5432/tcp   pg1-persist
f7fe355ea11e   postgres:16   "docker-entrypoint.s…"   3 hours ago         Up 46 minutes   0.0.0.0:5434->5432/tcp, [::]:5434->5432/tcp   pg1-poc
root@postrges:~# docker stop 8831386aedbc
8831386aedbc

root@postgres:~# docker ps
CONTAINER ID   IMAGE         COMMAND                  CREATED             STATUS          PORTS                                         NAMES
117be7f58337   postgres:17   "docker-entrypoint.s…"   6 minutes ago       Up 6 minutes    5432/tcp                                      pg-restored
5815112a434e   postgres:17   "docker-entrypoint.s…"   About an hour ago   Up 28 minutes   0.0.0.0:5431->5432/tcp, [::]:5431->5432/tcp   pg1-persist
f7fe355ea11e   postgres:16   "docker-entrypoint.s…"   3 hours ago         Up 47 minutes   0.0.0.0:5434->5432/tcp, [::]:5434->5432/tcp   pg1-poc
root@postrges:~#
```

### Remove a stopped container

```bash
docker rm <container-name>
```

```text
root@postgres:~# docker ps -a
CONTAINER ID   IMAGE         COMMAND                  CREATED             STATUS                          PORTS                                         NAMES
8831386aedbc   nginx         "/docker-entrypoint.…"   3 minutes ago       Exited (0) About a minute ago                                                 relaxed_murdock
117be7f58337   postgres:17   "docker-entrypoint.s…"   7 minutes ago       Up 7 minutes                    5432/tcp                                      pg-restored
5815112a434e   postgres:17   "docker-entrypoint.s…"   About an hour ago   Up 28 minutes                   0.0.0.0:5431->5432/tcp, [::]:5431->5432/tcp   pg1-persist
f7fe355ea11e   postgres:16   "docker-entrypoint.s…"   3 hours ago         Up 47 minutes                   0.0.0.0:5434->5432/tcp, [::]:5434->5432/tcp   pg1-poc
8b1f9fd7b607   postgres:16   "docker-entrypoint.s…"   3 hours ago         Created                                                                       pg1-test
31a9315d95be   hello-world   "/hello"                 3 hours ago         Exited (0) 3 hours ago                                                        ecstatic_bassi
root@postgres:~# docker rm relaxed_murdock
relaxed_murdock
```

> **Note:**
> - You do not need the full container ID. The first few characters are enough, as long as they match only one container.
> - You can remove multiple containers in one command.

```text
[root@postgres ~]# docker ps -a
CONTAINER ID   IMAGE     COMMAND                  CREATED         STATUS                          PORTS     NAMES
a3c539c463c5   nginx     "/docker-entrypoint.…"   2 minutes ago   Exited (0) About a minute ago             funny_sammet
8d7644fc6a08   nginx     "/docker-entrypoint.…"   2 minutes ago   Exited (0) 2 minutes ago                  elegant_shamir

[root@postgres ~]# docker rm a3c53 8d764
a3c53
8d764
```

---

## 3. List and remove images

### See available images and their sizes

```bash
docker images
```

```text
root@postgres:~# docker images
                                                                                                                                                                    i Info →   U  In Use
IMAGE                ID             DISK USAGE   CONTENT SIZE   EXTRA
alpine:latest        28bd5fe8b56d         13MB         3.93MB
hello-world:latest   5dd0d3e6e255       25.9kB         9.49kB    U
nginx:latest         0d4374c710a9        241MB         66.2MB    U
postgres:16          e17e86066e5e        642MB          166MB    U
postgres:17          e38411452a46        645MB          167MB    U
root@postgres:~#
```

### Remove an image

> **Note:** Docker will not remove an image if any container (running or stopped) still uses it. Remove those containers first.

```bash
docker rmi <image-name>
# or
docker image rm <image-name-or-id>
```

```text
root@postgres:~# docker image rm nginx:latest
Untagged: nginx:latest
Deleted: sha256:0d4374c710a9649200e84f8ef8dbdd4fa76c0c107839cd50f1e42a63916b0f2e
```

---

## 4. Pull an image and run a container

### docker pull

```bash
docker pull nginx
```

```text
root@postgres:~# docker pull nginx
Using default tag: latest
latest: Pulling from library/nginx
7eb55399d6de: Pull complete
5d480233f531: Pull complete
746b934a8960: Pull complete
5508f6432d3e: Pull complete
f530c3e421fc: Pull complete
128fcc7b23b0: Pull complete
81dd0279e705: Download complete
3976f7b8a9d7: Download complete
Digest: sha256:0d4374c710a9649200e84f8ef8dbdd4fa76c0c107839cd50f1e42a63916b0f2e
Status: Downloaded newer image for nginx:latest
docker.io/library/nginx:latest
```

### docker run (background mode)

Use the `-d` option to run the container in the background (detached mode).

```bash
docker run -d nginx
```

```text
root@postgres:~# docker run -d nginx
8831386aedbce7e032aed6c86f4c174149d3767786f22a5de9f3311d884f28d5

root@postgres:~# docker ps
CONTAINER ID   IMAGE         COMMAND                  CREATED             STATUS          PORTS                                         NAMES
8831386aedbc   nginx         "/docker-entrypoint.…"   3 seconds ago       Up 2 seconds    80/tcp                                        relaxed_murdock
```

---

## 5. Prune command

`docker system prune` reclaims disk space by deleting unused Docker data.

By default it removes:

- Stopped containers
- Unused networks
- Dangling images
- Build cache

By default it leaves **volumes** and **tagged images** untouched.

### Step 1: Check what uses disk space

Summary:

```bash
docker system df
```

```text
root@postgres:~# docker system df
TYPE            TOTAL     ACTIVE    SIZE      RECLAIMABLE
Images          4         3         1.183GB   13.01MB (1%)
Containers      5         3         110.6kB   8.192kB (7%)
Local Volumes   8         4         241MB     96.89MB (40%)
Build Cache     0         0         0B        0B
```

Detailed breakdown with individual items:

```bash
docker system df -v
```

```text
root@postgres:~# docker system df -v
Images space usage:

REPOSITORY    TAG       IMAGE ID       CREATED        SIZE      SHARED SIZE   UNIQUE SIZE   CONTAINERS
postgres      16        e17e86066e5e   11 days ago    642MB     117.2MB       524.5MB       2
postgres      17        e38411452a46   11 days ago    645MB     117.2MB       528.2MB       2
alpine        latest    28bd5fe8b56d   2 months ago   13MB      0B            13.01MB       0
hello-world   latest    5dd0d3e6e255   5 months ago   25.9kB    0B            25.87kB       1

Containers space usage:

CONTAINER ID   IMAGE         COMMAND                  LOCAL VOLUMES   SIZE      CREATED             STATUS                   NAMES
117be7f58337   postgres:17   "docker-entrypoint.s…"   1               20.5kB    12 minutes ago      Up 12 minutes            pg-restored
5815112a434e   postgres:17   "docker-entrypoint.s…"   1               61.4kB    About an hour ago   Up 33 minutes            pg1-persist
f7fe355ea11e   postgres:16   "docker-entrypoint.s…"   1               20.5kB    3 hours ago         Up 52 minutes            pg1-poc
8b1f9fd7b607   postgres:16   "docker-entrypoint.s…"   1               4.1kB     3 hours ago         Created                  pg1-test
31a9315d95be   hello-world   "/hello"                 0               4.1kB     3 hours ago         Exited (0) 3 hours ago   ecstatic_bassi

Local Volumes space usage:

VOLUME NAME                                                        LINKS     SIZE
pg1-poc-data                                                       1         48.17MB
pg_data_restored                                                   1         47.85MB
146af4a5511598acc686db1311bb1d96d62cb095e0d1282630b6c7cc9381e0c1   0         47.98MB
7129a3e317bd07612fafca72615b84dacbb87818581d822e15dfc16429091c51   0         0B
817b9eab2410a43984fdc32a0a448a24cce54e30081d6eb036c17a701cd8a220   1         0B
c809b7f9997d2b639dd80f75f548da03a9610b37b48ce2004cdc33c2ccc3ef59   1         48.13MB
dd9366f9523e898ffe36af18ee05379dfc9faa866c497bdb5ff9bdbe535d598b   0         48.91MB
f9e1edf29d57f3324650c4560a481fd307980b3dd7fbb63588b8d4f80cc19085   0         0B

Build cache usage: 0B

CACHE ID   CACHE TYPE   SIZE      CREATED   LAST USED   USAGE     SHARED
```

### Step 2: Run the prune

Basic prune. It asks for confirmation first.

```bash
docker system prune
```

```text
root@postgres:~# docker system prune
WARNING! This will remove:
  - all stopped containers
  - all networks not used by at least one container
  - all dangling images
  - unused build cache

Are you sure you want to continue? [y/N] y
Deleted Containers:
8b1f9fd7b607b9ccf660b1519a53161a1d00edd6a050aee20be32a80c53f4201
31a9315d95be61afe63ac4cdca09c89efa3fc59848d20354e7451b3df40b91dc

Total reclaimed space: 8.192kB
```

> **Note:** Volumes are excluded from prune by default because they usually hold data you want to keep (databases, uploads, etc.). To also remove unused volumes, add the `--volumes` flag. Be careful: this deletes data permanently.
