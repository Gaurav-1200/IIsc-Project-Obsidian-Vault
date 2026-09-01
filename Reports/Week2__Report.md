# Week 02 Progress Report: Thinking While Speaking & Multimodal Datasets

* **Date:** 1 September 2026
* **Author:** Gaurav 
* **Industry Mentor:** Sourabh Tiwari
* **Institute Mentor:** Dr Prathosh AP
* **Affiliation:** Samsung R&D Institute Bangalore | IISc Bangalore
* **Relevant Primary Sources:** [[Thinking-while-speaking]], [[CANDOR]]

---


### Interleaved Generation (Thinking While Speaking)
This week, I explored the [[Different Paradigms of Realtime agents|Paradigm 2]]  (where the formulation brain streams data to articulation brain) with the paper "thinking while speaking" [[Thinking-while-speaking]]. This approach inserts reasoning steps dynamically during natural speech generation. It addresses the high Time To First Token (TTFT) and rigid alignment issues seen in traditional models.

The response sequence is modeled as interleaved chunks of thinking and answering (e.g., `T1 A1, T2 A2`). This allows the agent to compute reasoning for the next segment during the audio playback of the current one. The authors use specific `<|thinking|>` and `<|answer|>` tokens instead of standard `<think>` tags. This prevents compressing the overall sequence length.

The model is trained using GRPO with three specific rewards:
- **TA Balance:** Controls the thinking-to-answering ratio to prevent unnatural pauses or excessively long replies.
- **Linguistic Quality:** Compares output against a reference model to penalize robotic or non-fluent responses.
- **Accuracy:** Ensures the logical deduction matches the ground truth.

For some benchmarks like KK-audio, it beats the Thinking base line, authors claim it retains 96.5% of analytical capability without the latency cost. Better TTFT and fluency in many cases. Detailed notes are available here: [[Thinking-while-speaking]].

---

### The CANDOR Corpus
I explored the CANDOR corpus to help model naturalistic conversation delays. 

The dataset contains 1,656 unscripted conversations, totaling over 850 hours of audio. Crucially, it includes detailed metrics for overlaps and gaps between turns. This data is essential for modeling realistic conversational delays in real-time agents.

Transcriptions and labeling were generated using models like AudioPhile, CliffHanger, and BackBiter. I have requested transcript access from BetterInc. Raw audio files are not provided. Further details are in my notes: [[CANDOR]].

---

