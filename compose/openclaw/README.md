# OpenClaw Docker Compose

Personal AI Assistant with Telegram integration.

## Quick Start

### 1. Configure Environment

Copy `.env.example` to `.env` and fill in your credentials:

```bash
cp .env.example .env
```

Edit `.env` with your API keys:

- **AI Provider**: Choose one of Anthropic (Claude), OpenAI, MiniMax, Ollama, or Claude Web Session
- **Telegram Bot Token**: Get from [@BotFather](https://t.me/BotFather)

### 2. Get Telegram Bot Token

1. Open Telegram and search for **@BotFather**
2. Send `/newbot` command
3. Follow the prompts to create your bot
4. Copy the token provided by BotFather
5. Add to `.env` as `TELEGRAM_BOT_TOKEN`

### 3. Start OpenClaw

```bash
docker-compose up -d
```

### 4. Initial Setup

#### 4.1 Run Onboarding

```bash
docker-compose run --rm openclaw-cli onboard --no-install-daemon
```

Follow the prompts:
- Gateway bind: `lan`
- Gateway auth: `token`
- Gateway token: (use the token from your `.env` or generated one)
- Tailscale exposure: `Off`
- Install Gateway daemon: `No`

#### 4.2 Configure AI Provider

After onboarding, configure your AI provider inside the container:

```bash
docker-compose run --rm openclaw-cli providers add --provider anthropic
```

Or use the provider of your choice: `openai`, `minimax`, `ollama`, `claude-web`

#### 4.3 Connect Telegram

```bash
docker-compose run --rm openclaw-cli channels add --channel telegram --token $TELEGRAM_BOT_TOKEN
```

#### 4.4 Pair Your Telegram Account

Open your bot on Telegram and send `/start`. You'll receive a pairing code.

Approve the pairing:

```bash
docker-compose run --rm openclaw-cli pairing approve telegram <CODE>
```

## Usage

### View Logs

```bash
docker-compose logs -f openclaw-gateway
```

### Access CLI

```bash
docker-compose run --rm openclaw-cli
```

### Check Health

```bash
docker-compose exec openclaw-gateway node dist/index.js health --token $OPENCLAW_GATEWAY_TOKEN
```

### Restart

```bash
docker-compose restart
```

### Stop

```bash
docker-compose down
```

## Configuration

### Environment Variables

| Variable | Description | Required |
|----------|-------------|----------|
| `OPENCLAW_GATEWAY_TOKEN` | Gateway authentication token | Yes |
| `ANTHROPIC_API_KEY` | Anthropic Claude API key | No* |
| `OPENAI_API_KEY` | OpenAI API key | No* |
| `MINIMAX_API_KEY` | MiniMax API key | No* |
| `OLLAMA_BASE_URL` | Ollama server URL | No* |
| `TELEGRAM_BOT_TOKEN` | Telegram bot token | No |

*At least one AI provider is required.

### Data Persistence

- Config: Docker volume `openclaw-config`
- Workspace: Docker volume `openclaw-workspace`

## Access

- Gateway: http://localhost:18789
- Bridge: http://localhost:18790

## Documentation

- [OpenClaw Docs](https://docs.openclaw.ai)
- [Channel Setup](https://docs.openclaw.ai/channels)
