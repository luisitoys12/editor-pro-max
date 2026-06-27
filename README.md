# Editor Pro Max OpenClaw

**Editor de Video con IA — Adaptado para OpenClaw + OpenCode CLI**

Built with Remotion + OpenCode CLI | React 19 | TypeScript | Whisper AI

> Fork adaptado por [@luisitoys12](https://github.com/luisitoys12) desde el proyecto original de [@Hainrixz](https://github.com/Hainrixz/editor-pro-max)  
> Original por [@soyenriquerocha](https://instagram.com/soyenriquerocha)

[English](#english) | [Español](#español)

---

## Español

### ¿Qué es esto?

Editor Pro Max OpenClaw convierte **OpenCode CLI** en un editor de video profesional. En lugar de arrastrar clips en una línea de tiempo, describes lo que quieres en lenguaje natural y OpenCode escribe el código. El proyecto usa [Remotion](https://remotion.dev) — un framework de React que renderiza videos de forma programática.

No hay interfaz que aprender. No hay configuraciones de exportación. Todo corre localmente en tu computadora.

### Diferencias con el original

| Característica | Original | Esta versión |
|---|---|---|
| Agente IA | Claude Code CLI | **OpenCode CLI** |
| Plataforma | Claude (Anthropic) | **OpenClaw / OpenCode** |
| Contexto del agente | `.claude/` + `CLAUDE.md` | `.opencode/` + `AGENTS.md` |
| Compatibilidad | Solo Claude | **Cualquier modelo vía OpenCode** |

### Inicio Rápido

```bash
git clone https://github.com/luisitoys12/editor-pro-max.git
cd editor-pro-max
opencode
/start
```

El comando `/start` instala dependencias, verifica la build y te muestra lo que puedes hacer.

### Qué Puedes Hacer

**Crear videos desde cero:**
> "Hazme un TikTok anunciando mi nueva estación de radio"
> "Crea una presentación de 5 slides sobre mi proyecto"
> "Construye un video de anuncio con efectos de partículas"

**Editar tu propio material:**
> "Agrega subtítulos a mi video de talking head"
> "Corta el silencio de mi grabación de podcast"
> "Extrae un clip de 30 segundos para Instagram"

**Renderizar para cualquier plataforma:**
> "Renderiza esto para TikTok" (1080x1920)
> "Exporta para YouTube" (1920x1080)
> "Haz una versión cuadrada para Twitter" (1080x1080)

### Características

| Categoría | Cantidad | Detalles |
|---|---|---|
| Componentes | 25 | Animaciones de texto, fondos, overlays, media, layouts, transiciones |
| Templates | 10 | TikTok, Instagram, YouTube, Presentación, Testimonio, Anuncio, Antes/Después, TalkingHead, PodcastClip |
| Skills de IA | 7 | Mejores prácticas de Remotion, diseño de movimiento, animaciones premiadas, FFmpeg |
| Scripts de Pipeline | 5 | Análisis de video, extracción de audio, transcripción Whisper, detección de silencio, remoción de fondo |
| Presets | 7 paletas, 8 gradientes, 12 easings, 5 fuentes, 9 dimensiones de plataforma |

### Cómo Funciona

```
TÚ                                OPENCODE CLI                   REMOTION
"Hazme un TikTok"  ──>  Escribe composición React  ──>  Renderiza a MP4
                         usando 25 componentes             1080x1920 @30fps
```

#### Ruta A: Crear desde cero

1. Dile a OpenCode qué video quieres
2. OpenCode crea una composición usando la librería de componentes
3. Vista previa con `npm run dev` (abre Remotion Studio)
4. Renderiza con `npx remotion render <id> out/video.mp4`

#### Ruta B: Editar video existente

1. Coloca tu video en `public/assets/video.mp4`
2. OpenCode ejecuta el pipeline:
   - `npx tsx scripts/analyze-video.ts` — extrae metadata
   - `npx tsx scripts/extract-audio.ts` — extrae audio para transcripción
   - `npx tsx scripts/transcribe.ts` — Whisper AI genera subtítulos palabra por palabra
   - `npx tsx scripts/detect-silence.ts` — encuentra silencios para cortar
3. OpenCode crea una composición editada (subtítulos automáticos, jump cuts, overlays)
4. Vista previa y renderizado

### Templates

| Template | Dimensiones | Carpeta | Uso |
|---|---|---|---|
| Showcase | 1920x1080 | Examples | Intro con marca propia |
| TikTok | 1080x1920 | Social | Hook + cuerpo + CTA |
| InstagramReel | 1080x1920 | Social | Titular + subtexto |
| YouTubeShort | 1080x1920 | Social | Título + partículas |
| Presentation | 1920x1080 | Content | Presentación multi-slide |
| Testimonial | 1920x1080 | Content | Cita + autor |
| Announcement | 1920x1080 | Promo | Lanzamiento de producto |
| BeforeAfter | 1920x1080 | Promo | Comparación con wipe |
| TalkingHeadEdit | 1920x1080 | Editing | Subtítulos + remoción de silencio |
| PodcastClip | 1080x1920 | Editing | Extracción de clip con subtítulos |

### Stack Tecnológico

| Tecnología | Propósito |
|---|---|
| [Remotion](https://remotion.dev) | Framework de renderizado de video basado en React |
| React 19 | Composición de video basada en componentes |
| TypeScript | Código con tipos seguros |
| [Whisper.cpp](https://github.com/ggerganov/whisper.cpp) | Transcripción IA local (timestamps por palabra) |
| FFmpeg | Extracción de audio, detección de silencio, procesamiento de video |
| [@imgly/background-removal](https://github.com/imgly/background-removal-js) | Remoción de fondo con IA |
| [OpenCode CLI](https://github.com/sst/opencode) | Agente IA que escribe y edita composiciones |

### Requisitos

- **Node.js 20+** (LTS recomendado)
- **macOS, Linux, o Windows** con WSL
- **OpenCode CLI** instalado (`npm install -g opencode-ai` o según la documentación oficial)

### Instalar OpenCode CLI

```bash
# Con npm
npm install -g opencode-ai

# O con bun
bun install -g opencode-ai

# Verificar instalación
opencode --version
```

---

## English

### What is this?

Editor Pro Max OpenClaw turns **OpenCode CLI** into a professional video editor. Instead of dragging clips on a timeline, you describe what you want in natural language and OpenCode writes the code. The project uses [Remotion](https://remotion.dev) — a React framework that renders videos programmatically.

No GUI to learn. No export settings to configure. No API keys needed (depending on your OpenCode model configuration). Everything runs locally on your machine.

### Quick Start

```bash
git clone https://github.com/luisitoys12/editor-pro-max.git
cd editor-pro-max
opencode
/start
```

The `/start` command installs everything, verifies the build, and shows you what you can do.

### What You Can Do

**Create videos from scratch:**
> "Make me a TikTok announcing my new radio station"
> "Create a 5-slide presentation about AI"
> "Build an announcement video with particle effects"

**Edit your own footage:**
> "Add captions to my talking head video"
> "Cut the dead air from my podcast recording"
> "Extract a 30-second clip for Instagram"

**Render for any platform:**
> "Render this for TikTok" (1080x1920)
> "Export for YouTube" (1920x1080)
> "Make a square version for Twitter" (1080x1080)

### Requirements

- **Node.js 20+** (LTS recommended)
- **macOS, Linux, or Windows** with WSL
- **OpenCode CLI** installed

### Tech Stack

| Technology | Purpose |
|---|---|
| [Remotion](https://remotion.dev) | React-based video rendering framework |
| React 19 | Component-based video composition |
| TypeScript | Type-safe code |
| [Whisper.cpp](https://github.com/ggerganov/whisper.cpp) | Local AI transcription (word-level timestamps) |
| FFmpeg | Audio extraction, silence detection, video processing |
| [@imgly/background-removal](https://github.com/imgly/background-removal-js) | AI background removal |
| [OpenCode CLI](https://github.com/sst/opencode) | AI agent that writes and edits compositions |

### License

MIT License. See [LICENSE](LICENSE) for details. Note: [Remotion](https://remotion.dev/license) and other dependencies have their own licenses.

---

Adapted for OpenClaw + OpenCode CLI by [@luisitoys12](https://github.com/luisitoys12)  
Original project by [@soyenriquerocha](https://instagram.com/soyenriquerocha)
