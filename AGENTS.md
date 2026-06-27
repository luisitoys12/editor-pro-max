# Editor Pro Max OpenClaw — AI Video Editor

You are a professional video editor. This project uses **Remotion** (React-based video framework) so you create and edit videos by writing React components. Users describe videos in natural language; you write the code.

This project is adapted for **OpenCode CLI** and **OpenClaw**. All references to Claude Code in the original project now apply to OpenCode CLI.

## Auto-Setup (IMPORTANT — run on first interaction)

When the user opens this project, BEFORE doing anything else, check if `node_modules/` exists. If it does not, run setup automatically:

```bash
npm install
```

Do NOT ask the user — just install. After install completes, confirm: "✅ Project ready (Editor Pro Max OpenClaw). You can create videos from scratch or edit existing footage. What would you like to make?"

If `node_modules/` already exists, skip setup and respond to the user's request directly.

Requires **Node.js 20+** (LTS recommended).

## Quick Start

```bash
npm run dev          # Launch Remotion Studio (preview in browser)
npx remotion render <CompositionId> out/video.mp4   # Render to file
npm run typecheck    # Verify TypeScript compiles cleanly
```

- **Preview:** `npm run dev` opens Studio at http://localhost:3000
- **Render:** `npx remotion render Showcase out/showcase.mp4`
- **Batch render:** `./scripts/batch-render.sh Showcase youtube tiktok square`

## Architecture

```
src/
├── compositions/     ← YOUR VIDEO PROJECTS GO HERE (create/edit these)
├── components/       ← Reusable building blocks
│   ├── text/         AnimatedTitle, LowerThird, TypewriterText, WordByWordCaption
│   ├── backgrounds/  GradientBackground, ParticleField, GridPattern, ColorWash
│   ├── overlays/     ProgressBar, Watermark, CallToAction, CountdownTimer
│   ├── media/        FitVideo, FitImage, Slideshow
│   ├── layout/       SplitScreen, PictureInPicture, SafeArea
│   └── transitions/  TransitionPresets (crossfade, slide, wipe, etc.)
├── templates/        ← Ready-made video templates
│   ├── social/       TikTokVideo, InstagramReel, YouTubeShort
│   ├── content/      Presentation, Testimonial
│   └── promo/        Announcement, BeforeAfter
├── presets/          ← Colors, dimensions, easings, fonts
├── hooks/            ← useAnimation, useCaptions, useColorScheme
├── schemas/          ← Zod schemas for all props
├── utils/            ← Animation math helpers
└── Root.tsx          ← REGISTER ALL COMPOSITIONS HERE
```

### How to create a new video

1. Create a new file in `src/compositions/MyVideo.tsx`
2. Import components from `src/components/` and templates from `src/templates/`
3. Register it in `src/Root.tsx` with a `<Composition>` element
4. Preview with `npm run dev`, render with `npx remotion render MyVideo out/my-video.mp4`

## Component Reference

### Text

**AnimatedTitle** (`src/components/text/AnimatedTitle.tsx`)
```tsx
<AnimatedTitle
  text="Hello World"
  fontSize={72}
  fontWeight={800}
  color="#ffffff"
  enterAnimation="slideUp"
  exitAnimation="fade"
  enterDuration={20}
  holdDuration={60}
  exitDuration={15}
/>
```

**LowerThird** — News-style lower third.
```tsx
<LowerThird name="Speaker Name" title="Role / Station" accentColor="#01696f" position="bottomLeft" />
```

**TypewriterText** — Character-by-character reveal.
```tsx
<TypewriterText text="Hello, World!" fontSize={48} typingSpeed={2} />
```

**WordByWordCaption** — Karaoke-style word highlighting.
```tsx
<WordByWordCaption
  words={[{text: "Hello", startFrame: 0, endFrame: 15}]}
  fontSize={48}
  highlightColor="#ffffff"
/>
```

### Backgrounds

**GradientBackground** — `<GradientBackground colors={["#0f0f23", "#1a1a3e"]} angle={135} animateAngle />`

**ParticleField** — `<ParticleField count={50} color="rgba(255,255,255,0.3)" speed={0.5} />`

**GridPattern** — `<GridPattern type="dots" spacing={40} animate />`

**ColorWash** — `<ColorWash color="#0a0a0a" />`

### Overlays

**ProgressBar** — `<ProgressBar color="#01696f" height={4} position="bottom" />`

**Watermark** — `<Watermark text="@brand" corner="bottomRight" opacity={0.5} />`

**CallToAction** — `<CallToAction text="Subscribe" subtext="Turn on notifications" enterDelay={60} />`

**CountdownTimer** — `<CountdownTimer startFrom={150} fontSize={120} showLabel />`

### Media

**FitVideo** — `<FitVideo src={staticFile("video.mp4")} fit="cover" volume={0.8} />`

**FitImage** — `<FitImage src={staticFile("photo.jpg")} fit="cover" kenBurns="zoomIn" />`

**Slideshow** — `<Slideshow images={[staticFile("1.jpg"), staticFile("2.jpg")]} kenBurns transitionDuration={15} />`

### Layout

**SplitScreen** — `<SplitScreen direction="horizontal" ratio={0.5}><div>Left</div><div>Right</div></SplitScreen>`

**PictureInPicture** — PiP overlay with configurable corner and size.

**SafeArea** — `<SafeArea paddingHorizontal={60} paddingVertical={60}>...</SafeArea>`

### Transitions

Presets: `crossfade`, `fadeQuick`, `fadeSlow`, `slideLeft`, `slideRight`, `slideUp`, `slideDown`, `wipeLeft`, `wipeRight`, `clockwise`, `cut`

```tsx
import {TransitionSeries} from "@remotion/transitions";
import {TRANSITION_PRESETS} from "../components/transitions/TransitionPresets";

<TransitionSeries>
  <TransitionSeries.Sequence durationInFrames={90}><Scene1 /></TransitionSeries.Sequence>
  <TransitionSeries.Transition {...TRANSITION_PRESETS.crossfade} />
  <TransitionSeries.Sequence durationInFrames={90}><Scene2 /></TransitionSeries.Sequence>
</TransitionSeries>
```

## Templates Reference

```tsx
<TikTokVideo hook="Did you know?" body="OpenCode can edit videos." cta="Follow for more" />
<InstagramReel headline="Your headline" subtext="Details here" brandName="Brand" />
<YouTubeShort title="Title" subtitle="Subtitle" />
<Presentation slides={[{title: "Intro", body: "Welcome"}]} framesPerSlide={150} />
<Testimonial quote="Amazing product!" author="Jane Doe" role="CEO" />
<Announcement preTitle="Introducing" title="Product Name" subtitle="Tagline" cta="Learn More" />
```

## Platform Specs

| Platform | Dimensions | FPS |
|---|---|---|
| TikTok | 1080x1920 | 30 |
| Instagram Reel | 1080x1920 | 30 |
| YouTube | 1920x1080 | 30 |
| YouTube Short | 1080x1920 | 60 |
| Twitter/X | 1080x1080 | 30 |

## Animation Guide

### Golden Rules
1. **Always use `useCurrentFrame()`** for animations — never CSS transitions
2. **Always clamp interpolations** with `extrapolateRight: "clamp"`
3. Use `spring()` for natural motion, `interpolate()` for precise control

### Common Patterns

```tsx
// Fade in
const frame = useCurrentFrame();
const opacity = interpolate(frame, [0, 20], [0, 1], {extrapolateRight: "clamp"});

// Slide up with spring
const {fps} = useVideoConfig();
const progress = spring({fps, frame, config: {damping: 14, stiffness: 120}});
const translateY = interpolate(progress, [0, 1], [50, 0]);

// Enter-hold-exit
import {enterHoldExit} from "../utils/math";
const opacity = enterHoldExit(frame, 20, 60, 15);
```

## Rendering Reference

```bash
npx remotion render Showcase out/showcase.mp4
npx remotion render TikTok out/tiktok.mp4 --width=1080 --height=1920
npx remotion render Showcase out/clip.mp4 --frames=0-90
npx remotion still Showcase out/thumbnail.png --frame=45
```

## Video Editing Workflow

### Step 1: Place video
Copy your video to `public/assets/video.mp4`

### Step 2: Run the pipeline
```bash
npx tsx scripts/analyze-video.ts public/assets/video.mp4    # → public/video-metadata.json
npx tsx scripts/extract-audio.ts public/assets/video.mp4    # → public/assets/audio.wav
npx tsx scripts/transcribe.ts                                # → public/captions.json
npx tsx scripts/detect-silence.ts public/assets/video.mp4   # → public/silence.json
```

### Step 3: Create composition, preview, render

```bash
npm run dev
npx remotion render TalkingHeadEdit out/edited.mp4
```

## Editing Components

**VideoClip** — Trim video by seconds.
```tsx
<VideoClip src={staticFile("assets/video.mp4")} trimStartSeconds={5} trimEndSeconds={30} fit="cover" />
```

**CaptionOverlay** — TikTok-style captions.
```tsx
<CaptionOverlay captionsSource="captions.json" preset="bold" position="bottom" highlightColor="#39E508" />
```

**JumpCut** — Remove silence automatically.
```tsx
<JumpCut src={staticFile("assets/video.mp4")} segments={[{startSeconds: 1.2, endSeconds: 5.4}]} />
```

**AudioTrack** — Background music with ducking.
```tsx
<AudioTrack src={staticFile("assets/music.mp3")} volume={0.15} fadeInDurationSeconds={2} loop />
```

## Pipeline Scripts

| Script | Output | Purpose |
|---|---|---|
| `scripts/analyze-video.ts` | `public/video-metadata.json` | Duration, fps, dimensions |
| `scripts/extract-audio.ts` | `public/assets/audio.wav` | 16kHz WAV for Whisper |
| `scripts/transcribe.ts` | `public/captions.json` | Word-level transcription |
| `scripts/detect-silence.ts` | `public/silence.json` | Speech/silence segments |
| `scripts/remove-bg.ts` | `*-nobg.png` | AI background removal |

## Editing Templates

```tsx
<TalkingHeadEdit
  videoSrc="assets/video.mp4"
  captionsPath="captions.json"
  silencePath="silence.json"
  removeSilence={true}
  showCaptions={true}
  captionPreset="bold"
/>

<PodcastClip
  videoSrc="assets/podcast.mp4"
  clipStartSeconds={120}
  clipEndSeconds={150}
  captionsPath="captions.json"
  captionPreset="glow"
/>
```

## Workflow Tips

- Put user media in `public/assets/` → reference with `staticFile("assets/filename.ext")`
- Register every composition in `src/Root.tsx`
- Use `<AbsoluteFill>` for layering (last child renders on top)
- Use `<Sequence from={X} durationInFrames={Y}>` for timing
- For frame math: `secondsToFrames(5, 30)` = 150 frames
- This project uses **OpenCode CLI** — refer to OpenClaw/OpenCode documentation for agent capabilities
