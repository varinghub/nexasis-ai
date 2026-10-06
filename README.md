# Nexasis AI

**Nexasis AI Public Beta** is a self-hosted AI assistant designed to work primarily with local Ollama models.

This repository is the **public distribution, documentation, and feedback hub** for Nexasis AI. The application source code is **not published in this repository**.

> Current public beta: **v1.0.1**

## Highlights

- Local Ollama chat
- Auto and Manual model selection
- Text, code, image, PDF, ZIP/source-project and supported file workflows
- Local RAG for document and project retrieval
- Conversation history and persistent application data
- Usage and request analytics
- Optional supported external AI providers
- Docker-based deployment

## Quick Start

### Requirements

- Docker
- Ollama running on the host
- At least one Ollama chat model

Pull the current public beta image:

```bash
docker pull varinb12/nexasis-ai-web:1.0.1
```

Create `docker-compose.yml`:

```yaml
services:
  nexasis-ai:
    image: varinb12/nexasis-ai-web:1.0.1
    container_name: nexasis-ai-web
    restart: unless-stopped
    ports:
      - "8091:8090"
    environment:
      PORT: 8090
      OLLAMA_BASE_URL: http://host.docker.internal:11434
      NEXASIS_DATA_DIR: /data
      NEXASIS_MAX_UPLOAD_MB: 100
      NEXASIS_MAX_DATA_MB: 256
      NEXASIS_MAX_PDF_PAGES: 500
      NEXASIS_MAX_IMPORT_MB: 128
      NEXASIS_MAX_CHAT_MB: 64
    extra_hosts:
      - "host.docker.internal:host-gateway"
    volumes:
      - nexasis_data:/data

volumes:
  nexasis_data:
```

Start it:

```bash
docker compose up -d
```

Open `http://localhost:8091`.

Application data is stored in the Docker volume and survives normal container recreation.

> **Important:** Do not run `docker compose down -v` unless you intentionally want to delete the persistent volume and its data.

## Ollama

Nexasis AI discovers models from the configured Ollama server.

Example chat model:

```bash
ollama pull qwen3.5:4b
```

For local RAG, configure an embedding model. The currently tested embedding model is:

```bash
ollama pull qwen3-embedding:0.6b
```

Model availability and hardware requirements depend on your Ollama installation and system resources.

## Updating

When a newer public image is announced, update the image tag in your Compose file and run:

```bash
docker compose pull
docker compose up -d
```

Keep the persistent volume to retain your application data.

## We Want Your Feedback

The purpose of this public beta is to learn from real users.

Use GitHub Issues for **bug reports**, **feature requests**, and **general beta feedback** about what you liked, disliked, found confusing, or want improved.

See the [Feedback Guide](docs/FEEDBACK.md) before posting.

When reporting a bug, include the Nexasis AI version, OS, browser, Docker version, Ollama version, model name, reproduction steps, and relevant **sanitized** logs/screenshots.

> Never post API keys, passwords, access tokens, private documents, confidential prompts, or other sensitive information in a public issue.

## Troubleshooting

See [Troubleshooting](docs/TROUBLESHOOTING.md) for common Docker, Ollama, model, RAG, and persistence checks.

## Privacy

Nexasis AI is intended to support local-first use with Ollama. Optional external providers may send request content to the provider you configure. Review your own configuration before using sensitive information.

The Docker image is distributed for public beta testing. **This repository does not currently publish the Nexasis AI application source code and is not an open-source source repository.**

## Support the Project

Nexasis AI is independently developed. If the project proves useful, voluntary support/donation options may be added later.

There is **no official donation link configured yet**. Do not send money to accounts or links claiming to represent Nexasis AI unless the link is published in this repository by its owner.

## Beta Notice

This is beta software. Back up important data before upgrades. Features, configuration, storage formats, and behavior may change as feedback is incorporated.

Thanks for testing Nexasis AI.
