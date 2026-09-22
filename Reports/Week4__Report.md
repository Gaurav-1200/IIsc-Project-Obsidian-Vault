# Week 04 Progress Report: MiniCPM-o 4.5 Full-Duplex Architecture & Full Duplex Bench Audit

* **Date:** 22 September 2026
* **Author:** Gaurav 
* **Industry Mentor:** Sourabh Tiwari
* **Institute Mentor:** Dr Prathosh AP
* **Affiliation:** Samsung R&D Institute Bangalore | IISc Bangalore
* **Relevant Primary Sources:** [[MiniCPM4.5-o]], [[Full Duplex Bench v1]], [[Full Duplex Bench v1.5]]

---

## 1. Architectural Deep Dive: MiniCPM-o 4.5

This week, I read the  **MiniCPM-o 4.5** [[MiniCPM4.5-o]], an end-to-end full-duplex multimodal speech-language model. Unlike half-duplex architectures that wait for turn boundaries, MiniCPM-o 4.5 is proactive and only has arounf 9B parameters.

### Key Architectural Highlights
* **3 Time-Aligned Input/Output Streams:**
  * **`env-visual`**: Continuous live camera feed / visual environment.
  * **`env-audio`**: Complete acoustic background including user speech.
  * **`out-stream`**: Co-generated assistant text and speech responses.
* **Proactive Behavior & Explicit Control Tokens:**
  * Replaces external voice activity detection (VAD) silence triggers with a model-trained `[listen]` token.
  * Enables autonomous proactive speech without explicit user prompts (e.g., continuous scene descriptions, ambient reminders, context-aware commentary).
  * Emits explicit `listen`/`speak` control tokens prior to sequence generation inside time-divisioned 1.0s processing chunks.
* **Component Coupling & Modality Compression:**
  * **Backbone Stack:** Audio/Vision Encoders $\rightarrow$ LLM Backbone (Qwen-9B) $\rightarrow$ Text $\rightarrow$ Speech Decoder (LLaMA-based ~0.3B parameter model).
  * **Audio Compression:** Whisper encoder (50 tokens/s) compressed 5x down to 10 tokens/s fed to the LLM.
  * **Vision Compression:** SigLIP-2 / ViT patch-based processing reduces 1024 visual tokens down to 64 tokens per frame.
* **Time-Aligned Interleaving (TAIL) & Latency Optimization:**
  * Eliminates standard TTS pipeline delays by directly decoding speech tokens from internal LLM hidden states.
  * **TAIL Mechanism:** Addresses rate mismatch between fast text generation and slower speech generation. Compares three modes:
    * *Mode A (Non-Interleaving):* Large text lead built prior to speech synthesis.
    * *Mode B (Fixed Ratio):* Hardcoded token ratio mapping.
    * *Mode C (Adaptive Chunking):* Dynamically adjusts text generation; if chunk $k$ incurs speech synthesis delay, chunk $k+1$ generates fewer text tokens.
  * **Bounded Look-Ahead:** Allows context lookahead for phonetics (e.g., distinguishing pronunciation in "the apple" vs. "the car") while deferring future speech tokens to ensure immediate interruption responsiveness.

---

## 2. Benchmark Audit: Full Duplex Bench (v1 vs. v1.5)

To prepare for experimental evaluation of full-duplex agents, I audited **Full Duplex Bench v1** [[Full Duplex Bench v1]] and its updated iteration **v1.5** [[Full Duplex Bench v1.5]].

### Comparative Benchmark Structure

| Property | Full Duplex Bench v1 | Full Duplex Bench v1.5 |
| :--- | :--- | :--- |
| **Total Scenarios** | 727 scenarios (5 conditions, ~3.3h audio) | 499 scenarios (4 conditions) |
| **Ablation Baseline** | None  | Paired `clean_input.wav` overlap-free control |
| **Audio Formatting** | Mixed (16kHz, 24kHz PCM16/float32, 48kHz) | Uniform 16 kHz PCM16 across all channels |
| **Injection Offset** | Fixed at 7s post-context | Fixed at 4s post-context |
| **Conditions Tested** | Interruption, Synth Pause, CANDOR Pause, CANDOR Turn, ICC Backchannel | Interruption, User Backchannel, Talking to Other, Background Speech |

### Key Audit Findings & Limitations

1. **Barge-in Duration vs. Natural Speech:**
   * In both v1 and v1.5, interruptions are injected at static fixed offsets (7s in v1, 4s in v1.5) followed by 5.0s of trailing silence.
   * Interruption durations have a median of 3.42s (range 1.7–5.5s), which is significantly longer than naturalistic human overlaps observed in [[CANDOR]] (median 0.22s, p95 1.10s).
2. **Semantic Scope Limitations:**
   * In the 200 interruption scenarios, 185 are direct questions introducing topic-shifts or new requests.
   * The benchmark lacks evaluations for corrections, retractions, mid-turn clarifications, or explicit stop commands.
3. **Backchannel Modeling Discrepancies:**
   * In v1.5, backchannel durations average 0.79s (built across 99 scenarios from 30 unique strings), which is nearly 4x longer than natural human backchannels in [[CANDOR]] (0.22s median entry/duration).
4. **Methodological Progress in v1.5:**
   * The inclusion of paired `clean_input.wav` in v1.5 enables isolated ablation of full-duplex overlap handling versus clean conversational performance.

---
