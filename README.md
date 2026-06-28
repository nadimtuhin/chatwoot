# chatwoot

A Docker-based deployment configuration for [Chatwoot](https://github.com/chatwoot/chatwoot), an open-source customer support platform. This repository contains a custom Dockerfile and supporting configuration for self-hosted deployments of Chatwoot v2.11.0.

## What it is

Chatwoot is a multi-channel customer communication platform that lets you manage conversations across email, live chat, and social messaging in one inbox. This repository wraps the official Chatwoot Docker image with a custom entrypoint to support deployment customizations.

## Why it exists

The upstream Chatwoot image requires specific entrypoint configuration for certain hosting environments. This fork pins to a known-stable version and configures the Docker entrypoint explicitly, making it suitable for consistent self-hosted deployments.

## Prerequisites

- Docker 20.10 or later
- Docker Compose (optional, recommended for full-stack deployment)
- A PostgreSQL database
- A Redis instance

## Installation

Clone the repository:

```bash
git clone https://github.com/nadimtuhin/chatwoot.git
cd chatwoot
```

Build the Docker image:

```bash
docker build -t chatwoot-custom .
```

## Usage

Run the container:

```bash
docker run -d \
  -e SECRET_KEY_BASE=your_secret_key \
  -e DATABASE_URL=postgresql://user:pass@host/chatwoot \
  -e REDIS_URL=redis://host:6379 \
  -p 3000:3000 \
  chatwoot-custom
```

For a full deployment with all required services, use the official [Chatwoot Docker Compose guide](https://www.chatwoot.com/docs/self-hosted/deployment/docker) and replace the image reference with this custom build.

## Configuration

The Dockerfile pins to `chatwoot/chatwoot:v2.11.0`. To upgrade, change the base image version in the `FROM` line and rebuild.

Key environment variables (see [Chatwoot docs](https://www.chatwoot.com/docs/self-hosted/configuration/environment-variables) for the full list):

| Variable | Description |
|---|---|
| `SECRET_KEY_BASE` | Rails secret key (required) |
| `DATABASE_URL` | PostgreSQL connection URL |
| `REDIS_URL` | Redis connection URL |
| `RAILS_ENV` | Set to `production` for production deployments |

## Dockerfile

```dockerfile
FROM chatwoot/chatwoot:v2.11.0
RUN chmod +x docker/entrypoints/rails.sh
ENTRYPOINT ["docker/entrypoints/rails.sh"]
```

## Contributing

1. Fork the repository
2. Create a feature branch: `git checkout -b feature/my-change`
3. Commit with a descriptive message: `git commit -m 'feat: describe change'`
4. Push and open a pull request

See [CONTRIBUTING.md](CONTRIBUTING.md) for details and [CODE_OF_CONDUCT.md](CODE_OF_CONDUCT.md) for community standards.

## License

See [LICENSE](LICENSE).
