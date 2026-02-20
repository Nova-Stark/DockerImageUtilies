# Python 3.13 Free-threaded Docker Image

Custom Python 3.13.0 image compiled from source with the Global Interpreter Lock (GIL) disabled.

## Features

- **Python 3.13.0** compiled from source
- **Free-threaded** build (`--disable-gil`) for true parallelism
- **Optimized** with LTO (Link Time Optimization)
- **Shared library** enabled for extension modules
- **SQLite extensions** support enabled
- **Minimal footprint** using Debian Bookworm Slim
- **Non-root user** (python:999) for security

## Build

```bash
docker build -t python-free-threaded:3.13 .
```

## Usage

```bash
docker run -it --rm python-free-threaded:3.13 python3 -c "import sys; print(sys.version)"
```

### With Volume Mount

```bash
docker run -it --rm -v $(pwd):/app -w /app python-free-threaded:3.13 python3 script.py
```

### Interactive Shell

```bash
docker run -it --rm python-free-threaded:3.13 /bin/bash
```

## Build Details

| Setting | Value |
|---------|-------|
| Base Image | debian:bookworm-slim |
| Python Version | 3.13.0 |
| GIL | Disabled (--disable-gil) |
| Optimizations | Enabled (--enable-optimizations, --with-lto) |
| Shared Lib | Enabled |
| SQLite Extensions | Enabled |
| User | python (uid: 999, gid: 999) |

## Environment Variables

- `PYTHONDONTWRITEBYTECODE=1` - Prevents .pyc files
- `PYTHONUNBUFFERED=1` - Disables output buffering

## Note

Free-threaded Python is experimental. Some C extensions may not be compatible. Test thoroughly before production use.

## NOTICE

PLEASE CONTRIBUTE... 

~ *Thankyou*