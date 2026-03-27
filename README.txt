═══════════════════════════════════════════════════════════════
  सृष्टिसंस्कृतम् · Sanskrit Grammar Engine
  File Architecture & Setup Guide
═══════════════════════════════════════════════════════════════

📁 YOUR PROJECT FOLDER MUST CONTAIN:
───────────────────────────────────────
  📄 sanskrit_engine_v16.html    (94 KB)  ← Main engine UI
  📄 words.js                   (6.5 MB)  ← 9,000-word dictionary
  🖼️  2d logo-01.jpg              (any)   ← Your logo (optional)

  All three files in the SAME folder. That's it.

═══════════════════════════════════════════════════════════════
  HOW THE ARCHITECTURE WORKS
═══════════════════════════════════════════════════════════════

  HTML (94 KB)                words.js (6.5 MB)
  ┌─────────────────┐         ┌──────────────────────────────┐
  │ UI + Engine JS  │ loads → │ const SHABDA_DB = {          │
  │                 │         │   "राम": {l:"P", f:[...24]}, │
  │ PRAKRIYA_DB     │         │   "चन्दन": {l:"P", f:[...]}, │
  │ (18 words, 5KB) │         │   ... 8,287 more words       │
  │ inline — fast   │         │ };                           │
  └─────────────────┘         └──────────────────────────────┘
           │                            │
           └──────────┬─────────────────┘
                      ▼
              When user types a word:
              1. PRAKRIYA_DB[word] → gender + prakriya (O(1))
              2. SHABDA_DB[word]   → 24 correct forms  (O(1))
              3. Auto-engine       → fallback for unknown words

═══════════════════════════════════════════════════════════════
  WHY THIS IS FAST (No browser freeze)
═══════════════════════════════════════════════════════════════

  • words.js loads with `defer` — non-blocking, page renders first
  • SHABDA_DB is a JS Object (hash map) → lookup is O(1)
    Even 9,000 words: lookup takes < 0.001 milliseconds
  • No loops, no Array.find(), no linear scan
  • A green dot ● appears in the UI when dictionary is ready

═══════════════════════════════════════════════════════════════
  TO DEPLOY ON A WEBSITE
═══════════════════════════════════════════════════════════════

  Option A — Static hosting (GitHub Pages, Netlify, etc.)
    Upload both files to same directory. Done.

  Option B — Local use
    Open sanskrit_engine_v16.html in Chrome/Firefox.
    Note: Chrome may block local file loading of words.js.
    Fix: run a simple local server:
      python3 -m http.server 8000
    Then open: http://localhost:8000/sanskrit_engine_v16.html

  Option C — Embed words.js inline (single file, no server needed)
    Replace: <script src="words.js" defer></script>
    With:    <script>[paste entire words.js content here]</script>
    Result:  One large self-contained HTML (~6.6 MB)

═══════════════════════════════════════════════════════════════
  ADDING MORE WORDS TO PRAKRIYA_DB (Etymology)
═══════════════════════════════════════════════════════════════

  In sanskrit_engine_v16.html, find: const PRAKRIYA_DB = {
  Add entries in this format:

  'शब्द': { g:'P', r:'धातु', r_en:'meaning', p:'प्रत्यय',
             p_en:'suffix meaning', t:'krit',
             t_sa:'कृदन्त', t_en:'Krit',
             meaning:'word meaning',
             sutra:'पाणिनीय सूत्र',
             process:'derivation steps' },

  g values: 'P' = पुंलिङ्ग, 'S' = स्त्रीलिङ्ग, 'N' = नपुंसकलिङ्ग
  t values: 'krit', 'taddhit', 'stri', 'samas', 'other'

© सृष्टिसंस्कृतम् · Dr. Chandan Kumar Pathak (CK)
