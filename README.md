<div align="center">
  <h1>🎼 Ritmo Music Player</h1>
  <p><strong>A high-performance, cross-platform local music player built with Tauri, Rust, and React.</strong></p>
  <p><i>Motto: "Pro & Deluxe"</i></p>
</div>

**Ritmo** is a modern desktop application designed to manage and play extensive local music libraries. Breaking away from typical web-wrapped media players, Ritmo shifts the heavy lifting—audio decoding, file system scanning, and database management—into a 100% native Rust backend. 

The result is zero-latency playback, high-fidelity lossless audio support, and a highly responsive React 19 frontend that simply reacts to the backend state at a fluid 60 FPS.

## ✨ Key Features

### 🚀 100% Native Rust Audio Engine
Bypassing the browser's Web Audio API entirely, Ritmo uses a custom audio pipeline powered by `rodio`, `symphonia`, and `cpal`. 
* **High Fidelity**: Full support for lossless formats like FLAC.
* **Hardware-Safe Buffering**: Dynamically clamps buffer allocations (e.g., 4096 frames) based on your sound card's physical limits to completely eliminate audio crackling and under-runs.
* **Smooth Seeking**: Asynchronous micro fade-outs and fade-ins (50ms) mask hardware pops and glitches during timeline jumps.

### 🔀 Dual-Playback Queue System
Never interrupt your carefully curated listening session just to preview a file. Ritmo manages two isolated playback tracks:
1. **The Library Track (`contextQueue`)**: Your persistent, standard album and library navigation.
2. **The Workbench (`userQueue`)**: A volatile "Tavolo di Lavoro" drawer. Drag and drop files directly from your OS into this queue for immediate, on-the-fly playback without destroying your primary queue.

### 🧠 Smart Metadata Sync & Acoustic DNA
Ritmo doesn't just read tags; it understands your music.
* **Acoustic Fingerprinting**: Uses a fully native, pure-Rust engine powered by `chromaprint-next` and `symphonia` to extract exact audio fingerprints entirely in RAM. By eliminating the external `fpcalc` sidecar binary, Ritmo ensures maximum cross-platform compatibility (Desktop, Android, iOS), enhanced security, and superior decoding performance.
* **Android Content URIs Bridge**: Solves mobile sandbox restrictions on Android by retrieving native Unix File Descriptors (FDs) through a Tauri V2 plugin bridge (`tauri-plugin-ritmo-audio`) and converting them to `std::fs::File` via `FromRawFd`. This enables the pure-Rust fingerprinting engine to read and decode `content://` files directly.
* **Background Daemon**: A rate-limited background worker securely queries the MusicBrainz API to identify missing metadata and fetch high-quality album covers via CoverArtArchive. Includes self-healing database routines that reset legacy failures to guarantee metadata enrichment.

### 🛡️ Zero-Trust Security Model
Ritmo treats the frontend UI as untrusted by default.
* **Strict Path Validation**: Every file path sent from the UI is canonicalized and verified against system blacklists (e.g., `Windows/System32`) to prevent Path Traversal attacks.
* **Restricted Assets**: Local cover art is served via a tightly scoped Tauri Asset protocol (`$APPDATA/covers/**`) to prevent unauthorized file system access.

### 💎 "Pro & Deluxe" UX
* **Optimistic UI Synchronization**: The React frontend polls the Rust backend at 16ms ticks. Smart visual holds (`isSeekingRef`) freeze the scrubber momentarily during seeks, preventing frustrating UI "rubber-banding."
* **Fluid Drag & Drop**: Seamless visual reordering in the Workbench using `@hello-pangea/dnd` and native OS integration via Tauri v2.
* **Volatile Previews**: Dropped OS files instantly fetch fast metadata and pseudorandom IDs, displaying embedded covers without bloating your persistent SQLite database.

### 🌐 Internationalization & Scalability
* **English First i18n**: Full-stack internationalization with `i18next`. The Rust backend remains language-agnostic, communicating via standardized state keys. Automatic OS language detection with intelligent English fallback.
* **JSON Settings Store**: A scalable, domain-partitioned Document Store built on SQLite. Uses native `json_extract` and `json_set` functions for atomic, low-latency updates of complex application preferences.

---

## 🏗️ Architecture Stack

Ritmo strictly separates presentation from core execution:

* **Backend / Core Engine**: Rust (Tauri v2 Core)
* **Audio Pipeline**: `rodio` (v0.21.1), `symphonia`, `cpal`
* **Frontend**: React 19, TypeScript, Vite, Tailwind CSS v4, `i18next`
* **Database**: SQLite (via `sqlx` in WAL Mode)
* **Tagging & Metadata**: `lofty` (v0.24.0), `chromaprint-next` & `symphonia` (AcoustID)

---

<div align="center">
  <p>Built with passion and precision.</p>
</div>
