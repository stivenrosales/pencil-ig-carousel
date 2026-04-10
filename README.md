<div align="center">

# pencil-ig-carousel

**Skill de [Claude Code](https://claude.com/claude-code) para crear carruseles de Instagram, TikTok y LinkedIn dentro de [Pencil](https://pencil.so) y publicarlos automáticamente con la API de Buffer.**

[![Claude Code](https://img.shields.io/badge/Claude_Code-D97757?style=for-the-badge&logo=anthropic&logoColor=white)](https://claude.com/claude-code)
[![Pencil](https://img.shields.io/badge/Pencil-000000?style=for-the-badge&logo=pencil&logoColor=white)](https://pencil.so)
[![Buffer](https://img.shields.io/badge/Buffer-168EEA?style=for-the-badge&logo=buffer&logoColor=white)](https://buffer.com)
[![MCP](https://img.shields.io/badge/MCP-5E4AE3?style=for-the-badge&logo=modelcontextprotocol&logoColor=white)](https://modelcontextprotocol.io)

[![Instagram](https://img.shields.io/badge/Instagram-E4405F?style=for-the-badge&logo=instagram&logoColor=white)](https://instagram.com)
[![TikTok](https://img.shields.io/badge/TikTok-000000?style=for-the-badge&logo=tiktok&logoColor=white)](https://tiktok.com)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-0A66C2?style=for-the-badge&logo=linkedin&logoColor=white)](https://linkedin.com)

[![License: MIT](https://img.shields.io/badge/License-MIT-00A86B?style=for-the-badge)](LICENSE)
[![PRs Welcome](https://img.shields.io/badge/PRs-Bienvenidos-00A86B?style=for-the-badge)](#contribuir)
[![Made with Love](https://img.shields.io/badge/Hecho_con-❤️-E4405F?style=for-the-badge)](#autor)

</div>

---

## ¿Qué hace?

Le das a Claude Code un artículo, noticia, o tema, y el skill va a:

1. 📋 **Planear** la estructura del carrusel (hook → explicación → features → CTA)
2. 🖼️ **Pedirte las imágenes** (generadas con IA, screenshots, o assets propios)
3. 🎨 **Construir** cada slide en Pencil usando la integración MCP
4. 📤 **Exportar** en diferentes escalas (x1 para TikTok, x3 para Instagram) + PDF
5. 🚀 **Publicar** en Instagram, TikTok y LinkedIn vía Buffer API con captions adaptados por plataforma

---

## ✨ Características

- 🎨 **Sistema de diseño consistente** — colores, tipografía y layouts predefinidos
- 📐 **Formato 4:5 optimizado** para el feed de Instagram (1080x1350)
- 🔄 **Exportación multi-escala** — x1 para TikTok, x3 para Instagram, PDF para revisión
- ✍️ **Copy adaptado por plataforma** — hashtags en IG, 150 chars en TikTok, profesional en LinkedIn
- 🔐 **Credenciales seguras** — manejadas con variables de entorno, nunca hardcoded
- 🌎 **Multi-cuenta** — soporta cuentas separadas (ej: marca vs personal)

---

## 📦 Requisitos

- [Claude Code](https://claude.com/claude-code) instalado
- [Pencil](https://pencil.so) instalado con MCP configurado
- Cuenta de [Buffer](https://buffer.com) con acceso a la API
- macOS o Linux (usa variables de entorno del shell)

---

## 🚀 Instalación

### 1. Cloná el repo en tu carpeta de skills

```bash
git clone https://github.com/stivenrosales/pencil-ig-carousel.git ~/.claude/skills/pencil-ig-carousel
```

### 2. Configurá tus credenciales de Buffer

Creá el archivo `~/.claude/.env.skills`:

```bash
# Buffer API — Cuenta principal (Instagram + TikTok)
export BUFFER_API_KEY_PRIMARY="tu-api-key-aca"
export BUFFER_CHANNEL_IG="tu-channel-id-de-instagram"
export BUFFER_CHANNEL_TIKTOK="tu-channel-id-de-tiktok"

# Buffer API — Cuenta secundaria (LinkedIn, opcional)
export BUFFER_API_KEY_SECONDARY="tu-api-key-de-linkedin"
export BUFFER_CHANNEL_LINKEDIN="tu-channel-id-de-linkedin"
```

Configurá los permisos del archivo:

```bash
chmod 600 ~/.claude/.env.skills
```

### 3. Cargá las credenciales en tu shell

Agregá esta línea a tu `~/.zshrc` (o `~/.bashrc`):

```bash
[ -f ~/.claude/.env.skills ] && source ~/.claude/.env.skills
```

Después abrí una terminal nueva o corré `source ~/.zshrc`.

### 4. Obtené tus credenciales de Buffer

1. Entrá a [publish.buffer.com](https://publish.buffer.com) y logueate
2. Generá una API key en **Settings → Developer**
3. Usá la query GraphQL que está en el SKILL.md ("Step 1: Get organization and channels") para encontrar los channel IDs de cada red social

### 5. Verificá el setup

```bash
echo $BUFFER_API_KEY_PRIMARY
```

Si ves tu key, ya está listo. Si está vacío, revisá que abriste una terminal nueva después de editar `.zshrc`.

---

## 💡 Cómo usarlo

En Claude Code, abrí un archivo `.pen` en Pencil y pedile:

```
Crea un carrusel de Instagram sobre [tu tema] usando el skill pencil-ig-carousel
```

Claude Code va a:
1. Presentarte la estructura de los slides para que la apruebes
2. Pedirte las imágenes que necesita
3. Construir el carrusel en Pencil
4. Exportar los slides
5. Publicarlos vía Buffer en las redes que configuraste

---

## 🎨 Sistema de diseño

El skill usa un sistema de diseño predefinido (colores, tipografía, espaciado) pensado para carruseles de alto engagement. Podés personalizarlo editando `SKILL.md` directamente.

| Elemento | Slide claro | Slide oscuro |
|----------|-------------|--------------|
| Fondo | `#FAF9F6` + gradiente radial | `#171717` |
| Acento | `#00A86B` | `#00A86B` |
| Título | `#171717` | `#FFFFFF` |
| Body | `#555555` | `#FFFFFFA0` |

**Tipografía:** Inter, tamaños 20-74px.

---

## 📱 Reglas por plataforma

El skill adapta automáticamente el caption según la plataforma:

| Plataforma | Tono | Límite | Hashtags |
|------------|------|--------|----------|
| 📸 **Instagram** | Casual, con emojis | Sin límite | ✅ Permitidos |
| 🎵 **TikTok** | Directo y corto | 150 caracteres | ✅ Incluidos en el límite |
| 💼 **LinkedIn** | Profesional, valor de negocio | Sin límite | ❌ No (o máx 3) |

---

## 🔒 Seguridad

- El archivo `~/.claude/.env.skills` debe tener permisos `600`
- **Nunca commitees ese archivo** — ya está en el `.gitignore`
- Si usás un repo de dotfiles, agregá `.env.skills` a su lista de ignorados
- Las credenciales nunca salen de tu máquina local

---

## 🤝 Contribuir

Los issues y PRs son bienvenidos. Si extendés el sistema de diseño o agregás integraciones con otras plataformas, abrí un PR.

---

## 📄 Licencia

MIT — ver [LICENSE](LICENSE)

---

## 👤 Autor

Hecho por [@stivenrosales](https://github.com/stivenrosales)

[![YouTube](https://img.shields.io/badge/YouTube-FF0000?style=for-the-badge&logo=youtube&logoColor=white)](https://youtube.com/@stivenrosalesc)
[![Instagram](https://img.shields.io/badge/Instagram-E4405F?style=for-the-badge&logo=instagram&logoColor=white)](https://instagram.com/stivenrosalesc)

---

<div align="center">

**¿Te sirvió este skill?** ⭐ Dale una estrella al repo y compartilo con otros creadores.

</div>
