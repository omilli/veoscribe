# Features

- **Local Video Processing**:
  - Transcribes audio from local video files (e.g., MP4) or webcam without uploading.
  - Video stays in-browser; only audio chunks stream to Deepgram.
  - Supports solo learning (e.g., watching a lecture) or group video calls for conversation practice.

- **Real-Time Transcription (Deepgram)**:
  - Transcribes audio with <300 ms latency for instant feedback.
  - Displays transcripts as interactive subtitles or a scrollable text pane.
  - Toggle on/off via button; auto-pause transcription during silent video segments to save Deepgram costs.

- **Word Selection**:
  - **Interactive Highlighting**: Click/tap a word in the transcript to highlight it; double-tap for multi-word phrases (e.g., “take off”).
  - **Context Menu**: Right-click or long-press a word/phrase to access:
    - **Explain**: Triggers AI explanation (Grok API).
    - **Add to Bucket**: Saves to learner’s vocabulary bucket.
    - **Pronounce**: Plays audio pronunciation using browser-based text-to-speech (Web Speech API).
    - **Quick Translate**: Shows translation in learner’s native language (e.g., English to Spanish) via Grok API.
  - **Frequency Indicator**: Words are color-coded based on frequency in the transcript (e.g., red for rare, green for common) to prioritize high-value vocabulary.
  - **Hover Preview**: Hover over a word to see a brief definition and part of speech without full AI query, reducing API calls.
  - **Synonym Suggestion**: Click a word to see synonyms (e.g., “happy” → “joyful, content”) for varied expression practice.

- **AI Assistant (Grok API)**:
  - **Passive Behavior**: Only activates on word/phrase selection or bucket interactions, staying out of conversations.
  - **Language Learning Insights**:
    - **Detailed Explanation**: Provides:
      - Definition, part of speech, and nuanced meaning (e.g., “run: verb, to move quickly; also means to manage, as in ‘run a business’”).
      - Context analysis: Explains how the word fits the video’s dialogue (e.g., “In ‘run the meeting,’ it means to lead”).
      - Two example sentences tailored to the learner’s proficiency level (beginner, intermediate, advanced).
      - Collocations: Common word pairings (e.g., “make a decision” for “decision”).
      - Cultural notes: Highlights idiomatic or culturally specific uses (e.g., “cheers” as a British toast).
    - **Personalized Learning Path**: Suggests related words or phrases based on the learner’s bucket history and video content (e.g., “You saved ‘strategy’; try ‘tactic’ next”).
    - **Mnemonic Aids**: Generates memory hooks (e.g., “Visualize a ‘bridge’ connecting two ideas for ‘bridge’ as a metaphor”).
    - **Proficiency Feedback**: Estimates word difficulty (e.g., CEFR A1–C2) and suggests practice strategies (e.g., “Use ‘persuade’ in a sentence today”).
  - **Interactive Practice**:
    - Offers mini-quizzes on selected words (e.g., “Choose the correct meaning of ‘persuade’”).
    - Suggests speaking exercises (e.g., “Record yourself using ‘innovate’ in a sentence” via WebRTC mic).
    - Provides writing prompts (e.g., “Write a short story using ‘challenge’ and ‘overcome’”).

- **Bucket**:
  - **Vocabulary Hub**: Saves words/phrases with timestamps, transcript context, and video frame thumbnails (captured locally) for recall.
  - **Smart Organization**:
    - Auto-categorizes words by theme (e.g., “Business,” “Emotions”) based on Grok’s analysis.
    - Tags words with proficiency level (e.g., A2, B1) and learning status (e.g., “New,” “Practiced,” “Mastered”).
    - Allows manual tags (e.g., “IELTS Prep,” “Work Meetings”).
  - **Learning Tools**:
    - **Flashcards**: Generate digital flashcards for saved words (e.g., front: “endeavor,” back: definition, example).
    - **Spaced Repetition**: Schedules word reviews based on forgetting curve (e.g., review “ambitious” in 1 day, then 3 days).
    - **Progress Tracking**: Visualizes mastery (e.g., “80% of B1 words learned”) with charts.
  - **Background Processing**:
    - Preloads Grok explanations for bucket words when idle, storing in IndexedDB for offline access.
    - Updates context as new transcripts arrive (e.g., adds new uses of “challenge”).
    - Generates practice questions or mnemonics in the background for upcoming reviews.
  - **Export/Import**:
    - Export bucket as JSON/CSV for Anki or other apps.
    - Import external word lists to merge with bucket.
  - **Gamification**:
    - Earn points for adding words, completing quizzes, or mastering words.
    - Unlock badges (e.g., “Vocabulary Ninja” for 50 words learned).
    - Daily streaks for consistent practice.

- **PWA**:
  - Installable on mobile/desktop for native-like experience.
  - Offline access to UI, bucket, and cached explanations (transcription/AI needs internet).
  - Responsive design for phones, tablets, laptops.
  - Push notifications for spaced repetition reminders (e.g., “Review ‘resilient’ today!”).

- **Additional Features**:
  - Speaker diarization (Deepgram) to attribute words to speakers in group calls.
  - Punctuation and sentence segmentation for clear transcripts.
  - Auto-detects learner’s native language for translations via Grok.
  - Adjustable transcription speed (e.g., slow down for beginners).
  - Video playback controls (pause, rewind) synced with transcript highlights.