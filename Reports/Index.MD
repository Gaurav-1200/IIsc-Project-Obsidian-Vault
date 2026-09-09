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
