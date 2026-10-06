# Nexasis AI Troubleshooting

## Nexasis AI cannot detect Ollama

Verify Ollama is running on the host:

```bash
curl http://localhost:11434/api/tags
```

The recommended Docker configuration uses `http://host.docker.internal:11434`.

On Linux, keep:

```yaml
extra_hosts:
  - "host.docker.internal:host-gateway"
```

Also make sure Ollama is configured so it can be reached from the container.

## Check the container

```bash
docker ps
docker logs nexasis-ai-web
```

## Check application health

```bash
curl http://localhost:8091/health
```

## A model is missing

```bash
ollama list
```

Then refresh detected models in Nexasis AI.

## RAG is unavailable

The currently tested embedding model is:

```bash
ollama pull qwen3-embedding:0.6b
```

Make sure RAG is enabled and the relevant document/project has been indexed.

## Data persistence

Nexasis AI stores persistent application data under `/data` inside the container. The recommended Compose configuration maps this to the `nexasis_data` volume.

Do not run `docker compose down -v` unless you intentionally want to remove that volume and its data.

## Still having a problem?

Open a Bug report in this repository. Include sanitized environment details, reproduction steps, logs, and screenshots where useful. Never publish secrets or private content.
