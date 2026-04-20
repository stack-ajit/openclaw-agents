# Setup Guide

## 1. EC2 Instance

Provision an Ubuntu EC2 instance and connect with SSH.

## 2. Install Ollama

```bash
curl -fsSL https://ollama.com/install.sh | sh
```

Verify:

```bash
ollama --version
ollama list
```

## 3. Pull Models

Example local models:

```bash
ollama pull llama3
ollama pull phi3
```

Cloud model example:

```bash
ollama signin
ollama run minimax-m2.7:cloud
```

## 4. Configure OpenClaw

Create or update `~/.openclaw/openclaw.json` using the example config in `configs/openclaw.example.json`.

## 5. Start OpenClaw

```bash
nohup openclaw gateway --force > log.txt 2>&1 &
```

Useful checks:

```bash
ps -ef | grep openclaw
ss -ltnp | grep 18789
tail -f /tmp/openclaw/openclaw-$(date +%F).log
```

## 6. Connect From Local Machine

Windows SSH tunnel example:

```powershell
ssh -N -L 18789:127.0.0.1:18789 -i "C:\Users\YOUR_NAME\Downloads\your-key.pem" ubuntu@YOUR_PUBLIC_IP
```

Then open:

```text
http://localhost:18789
```

## 7. Switch Models In Dashboard

Once multiple models are registered in the OpenClaw config, refresh the dashboard and switch models from the dropdown.

Suggested pattern:

- Use `minimax-m2.7:cloud` as the primary model
- Switch to `llama3:latest` when the cloud session is rate-limited

## 8. Recommended Cleanup

- Rotate any leaked tokens or API keys
- Remove sensitive data from screenshots before publishing
- Keep the example config sanitized before pushing to GitHub
