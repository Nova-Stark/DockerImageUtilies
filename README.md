# Docker Image Utility

A collection of custom Docker images for various tech stacks.

## Available Images

| Image | Description | Directory |
|-------|-------------|-----------|
| Python 3.13 (Free-threaded) | Python 3.13.0 compiled from source with GIL disabled | [python/](python/) |

## Usage

Each directory contains its own Dockerfile and README with specific usage instructions.

```bash
cd <stack-name>
docker build -t <image-name> .
```

## Contributing

Feel free to add new Docker images for different tech stacks by creating a new directory with a Dockerfile and README.

~ *Thank You* ,

[NovaStark](https://github.com/Nova-Stark)