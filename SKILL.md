---
name: pencil-ig-carousel
description: "Use when creating Instagram, TikTok or LinkedIn carousel posts inside Pencil (.pen files) using MCP tools and publishing via Buffer API. Triggers: pencil carousel, ig carousel, carousel design pencil, instagram slides, crear carrusel, carrusel instagram, publicar carrusel, programar post, buffer carousel"
---

# Pencil Instagram Carousel Builder

Create polished carousels in .pen files using Pencil MCP, export them, and publish/schedule via Buffer API.

## Setup (First Time Only)

This skill needs Buffer API credentials. Do NOT hardcode them in the skill.

### 1. Create the credentials file

Create `~/.claude/.env.skills` with your Buffer API keys:

```bash
# Buffer API — Primary account (Instagram + TikTok)
export BUFFER_API_KEY_PRIMARY="your-api-key-here"
export BUFFER_CHANNEL_IG="your-instagram-channel-id"
export BUFFER_CHANNEL_TIKTOK="your-tiktok-channel-id"

# Buffer API — Secondary account (LinkedIn, optional)
export BUFFER_API_KEY_SECONDARY="your-linkedin-api-key"
export BUFFER_CHANNEL_LINKEDIN="your-linkedin-channel-id"
```

Set file permissions: `chmod 600 ~/.claude/.env.skills`

### 2. Load the credentials in your shell

Add this line to `~/.zshrc` (or `~/.bashrc`):

```bash
[ -f ~/.claude/.env.skills ] && source ~/.claude/.env.skills
```

Then open a new terminal, or run `source ~/.zshrc`.

### 3. Get your Buffer API keys

1. Go to [publish.buffer.com](https://publish.buffer.com) and log in
2. Generate an API key in Settings → Developer
3. Use the GraphQL query in "Step 1: Get organization and channels" (below) to find your channel IDs

### 4. Verify the setup

```bash
echo $BUFFER_API_KEY_PRIMARY
```

If you see your key, you're ready. If empty, check that you opened a new terminal after editing `.zshrc`.

## When to Use

- User asks to create a carousel/carrusel in Pencil
- User has a .pen file open and wants social media slides
- User wants to turn an article, announcement, or news into carousel slides
- User provides images to include in carousel slides

## Platform Specs

| Platform | Dimensions | Aspect Ratio |
|----------|-----------|--------------|
| Instagram (recommended) | 1080 x 1350 | 4:5 |
| Instagram square | 1080 x 1080 | 1:1 |
| Stories | 1080 x 1920 | 9:16 |

**Default to 1080x1350 (4:5)** for carousels. More screen real estate in feed.

## Workflow

### Phase 1: Content Planning (BEFORE building)

1. **Read the source content** (article, announcement, news)
2. **Plan the carousel structure:** Define how many slides, what goes on each
3. **Write all the copy first:** Titles, body text, CTA. Apply copywriting rules
4. **Present the text structure to the user** as a numbered list:

```
Slide 1 (Hook): "Tu próximo empleado no duerme."
  → Body: "Claude ahora puede usar tu computadora..."
Slide 2 (Explain): "Trabaja en tu PC mientras tú no estás."
  → Body: "Claude controla tu mouse, teclado y navegador..."
...
```

5. **Ask the user for images:**

> "Esta es la estructura del carrusel. Antes de construirlo necesito las imágenes.
> Por favor envíame:
> - Las imágenes que quieras incluir (screenshots, fotos, iconos)
> - Deben estar en formato PNG o JPG (no SVG)
> - Deben estar guardadas en una carpeta real (Downloads, Documents, Desktop, etc.), no pegadas directamente en el chat
> - Si las pegas en el chat, guardalas primero en una carpeta y dame la ruta
>
> Los slides donde iría una imagen son: [listar slides con imagen]"

6. **Wait for user to provide images before building**

### Phase 2: Build in Pencil (AFTER receiving images)

```
1. get_editor_state() → identify .pen file
2. get_variables() → check existing design tokens
3. find_empty_space_on_canvas() → find placement area
4. Create ALL placeholder frames first (batch_design)
5. Build content slide by slide (batch_design, max 25 ops each)
6. Insert images using relative paths to .pen file
7. Remove placeholders
8. get_screenshot() → verify each slide visually
```

**IMPORTANT:** Never start Phase 2 without the images. The carousel looks incomplete without them.

## Carousel Structure (5-7 slides)

| Slide | Purpose | Background |
|-------|---------|------------|
| 1 | **Hook** — bold claim + hero image/icon | Light with accent gradient |
| 2-3 | **Explain** — what/how with images | Alternate light/dark |
| 4 | **Features/Details** — bullet list with icons | Light |
| 5 | **CTA** — share, save, visit link | Dark |

## Anatomy of a Slide

Every slide follows this vertical stack (layout: vertical, alignItems: center):

```
[Badge]           — cornerRadius:100, fill:accent, padding:[10,20]
[Title]           — fontSize:56-74, fontWeight:800, textAlign:center
[Card]            — cornerRadius:28, fill:#FFFFFF or #FFFFFF0A, padding:[36,40]
  [Body text]     — fontSize:26-30, inside the card
[Image frame]     — cornerRadius:20, clip:true, fill:{type:"image",...}
[Footer]          — your brand URL or handle
[Pagination]      — row of ellipses, active dot larger + green
[Brand line]      — "Contenido por [Brand]"
```

## Design System

### Colors

| Element | Light Slide | Dark Slide |
|---------|------------|------------|
| Background | `#FAF9F6` + radial gradient `#00A86B18` | `#171717` |
| Accent | `#00A86B` | `#00A86B` |
| Title | `#171717` | `#FFFFFF` |
| Body text | `#555555` | `#FFFFFFA0` |
| Card fill | `#FFFFFF` | `#FFFFFF0A` + stroke `#FFFFFF12` |
| Feature text | `#333333` | `#FFFFFFDD` |
| Muted text | `#999999` / `#BBBBBB` | `#FFFFFF50` / `#FFFFFF30` |

### Light Background with Gradient

```json
[
  {"type":"gradient","gradientType":"radial","center":{"y":0.15},
   "size":{"width":1.2,"height":0.6},"rotation":0,
   "colors":[{"color":"#00A86B18","position":0},{"color":"#FAF9F600","position":1}]},
  "#FAF9F6"
]
```

### Typography

| Element | Size | Weight |
|---------|------|--------|
| Badge text | 20-24px | 700 |
| Slide title | 56-74px | 800 |
| Body text | 26-30px | 400-500 |
| Feature items | 28px | 500 |
| Footer | 18-20px | 500 |
| CTA button | 28px | 700 |

All text uses `fontFamily: "Inter"`. Always set `fill` on text nodes.

### Components

**Badge:**
```javascript
badge=I(parent,{type:"frame",cornerRadius:100,fill:"#00A86B",padding:[10,20],alignItems:"center"})
I(badge,{type:"text",content:"BADGE TEXT",fontFamily:"Inter",fontSize:20,fontWeight:"700",fill:"#FFFFFF",letterSpacing:2})
```

**White Card (light slides):**
```javascript
card=I(parent,{type:"frame",cornerRadius:28,fill:"#FFFFFF",padding:[36,40],gap:24,layout:"vertical",alignItems:"center",width:920})
I(card,{type:"text",content:"Body text here",fontFamily:"Inter",fontSize:28,fontWeight:"normal",fill:"#555555",lineHeight:1.4,textAlign:"center",textGrowth:"fixed-width",width:"fill_container"})
```

**Dark Card (dark slides):**
```javascript
card=I(parent,{type:"frame",cornerRadius:28,fill:"#FFFFFF0A",padding:[36,40],gap:24,layout:"vertical",alignItems:"center",width:920,stroke:{fill:"#FFFFFF12",thickness:1}})
```

**Feature Row with Icon:**
```javascript
row=I(parent,{type:"frame",gap:14,width:"fill_container"})
I(row,{type:"icon_font",iconFontFamily:"lucide",iconFontName:"globe",width:32,height:32,fill:"#00A86B"})
I(row,{type:"text",content:"Feature description here",fontFamily:"Inter",fontSize:28,fontWeight:"500",fill:"#333333",lineHeight:1.3,textGrowth:"fixed-width",width:"fill_container"})
```

**Pagination Dots (per slide):**
```javascript
pag=I(parent,{type:"frame",gap:14,alignItems:"center"})
// Active dot = 14x14, fill:#00A86B. Inactive = 12x12, fill:#CCCCCC (light) or #FFFFFF40 (dark)
I(pag,{type:"ellipse",width:14,height:14,fill:"#00A86B"})  // active
I(pag,{type:"ellipse",width:12,height:12,fill:"#CCCCCC"})  // inactive
```

**CTA Button:**
```javascript
cta=I(parent,{type:"frame",cornerRadius:100,fill:"#00A86B",padding:[20,60],alignItems:"center",justifyContent:"center"})
I(cta,{type:"text",content:"Button Text",fontFamily:"Inter",fontSize:28,fontWeight:"700",fill:"#FFFFFF"})
```

**Image Frame:**
```javascript
I(parent,{type:"frame",cornerRadius:20,width:880,height:500,clip:true,
  fill:{type:"image",url:"./image-name.png",mode:"fill"},
  stroke:{fill:"#E0E0E0",thickness:1}})
```

Image `url` must be **relative to the .pen file**. Use `./filename.png` for files in the same directory. SVGs don't work as image fills; use PNG/JPG only.

**Image checklist before building:**
- Images must be PNG or JPG (never SVG)
- Images must be in a carpeta real del sistema (Downloads, Documents, Desktop, etc.)
- Las imágenes de cache temporal (`.claude/image-cache/`) NO funcionan
- Si el usuario pega imágenes en el chat, pedirle que las guarde en una carpeta real primero
- La ruta en el fill debe ser relativa al archivo .pen: `../../ruta/a/imagen.png`

**Hero Image with Background Color:**
```javascript
I(parent,{type:"frame",width:300,height:300,cornerRadius:36,clip:true,
  fill:[{type:"color",color:"#E8825A"},{type:"image",url:"./icon.png",mode:"fill"}]})
```

## Copywriting Rules

- **Hook slide:** Bold, curiosity-driven. "Tu proximo empleado no duerme." not "Nuevo feature de Claude"
- **No guiones/dashes** in any text
- **One idea per slide**
- Max 30-40 words per content slide
- Titles: short, punchy, benefit-driven
- Body: conversational, direct, no jargon
- CTA: focused on sharing/saving, not just "follow"
- Always end with brand attribution
- **Nunca llamar "carrusel" al post** en el copy. Solo referirse como "post"
- **Nunca usar CTAs autorreferenciales** como "este carrusel te va a interesar", "guarda este post", "si te gustó comparte". Dejar que el contenido hable por sí solo

### Copy por plataforma

**Instagram:** Casual, con hashtags, emojis opcionales. Caption largo OK.

**TikTok:** Max 150 caracteres incluyendo hashtags. Directo y corto.

**LinkedIn:** Tono profesional orientado a valor de negocio:
- Abrir con un problema real del negocio
- Usar primera persona ("Nosotros lo resolvimos")
- Listar resultados concretos numerados
- Sin hashtags (o máximo 3 relevantes)
- Sin CTAs tipo "guarda esto" o "comparte este post"
- Etiquetar empresas mencionadas con annotations
- Cerrar con el resultado, no con una invitación

## Slide Spacing

| Property | Value |
|----------|-------|
| Frame padding | `[60,80]` |
| Gap between elements | `28-40` |
| Card internal padding | `[28-36, 36-40]` |
| Card internal gap | `20-24` |
| Feature list gap | `24` |
| Card width | `920` (of 1080) |
| Image width | `880` |

## Frame Placement

Carousels go **below** existing content. Use `find_empty_space_on_canvas(direction:"bottom")`. Space slides horizontally with 100px gaps:

```
Slide 1: x=startX, y=startY
Slide 2: x=startX+1180, y=startY
Slide 3: x=startX+2360, y=startY
...
```

## Phase 3: Export

Use `export_nodes` from Pencil MCP to export slides:

```
Export x1 (TikTok):  scale: 1  → 1080x1350px
Export x3 (Instagram): scale: 3  → 3240x4050px
Export PDF: format: "pdf" → all slides in one document
```

Create a folder structure:
```bash
mkdir -p "Downloads/Carrusel-[nombre]/x1" "Downloads/Carrusel-[nombre]/x3"
```

Export all slide nodeIds to each folder + a PDF to the root.

## Phase 4: Publish via Buffer API

### Authentication

```
Endpoint: https://api.buffer.com (GraphQL)
Header: Authorization: Bearer {API_KEY}
Header: Content-Type: application/json
```

User-Agent must be set or requests get 403.

**Buffer API Keys (environment variables):**

Credentials live in `~/.claude/.env.skills` (auto-loaded from `~/.zshrc`).
Read values with `echo $VARIABLE` via the Bash tool. NEVER hardcode them in the skill.

See the **Setup** section at the top of this file for how to configure these variables.

| Account | Env var API Key | Env var Channel |
|---------|-----------------|-----------------|
| Primary (Instagram) | `$BUFFER_API_KEY_PRIMARY` | `$BUFFER_CHANNEL_IG` |
| Primary (TikTok) | `$BUFFER_API_KEY_PRIMARY` | `$BUFFER_CHANNEL_TIKTOK` |
| Secondary (LinkedIn) | `$BUFFER_API_KEY_SECONDARY` | `$BUFFER_CHANNEL_LINKEDIN` |

> You can rename these variables or add more accounts (e.g., `$BUFFER_API_KEY_CLIENT1`).
> The structure is flexible — adjust to your own setup.

If a variable is empty, tell the user to verify that:
1. The `~/.claude/.env.skills` file exists
2. `~/.zshrc` contains `[ -f ~/.claude/.env.skills ] && source ~/.claude/.env.skills`
3. The terminal session was restarted (or they ran `source ~/.zshrc`)

### Step 1: Get organization and channels

```graphql
query { account { organizations { id name } } }
```

```graphql
query { channels(input: { organizationId: "ORG_ID" }) { id name displayName service timezone } }
```

### Step 2: Get suggested posting times

```graphql
query { channel(input: { id: "CHANNEL_ID" }) { postingSchedule { day times } timezone } }
```

### Step 3: Upload images to public URL

Images must be publicly accessible URLs. Use `tmpfiles.org` for temporary hosting:

```bash
curl -s -F "file=@image.png" https://tmpfiles.org/api/v1/upload
# Response: {"data":{"url":"http://tmpfiles.org/12345/image.png"}}
# Convert to direct link: replace tmpfiles.org/ with tmpfiles.org/dl/
```

### Step 4: Create scheduled posts

**Key parameters:**
- `schedulingType: automatic` → publishes automatically (NOT `notification` which only sends a reminder)
- `mode: customScheduled` → publish at specific time
- `dueAt` → ISO 8601 datetime in UTC
- `saveToDraft: true` → saves as draft instead of scheduling

**Instagram carousel:**
```graphql
mutation CreatePost { createPost(input: {
  text: """Caption with hashtags"""
  channelId: "IG_CHANNEL_ID"
  schedulingType: automatic
  mode: customScheduled
  dueAt: "2026-03-30T03:57:00.000Z"
  metadata: { instagram: { type: carousel, shouldShareToFeed: true } }
  assets: { images: [
    {url: "https://public-url/slide1.png"}
    {url: "https://public-url/slide2.png"}
  ] }
}) { ... on PostActionSuccess { post { id text } } ... on MutationError { message } } }
```

**TikTok carousel:**
```graphql
mutation CreatePost { createPost(input: {
  text: """Short caption max 150 chars"""
  channelId: "TIKTOK_CHANNEL_ID"
  schedulingType: automatic
  mode: customScheduled
  dueAt: "2026-03-29T17:00:00.000Z"
  metadata: { tiktok: { title: "Short title" } }
  assets: { images: [
    {url: "https://public-url/slide1.png"}
  ] }
}) { ... on PostActionSuccess { post { id text } } ... on MutationError { message } } }
```

### Platform-specific rules

| Platform | Image Scale | Caption Limit | Metadata Required |
|----------|------------|---------------|-------------------|
| Instagram | x3 (3240x4050) | No limit | `instagram: { type: carousel, shouldShareToFeed: true }` |
| TikTok | x1 (1080x1350) | 150 chars | `tiktok: { title: "..." }` |

### ShareMode enum values

`addToQueue` | `shareNow` | `shareNext` | `customScheduled` | `recommendedTime`

### SchedulingType enum values

- `automatic` → Buffer publishes automatically (use this)
- `notification` → Only sends a mobile reminder (avoid)

### Instagram PostType enum values

`post` | `reel` | `story` | `carousel`

### Buffer API gotchas

- Use `http.client` in Python, NOT `urllib` (gets 403 from Cloudflare)
- Always set `User-Agent: Mozilla/5.0` header
- Buffer rejects duplicate posts (same text + images). Change caption slightly if reposting
- TikTok captions max 150 characters including hashtags
- Convert timezone to UTC for `dueAt`. Lima = UTC-5
- API cannot edit or delete posts. Remove duplicates manually from Buffer UI

## Common Mistakes

| Mistake | Fix |
|---------|-----|
| SVG as image fill | Convert to PNG first, SVGs won't render |
| Image path not relative | Use `./filename.png` relative to .pen file |
| Text without `fill` | Always set fill on text, it's invisible by default |
| Overcrowded slide | Max 30-40 words, one idea per slide |
| No pagination dots | Add to every slide showing position |
| Missing brand attribution | Always include brand footer + "Contenido por X" |
| Titles inside cards | Keep titles outside cards, only body text inside |
| Generic copy | Use curiosity hooks, benefit-driven language |
| `schedulingType: notification` | Use `automatic` for auto-publish |
| TikTok caption too long | Max 150 chars including hashtags |
| Images not public URLs | Upload to tmpfiles.org first, use /dl/ direct links |
| Duplicate post error | Change caption text slightly |
