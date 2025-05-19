# Technical Plan
## Overview
This Progressive Web App (PWA) enables real-time transcription of local video files or webcam feeds, tailored for language learning with interactive word selection, AI-driven explanations, and a vocabulary "Bucket." Videos are processed locally, with only audio chunks streamed to Deepgram for transcription, ensuring privacy. The project is organized in a **Bun monorepo** for unified dependency management and fast development, using **SolidJS** with **Vite** for the frontend and **NestJS** with **MongoDB** and **GraphQL** for the backend.

## Basic Tech Stack
### Frontend
- **SolidJS**: Reactive framework for performant UI rendering.
- **Vite**: Build tool for fast development and PWA setup.
- **Ark UI**: Component library for UI elements.
- **JavaScript (Browser APIs)**:
  - WebRTC: Streams audio from local video/webcam to Deepgram.
  - MediaStream API: Captures audio from `<video>` element.
  - Web Speech API: Pronounces selected words.

### Backend
- **Node.js (via Bun)**: Runtime for server-side logic, compatible with Bun’s fast execution.
- **NestJS**: Structured framework for GraphQL and WebSocket endpoints.
- **Apollo Server**: GraphQL server for data management.
- **MongoDB**: NoSQL database for user profiles, Bucket sync, and learning progress.

### External Services
- **Deepgram** (Paid): Cloud-based speech-to-text API (<300 ms latency).
- **Grok API** (Paid): Cloud-based AI for word explanations and learning content.

## Functionality
### Frontend (SolidJS + Vite)
- **PWA Support**:
- Service worker for offline caching of UI and Bucket data.
- Manifest generation for installability.
- **Word Selection**:
- Interactive text selection for clicking/tapping words/phrases in transcripts.
- Context menu for actions (Explain, Add to Bucket, Pronounce, Translate).
- **Real-Time Updates**:
- WebSocket client for Deepgram transcripts and Grok responses.
- **Local Storage**:
- IndexedDB wrapper for offline Bucket storage (words, explanations, thumbnails).
- **Audio Processing**:
- WebAssembly-based audio conversion to PCM (16-bit, 16kHz) for Deepgram.
- **GraphQL Client**:
- Query/mutate backend data (e.g., Bucket sync).
- **UI Components**:
- Reactive components for transcript display, Bucket management, flashcards, and progress charts.

### Backend (NestJS + MongoDB + GraphQL)
- **WebSocket Server**:
- Real-time broadcasting of transcripts and AI responses.
- **WebRTC Signaling**:
- Signaling server for WebRTC audio stream setup.
- **GraphQL Schema**:
- Define and serve GraphQL queries/mutations (e.g., `getBucket`, `addWord`).
- **Database Integration**:
- MongoDB ORM for user profiles, Bucket entries, and learning progress.

### Data Flow
- Local video audio → WebRTC → Deepgram (transcription) → Websocket → Frontend (transcripts).
- Word selection → GraphQL → Grok API (explanation) → Websocket → Frontend (pop-up).
- Bucket data → IndexedDB (local, offline) + MongoDB (sync) via GraphQL.
- Background Bucket processing → IndexedDB (local storage) + Grok API (preloaded explanations).