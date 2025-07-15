# 🗝️ KeyDB Docker Image (Alpine, Multi-Arch)

A custom-built Docker image for [KeyDB](https://docs.keydb.dev), optimized for Alpine and supporting both `amd64` and `arm64` architectures. This image includes both:

- `keydb-server`
- `keydb-cli`

## 🧱 Base Image

Built on Alpine Linux (`alpine:3.18`) for minimal size and fast boot time.

## 📦 Build Instructions

You need **Docker Buildx** with QEMU enabled to build for multiple platforms.

### 1. Set up Buildx (if not already)

```bash
docker buildx create --use
docker run --privileged --rm tonistiigi/binfmt --install all
```

### 2. Build for `amd64` and `arm64`

```bash
docker buildx build \
  --platform linux/amd64,linux/arm64 \
  -t yourdockerhubusername/keydb:alpine-multiarch \
  -f Dockerfile.alpine \
  . \
  --push
```

> 🔁 Replace `--push` with `--load` if you’re only testing locally.

---

## ▶️ Usage

Run the container with default KeyDB configuration:

```bash
docker run -d \
  --name keydb \
  -p 6379:6379 \
  yourdockerhubusername/keydb:alpine-multiarch
```

## 💻 CLI Access

Use the built-in `keydb-cli` inside the container:

```bash
docker exec -it keydb keydb-cli
```

Or connect from another container:

```bash
docker run -it --rm \
  --network container:keydb \
  yourdockerhubusername/keydb:alpine-multiarch keydb-cli -h 127.0.0.1 -p 6379
```

---

## 🔧 Configuration

Mount a config file and data volume as needed:

```bash
docker run -d \
  --name keydb \
  -p 6379:6379 \
  -v $(pwd)/keydb.conf:/etc/keydb/keydb.conf \
  -v keydb-data:/data \
  yourdockerhubusername/keydb:alpine-multiarch \
  keydb-server /etc/keydb/keydb.conf
```

---

## 🧪 Tested On

- Apple Silicon (M1/M2)
- Intel x86_64
- Docker Desktop 4.28+
- Buildx + QEMU

---

## 📜 License

MIT or Apache 2.0 — align with upstream KeyDB license.

---

## 🙌 Credits

- [KeyDB](https://github.com/Snapchat/KeyDB)
- [Alpine Linux](https://alpinelinux.org)
- [Docker Buildx](https://docs.docker.com/buildx/working-with-buildx/)

