# Architecture Notes

## Components

- Local browser
- SSH tunnel
- AWS EC2 Ubuntu host
- OpenClaw gateway
- Ollama runtime
- Cloud and local models

## Data Flow

1. The browser connects to `localhost:18789`.
2. SSH forwards that local port to `127.0.0.1:18789` on the EC2 instance.
3. OpenClaw receives the request on the EC2 host.
4. OpenClaw calls Ollama using the configured model.
5. Ollama routes the request to a cloud or local model.

## Why Loopback Binding Matters

Binding OpenClaw to loopback keeps the dashboard inaccessible from the public internet by default. Access is then provided through the SSH tunnel instead of exposing the port directly.

## Model Strategy

- Cloud-first model for better quality: `ollama/minimax-m2.7:cloud`
- Local fallback model for resilience: `ollama/llama3:latest`
- Experimental local model: `ollama/phi3:latest`

## Operational Lessons

- Exact model names must match between OpenClaw and Ollama
- Cloud models may hit session limits and require fallback options
- OAuth and device pairing flows are sensitive to stale state and interrupted sessions
