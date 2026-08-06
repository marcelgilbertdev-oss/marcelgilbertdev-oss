# Marcel Gilbert

**Full-stack engineer — TypeScript · Python · AI systems · real-time 3D**

I build software where being wrong has consequences, and I test it like it matters.
Twenty-one years leading technical teams in safety-critical aviation across four countries
taught me that; I now apply the same discipline to type systems, test suites, and reviewable code.

📍 Kuwait → relocating to **Tokyo, Japan** · 🇯🇵 residency via spouse (permanent resident) — **no visa sponsorship required**
💬 English (native) · Japanese (studying)

---

### 🔭 What I'm building

**Gabriel** — a desktop-class AI workstation platform, built solo. *(private repo)*

A single React interface over conversational AI, local image generation, image-to-3D mesh
conversion, voice synthesis, an animation and storyboard studio, Blender and Unity integration,
and a system-health lane that can inspect and repair the machine it runs on.

| | |
|---|---|
| Frontend | ~24,000 lines of **strict TypeScript** + React 19, 20 feature panels, 84 explicit types |
| Backend | **Python** + FastAPI, 39 REST endpoints, job orchestration with cancellation |
| Tests | **~9,300** automated tests across 312 modules |
| History | **1,180+** commits |
| Safety | Allowlisted + sandboxed operations, reversible actions, audit receipt on every state change |

Every operation that changes state writes an auditable record. A system that acts on a real
machine should be **verifiable, not trusted** — that principle shaped the whole architecture.

---

### 📌 Public work

#### [`fair-scan`](https://github.com/marcelgilbertdev-oss/fair-scan) · Python · MIT

A two-phase fair scan over a grouped document corpus, where **group order never decides results**.

Extracted from Gabriel's retrieval layer after I found a defect that made search silently
answer from the wrong source — no error, no empty result, just a confident wrong answer, with a
whole category of documents quietly unreachable.

The repo keeps the **original broken implementation next to the fix** and runs both on the same
corpus, so you can watch it fail and then pass:

```
BEFORE  ordered scan with an early break
  found the answer : NO
  -> answered confidently from the wrong engine's documentation.
     No error. No empty result. Just wrong.

AFTER   two-phase fair scan
  found the answer : yes

Group order swapped -> identical results: yes
```

10 hermetic tests · zero dependencies · CI on Python 3.9 / 3.11 / 3.13

---

### 🧰 Tech

**Languages** TypeScript · Python · C# · JavaScript · SQL · Bash

**Frontend** React 19 · Vite · strict-mode TypeScript · REST data layers · centralized state

**Backend** FastAPI · async I/O · schema validation · SQLite · background workers

**Testing** pytest · unit / integration / regression · ESLint 9 + typescript-eslint · `tsc -b` as a build gate

**AI/ML** Local LLM inference & model routing · RAG · embeddings · evaluation gates · MCP tool servers · SDXL / FLUX · TTS

**Generative media** SDXL / FLUX image generation · image-to-3D mesh conversion · local video
models · TTS voice cloning · ffmpeg assembly pipelines

**3D & games** Unity (C#) · Unreal · Blender + `bpy` automation · rigging, weight transfer &
animation pipelines · PBR texture authoring · LOD-tiered FBX / GLB export · scene & level building

**Platforms** macOS · Linux · Unix-like · Windows

---

### 🎨 Generative media & 3D pipelines

I build the whole content chain, not just the code around it.

**Image generation** — SDXL and FLUX running locally, driven from my own in-app lab rather than
a hosted service. Character sheets, colourways, environment and prop art.

**Image → 3D** — single images converted to textured 3D meshes, then cleaned, retopologised, and
brought into a rig.

**Video & animation generation** — local video models plus an animation and storyboard studio,
assembled through an ffmpeg pipeline into finished animated shorts.

**Voice** — cloned and synthesised voices through a local TTS sidecar, used for narration.

**3D game assets** — two complete character rigs authored in Blender with Python (`bpy`)
automation I wrote: weight transfer, a repeatable rig-transplant procedure that retargets a
validated armature onto new meshes, 16 animation clips, full PBR texture sets
(base colour, normal, metallic/roughness, emissive), and LOD-tiered FBX export.

**Unity & Unreal** — scene and level building, gameplay systems, camera controllers, asset
import pipelines, and a 2.5D mobile platformer in progress.

The engineering and the art feed each other: the pipeline exists because I needed it, and the
tooling got built because doing it by hand didn't scale.

---

### 🎓 Background

**B.S. Information Technology** · University of Phoenix
**Advanced Cyber Security Certificate** (Undergraduate, awarded with honor)

Before software: 21 years in aircraft maintenance and program leadership — U.S. Air Force,
Lockheed Martin, DynCorp, Zenetex, AAR. Site lead in Japan, training manager for an 80-person
depot program in Poland, instructor in Oman teaching F-16 systems to non-native English speakers.
Work governed entirely by technical data, quality gates, and traceable records.

U.S. Air Force veteran · U.S. Secret clearance (renewed 2023)

---

### 📫 Reach me

**marcel.gilbert.dev@gmail.com**

*Open to software engineering roles — Tokyo or remote.*

<!-- profile -->
