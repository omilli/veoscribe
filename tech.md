# Technical Plan for Language Learning PWA in Bun Monorepo

## Overview
This Progressive Web App (PWA) enables real-time transcription of local video files or webcam feeds, tailored for language learning with interactive word selection, AI-driven explanations, and a vocabulary "Bucket." Videos are processed locally, with only audio chunks streamed to Deepgram for transcription, ensuring privacy. The project is organized in a **Bun monorepo** for unified dependency management and fast development, using **SolidJS** with **Vite** for the frontend and **NestJS** with **MongoDB** and **GraphQL** for the backend.

## Basic Tech Stack
### Frontend
- **SolidJS**: Reactive framework for performant UI rendering.
- **Vite**: Build tool for fast development and PWA setup.
- **JavaScript (Browser APIs)**:
  - WebRTC: Streams audio from local video/webcam to Deepgram.
  - MediaStream API: Captures audio from `<video>` element.
  - Web Speech API: Pronounces selected words.

### Backend
- **Node.js (via Bun)**: Runtime for server-side logic, compatible with Bun’s fast execution.
- **NestJS**: Structured framework for GraphQL and WebSocket endpoints.
- **Apollo Server**: GraphQL server for data management.
- **MongoDB**: NoSQL database for user profiles, Bucket sync, and learning progress.
- **Socket.IO**: Real-time communication for transcripts and AI responses.

### External Services
- **Deepgram** (Paid): Cloud-based speech-to-text API (<300 ms latency).
- **Grok API** (Paid): Cloud-based AI for word explanations and learning content.

## Bun Monorepo Structure
The monorepo is organized with Bun for dependency management, build, and execution, leveraging its speed and TypeScript support.

- **Root Directory**:
  - `bunfig.toml`: Configures Bun settings (e.g., workspaces, scripts).
  - `package.json`: Root-level dependencies and scripts for monorepo management.
  - `tsconfig.json`: Shared TypeScript configuration.
- **Packages**:
  - `packages/client`: Frontend (SolidJS + Vite).
  - `packages/server`: Backend (NestJS + MongoDB + GraphQL).
  - `packages/shared`: Shared types, utilities, and GraphQL schemas.

## Setup Instructions
### Prerequisites
- **Bun**: Latest version (`curl -fsSL https://bun.sh/install | bash`).
- **MongoDB**: Local (v6+) or cloud-hosted (e.g., MongoDB Atlas).
- **Deepgram API Key**: From Deepgram Console.
- **Grok API Key**: From xAI (https://x.ai/api).
- **Node.js**: For compatibility (Bun handles most tasks).

### Monorepo Setup
1. **Initialize Monorepo**:
 - Create root directory:
   ```bash
   mkdir language-learning-pwa
   cd language-learning-pwa
   bun init
   ```
 - Configure `bunfig.toml`:
   ```toml
   [package]
   name = "language-learning-pwa"
   version = "1.0.0"

   [workspaces]
   packages = ["packages/*"]
   ```
 - Update `package.json`:
   ```json
   {
     "name": "language-learning-pwa",
     "private": true,
     "workspaces": ["packages/*"],
     "scripts": {
       "client:dev": "bun --filter client run dev",
       "server:dev": "bun --filter server run start:dev",
       "build": "bun --filter client run build && bun --filter server run build"
     }
   }
   ```
 - Create shared TypeScript config (`tsconfig.json`):
   ```json
   {
     "compilerOptions": {
       "target": "ESNext",
       "module": "ESNext",
       "moduleResolution": "node",
       "strict": true,
       "jsx": "preserve",
       "jsxImportSource": "solid-js",
       "baseUrl": ".",
       "paths": {
         "@shared/*": ["packages/shared/src/*"]
       }
     }
   }
   ```

2. **Frontend Setup (Client)**:
 - Create `packages/client`:
   ```bash
   mkdir -p packages/client
   cd packages/client
   bun init
   ```
 - Install dependencies:
   ```bash
   bun add solid-js vite vite-plugin-solid vite-plugin-pwa @deepgram/sdk socket.io-client graphql-request dexie ffmpeg.wasm @livekit/client
   ```
 - Configure `vite.config.ts`:
   - Enable SolidJS and PWA plugins for offline support and manifest generation.
 - Create `packages/client/src` with SolidJS components for video processing, transcript display, and Bucket UI.

3. **Backend Setup (Server)**:
 - Create `packages/server`:
   ```bash
   mkdir -p packages/server
   cd packages/server
   bun init
   ```
 - Install dependencies:
   ```bash
   bun add @nestjs/core @nestjs/common @nestjs/apollo @nestjs/graphql @nestjs/mongoose mongoose socket.io apollo-server-express @deepgram/sdk @livekit/server-sdk
   ```
 - Configure NestJS in `packages/server/src`:
   - Set up GraphQL module with Apollo Server.
   - Integrate MongoDB with Mongoose for Bucket sync.
   - Configure Socket.IO for real-time transcript delivery.
   - Proxy Deepgram and Grok API calls.
 - Create `.env` for API keys:
   ```
   DEEPGRAM_API_KEY=your_deepgram_key
   GROK_API_KEY=your_grok_key
   MONGODB_URI=mongodb://localhost:27017/transcription
   LIVEKIT_URL=ws://localhost:7880
   ```

4. **Shared Package**:
 - Create `packages/shared`:
   ```bash
   mkdir -p packages/shared
   cd packages/shared
   bun init
   ```
 - Define shared types (e.g., `Word`, `Bucket`) and GraphQL schemas.
 - Install dependencies (if needed):
   ```bash
   bun add graphql
   ```

5. **Run Development**:
 - Start MongoDB locally or connect to Atlas.
 - Run client and server concurrently:
   ```bash
   bun run client:dev
   bun run server:dev
   ```

## Functionality Requiring Libraries
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
- Local video audio → WebRTC → Deepgram (transcription) → Socket.IO → Frontend (transcripts).
- Word selection → GraphQL → Grok API (explanation) → Socket.IO → Frontend (pop-up).
- Bucket data → IndexedDB (local, offline) + MongoDB (sync) via GraphQL.
- Background Bucket processing → IndexedDB (local storage) + Grok API (preloaded explanations).