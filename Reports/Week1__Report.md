# Week 01 Progress Report: Tooling Infrastructure & Real-Time Agent Paradigms

* **Date:** 24 August 2024
* **Author:** Gaurav 
* **Industry Mentor:** Sourabh Tiwari
* **Institute Mentor:** Dr Prathosh AP
* **Affiliation:** Samsung R&D Institute Bangalore | IISc Bangalore
* **Relevant Primary Sources:** [[AMIE_2608.09861.pdf|AMIE (arXiv:2608.09861)]], [[Fast&SlowAgents_2410.08328v1.pdf|Fast & Slow Agents (arXiv:2410.08328)]]

---

## 1. Research Infrastructure & Tooling Selection

For maintaining reproducible, interconnected research notes across multiple devices without proprietary lock-in, I evaluated several knowledge-management tools:

* **Zotero:** Excellent for pure reference management, metadata extraction, and BibTeX citations, but lacks native bidirectional linking for synthesizing ideas into architectures.
* **Notion:** Strong database features, but relies on proprietary cloud storage, slow offline performance, and lack of fine-grained Git version control.
* **Obsidian + GitHub (Chosen Stack):**
  * **Local-First & Markdown-Based:** Plain `.md` files ensure zero vendor lock-in and high longevity.
  * **Knowledge Graph & Backlinks:** Enables bidirectional mapping between low-level notes (e.g., streaming latency, turn-taking) and high-level architectural proposals.
  * **Git Version Control:** Free, reliable multi-device synchronization and transparent audit logs for mentor reviews.

---

## 2. Literature Review & Technical Notes

### A. Case Study: Google's AMIE Framework
* **Context:** Google's *Articulate Medical Intelligence Explorer (AMIE)* research demonstrates dialogue-based reasoning in complex, high-stakes conversational environments.
* **Alignment with Our Objective:** AMIE addresses an identical problem space to ours: maintaining an empathetic, dynamic dialogue turn while simultaneously managing long-horizon differential diagnostic reasoning in the background.
* **Key Insights:** Documented architectural takeaways and multi-turn evaluation rubrics in [[AMIE]].

### B. Different Paradigms of Realtime agents (To be explored more)
- Talker - Reasoner : Talker optimized for speed, reason for deliberative thinking
- Dual Brain : formulation LLM brain streams continuous thoughts to the articulation brain
- Conversation Infill : The deep agent works while talker actively drives engagement during computation delay.
- Speech to Speech Models : textless spoken dialogues
---
