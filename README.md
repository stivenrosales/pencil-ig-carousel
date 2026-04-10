# pencil-ig-carousel

A [Claude Code](https://claude.com/claude-code) skill to create Instagram, TikTok, and LinkedIn carousels inside [Pencil](https://pencil.so) using MCP tools, export them, and publish via Buffer API.

## What it does

Give Claude Code a news article, announcement, or topic, and it will:

1. **Plan** the carousel structure (hook → explain → features → CTA)
2. **Ask you for images** (IA-generated, screenshots, or existing assets)
3. **Build** each slide in Pencil using the MCP integration
4. **Export** at different scales (x1 for TikTok, x3 for Instagram) + PDF
5. **Publish** across Instagram, TikTok, and LinkedIn via the Buffer API with captions adapted per platform

## Prerequisites

- **Claude Code** installed
- **Pencil** installed with MCP configured
- **Buffer account** with API access
- **macOS or Linux** (uses shell environment variables)

## Installation

### 1. Clone this repo into your skills folder

```bash
git clone https://github.com/stivenrosales/pencil-ig-carousel.git ~/.claude/skills/pencil-ig-carousel
```

### 2. Configure your Buffer credentials

Create `~/.claude/.env.skills`:

```bash
# Buffer API — Primary account (Instagram + TikTok)
export BUFFER_API_KEY_PRIMARY="your-api-key-here"
export BUFFER_CHANNEL_IG="your-instagram-channel-id"
export BUFFER_CHANNEL_TIKTOK="your-tiktok-channel-id"

# Buffer API — Secondary account (LinkedIn, optional)
export BUFFER_API_KEY_SECONDARY="your-linkedin-api-key"
export BUFFER_CHANNEL_LINKEDIN="your-linkedin-channel-id"
```

Set file permissions:

```bash
chmod 600 ~/.claude/.env.skills
```

### 3. Load the credentials in your shell

Add to `~/.zshrc` (or `~/.bashrc`):

```bash
[ -f ~/.claude/.env.skills ] && source ~/.claude/.env.skills
```

Then open a new terminal, or run `source ~/.zshrc`.

### 4. Get your Buffer credentials

1. Go to [publish.buffer.com](https://publish.buffer.com) and log in
2. Generate an API key in Settings → Developer
3. Use the GraphQL query inside the skill ("Step 1: Get organization and channels") to find your channel IDs

## Usage

In Claude Code, open a `.pen` file in Pencil and ask:

```
Create an Instagram carousel about [your topic] using the pencil-ig-carousel skill
```

Claude Code will:
1. Present the slide structure for approval
2. Ask you for the images it needs
3. Build the carousel in Pencil
4. Export the slides
5. Publish them via Buffer

## Design System

The skill uses a predefined design system (colors, typography, spacing) tuned for high-engagement carousels. You can customize it by editing `SKILL.md` directly.

| Element | Light slide | Dark slide |
|---------|-------------|------------|
| Background | `#FAF9F6` + radial gradient | `#171717` |
| Accent | `#00A86B` | `#00A86B` |
| Title | `#171717` | `#FFFFFF` |
| Body | `#555555` | `#FFFFFFA0` |

Typography: Inter, sizes 20-74px.

## Platform-specific rules

The skill auto-adapts the caption per platform:

- **Instagram:** casual tone, hashtags, emojis allowed
- **TikTok:** max 150 characters including hashtags
- **LinkedIn:** professional tone, no hashtags, business value focus

## Security

- `~/.claude/.env.skills` should have `600` permissions
- Never commit this file — it's in `.gitignore`
- If you use a dotfiles repo, add `.env.skills` to its ignore list

## Contributing

Issues and PRs welcome. If you extend the design system or add platform integrations, open a PR.

## License

MIT — see [LICENSE](LICENSE)

## Author

Built by [@stivenrosales](https://github.com/stivenrosales)
