# Week 03 Progress Report: On-Device Conversation Infill & CANDOR Corpus Audit

* **Date:** 8 September 2026
* **Author:** Gaurav 
* **Industry Mentor:** Sourabh Tiwari
* **Institute Mentor:** Dr Prathosh AP
* **Affiliation:** Samsung R&D Institute Bangalore | IISc Bangalore
* **Relevant Primary Sources:** [[ConvFill]], [[CANDOR]]

---

## 1. Literature Review: On-Device Conversation Infill (ConvFill)

This week, I explored [[Conversation Infill|Paradigm 3 (Conversation Infill)]] through the **ConvFill** paper [[ConvFill]]. ConvFill pairs a on-device model (360M parameter SmolLM2) with a powerful cloud backend (Gemini 2.5 Pro / Claude / Sonnet) to deliver sub-200ms initial response latency while incorporating continuous knowledge streams from the cloud.

### Key Architectural Highlights
* **Dual-Thread Execution & Streaming Queue:**
  * User queries are dispatched simultaneously to both the on-device infill model (SmolLM2-360M-Instruct) and the backend cloud model.
  * A shared streaming knowledge queue coordinates external knowledge chunks ($l_i$) from the backend to the infill model.
  * If the backend computation takes time, a periodic silence token ($s_i = \text{<|Sil|>}$) is sent every 1 second, signaling the infill model to bridge delays with natural conversational fillers or contextually appropriate acknowledgments.
* **Training & Data Pipeline:**
  * Trained on a synthetic dataset of 5,997 multi-domain conversations (46,151 user infills) generated via GPT-4o using 1,000 domain-specific personas from PersonaHub.
  * Models conditional generation over a special knowledge tag containing streamed backend context or silence tokens.
* **Empirical Evaluation & Performance:**
  * **TTFT Latency Reduction:** Achieves **4–67x faster initial response times** (ConvFill TTFT ~0.17s vs. Sonnet 4.5 at 2.16s and Gemini 2.5 Pro at 10.9s).
  * **QA Accuracy:** Standalone SmolLM2 scores ~10% on NaturalQuestions, but jumps to **45–52%** when backed by cloud streams via ConvFill, However it was around **15-20%** lower than accuracy provided by standalone backend model.
  * **Turn Entailment:** Evaluated using DeBERTaV3 fine-tuned on MNLI, showing ~33% direct entailment and only ~6% logical contradiction (with ~60% neutral).

---

## 2. Multimodal Data Audit: CANDOR Corpus

BetterInc approved the data access request, and I was able to download the transcription for the [[CANDOR]] corpus. The dataset spans **1,656 unscripted pair conversations** totaling over **850 hours** of speech, annotated with a high temporal precision of 10ms using models like AudioPhile, CliffHanger, and BackBiter.


### Key Audit Findings on downloaded dataset
1. **Turn-Ending Terminal Overlaps:**
      * **38% of all overlaps** (399k events) occur in the last 10% of the active speaker's turn.
      * For meaningful overlaps ($\ge 10$ words), **88.5%** occur in the final 10% of the turn.
      * Median normalized entry position at $0.977$.
      * Non-intrusive overlap in more common in Humans. (PS: IHBench does middle of turn barge-ins, rude interruptions)
2. **Rarity of Deep Barge-Ins:**
   * Substantive rude interruptions ($\ge 5$ words starting before the turn midpoint $<0.5$ and lasting $>500\text{ ms}$) happened only 1089 times in 500k+ turns.
   * IHBench does middle of turn barge-ins, rude interruptions. Can we suggest different benchmark with this?
3. **Backchannel Dynamics:**
   * Engagement backchannels (supportive listener behavior without taking the floor) occur at **~1,000 per hour** (~452.5k total).
   * Distribution in en-US: *Yeah* (40%), *Mhm* (15%), *Oh* (7%), *Okay* (7%), *Right*, *Uh Huh*, *Mm*, *Yes*, and *Wow* (1.5%), with a median entry point at $0.318$ of the active turn.
4. **Overlap Duration Distribution:**
   * Some overlaps are dominated by micro-events ($0\text{ ms}$: 399k noise/cough events; $80\text{ ms}$: 322k; $200\text{ ms}$: 220k quick backchannels; $500\text{ ms}$: 79k sustained dual-speech). They also claim this in the actual paper.

---
