#  Real-Time Agent Architecture & Dual-System Routing

* **Institution:** Samsung R&D Institute Bangalore | IISc Bangalore
* **Project Status:** Phase 1
* **Repository:** [GitHub Vault Link](https://github.com/Gaurav-1200/IIsc-Project-Obsidian-Vault)

---

##  Key Milestones & Deliverables

| Target Date | Milestone / Deliverable | Status | Owner / Action |
| :--- | :--- | :---: | :--- |
| 1st Sept 2026 | **Submit Concrete Project Proposal & Sign-off** |✅ Done | Gaurav / Mentor Sign-off |
---

##  Weekly Progress Reports

<details open>
<summary><b> Week 04: MiniCPM-o 4.5 Architecture & Full Duplex Bench v1/v1.5 Audit (Sept 8 - Sept 22)</b></summary>

* **Full Detailed Report:** [[Week4__Report]]
* **Summary:** 
    - Looked into **MiniCPM-o 4.5**, which has 3 time-aligned streams (env-visual, env-audio, out-stream), autonomous `[listen]` token generation, audio/vision compression ratios (Whisper 50$\rightarrow$10 tokens/s, SigLIP-2 1024$\rightarrow$64 tokens).
    - Came across different latency optimizations like direct LLM internal hidden-state speech decoding and Time-Aligned Interleaving (TAIL Mode A/B/C) with bounded look-ahead.
    - Audited **Full Duplex Bench v1** (727 scenarios) and **v1.5** (499 scenarios), highlighting v1.5 improvements like paired `clean_input.wav` ablation baselines and standardized 16 kHz PCM16 audio.
    - Identified benchmark discrepancies against natural speech (e.g., static fixed-offset injections, 3.42s median interruption length vs 0.22s in CANDOR, and lack of semantic correction/retraction tests).

</details>

<details>
<summary><b> Week 03: Conversation Infill Architecture & CANDOR Data Audit (Sept 1 - Sept 8)</b></summary>

* **Full Detailed Report:** [[Week3__Report]]
* **Summary:** 
    - Analyzed the **ConvFill** paper under Paradigm 3 (Conversation Infill), combining  on-device models (SmolLM2-360M) with cloud backends (Gemini 2.5 Pro).
    - Evaluated dual-thread streaming mechanics, periodic `<|Sil|>` silence token insertion, 4–67x TTFT speedup (0.17s latency), and QA accuracy / turn-level entailment trade-offs.
    - Confirmed data access request accepted for the **CANDOR** corpus; downloaded and completed comprehensive dataset audit across 850+ hours of speech.
    - Quantified natural overlap mechanics (88.5% of 10+ word overlaps are terminal at turn end), rarity of deep barge-ins (only 1,089 in 850h), and backchannel distribution (452.5k instances, ~1,000/hr).

</details>

<details>
<summary><b> Week 02: Thinking While Speaking & Multimodal Datasets (Aug 24 - Sept 1)</b></summary>

* **Full Detailed Report:** [[Week2__Report]]
* **Summary:** 
    - Explored the "Thinking while speaking" paper under the  Paradigm 2 ("where the formulation brain streams continous thoughts to the articulation brain"),
    - It uses Qwen2.5-Omni-3B model and interleaves thinking and anwer tokens during training
    -  Uses this interleaved SFT with RL based reward (Accuracy, Thinking-Answer ratio, Linguistic quality) for better TTFT, fluency while maintaining sufficient reasoning accuracy on Math & puzzle based benchmark (KK, SpokenMQA)
    - Explored the CANDOR corpus for naturalistic conversation delays, focusing on turn-taking overlap and gap metrics.
    - Requested for raw data access from BetterInc for CANDOR.

</details>

<details>
<summary><b> Week 01: Vault Setup, Tooling Evaluation & Literature Review (Ending Aug 24)</b></summary>

* **Full Detailed Report:** [[Week1__Report]]
* **Executive Summary:** 
    - Established a local-first, version-controlled research workflow using Obsidian and Git.
    - Explored tooling trade-offs (Obsidian vs. Zotero/Notion).
    - Conducted initial deep dives into real-time conversational agent paradigms, talker-reasoner architectures, and analyzed Google's **AMIE** framework.

</details>

---

##  Vault Directory
* **Research Notes:** (./Notes/)
* **Literature & Papers:** (./Sources/)
* **Formal Reports:** (./Reports/)
* **Datasets:** (./Datasets/)
* **Models:** (./Models/)
* **Standard Techniques:** (./Standard Techniques/)
