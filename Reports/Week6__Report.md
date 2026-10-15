# Week 06 Progress Report: Literature Consolidation

* **Date:** 15 October 2026
* **Author:** Gaurav 
* **Industry Mentor:** Sourabh Tiwari
* **Institute Mentor:** Dr Prathosh AP
* **Affiliation:** Samsung R&D Institute Bangalore | IISc Bangalore
* **Relevant Primary Sources:** [[Baseline - Fast and Slow layers]], [[Core Handoff Protocol]], [[Dynamic Orchestration]], [[AMIE]], [[ConvFill]], [[MiniCPM4.5-o]]

---

## 1. Executive Summary & Problem Formulation

In multi-step troubleshooting, conversational agents face an inherent **voice vs. deep reasoning trade-off**:
* **Monolithic Voice Models** (e.g., Gemini Live, MiniCPM-o 4.5) offer fluid, low-latency (<300ms) conversational turn-taking, but struggle with multi-step planning and degrade with large tool suites.
* **Deliberative Reasoning Systems** (e.g., LangGraph ReAct) deliver high accuracy on complex workflows, but incur 2–10s latencies and lack native handling for mid-turn interruptions (barge-ins).

To bridge this gap, I consolidated the literature log into **four structured phases**: All are listed in Notes/

---

## 2. Phase 1: Baseline Architecture  Fast and Slow Layers 

To isolate architectural gains, candidate front-ends and back-ends were categorized to establish experimental controls:

### Fast Conversational Front-Ends
* **Gemini Live:** Primary proprietary baseline with native full-duplex audio and sync/async tool calling; degrades with large tool sets (>5–10 tools).
* **OpenAI Realtime:** Secondary closed baseline for vendor-agnostic validation; higher cost profile.
* **MiniCPM-o 4.5 [[MiniCPM4.5-o]]:** Open-weight 9B Omni baseline featuring 3 time-aligned streams and a learned proactive `[listen]` token; outputs raw XML tool schemas.
* **Moshi & UltraVox:** Moshi provides a conversational sanity control (no tool support); UltraVox serves as an open half-cascade baseline to test direct audio conditioning without external ASR.

### Slow Deliberative Back-Ends
* **LangGraph + ReAct:** Core deliberative engine offering stateful directed cyclic graphs, persistent checkpoints, and multi-tool orchestration; turn-based execution is non-preemptible mid-node.
* **LlamaIndex Workflows:** Event-driven async architecture capable of emitting partial progress events to the fast layer.
* **LiveKit / Pipecat:** Live harness infrastructure providing WebRTC transport, turn detection, and native barge-in event hooks.
* **Cascaded Wrapper (ASR + ReAct + CosyVoice2):** Modular control baseline to quantify accumulated latency penalties (1.5–3.5s TTFT) against native omni pipelines.

---

## 3. Phase 2: Core Handoff Protocol (State, Interruption & Drift)

The handoff boundary coordinates bidirectional communication, interruption handling, and Phase 4 delay-hiding:


| Explored Approach | Mechanism | Limitation | Adaptation in Project |
| :--- | :--- | :--- | :--- |
| **AMIE** [[AMIE]] | Tri-agent setup: Talker, Planner (clinical goals, 1/10s rate-limit), Perception. | Fixed 10s polling rate; no suspend/resume formalism. | Replace polling with event-driven triggers; build an FSM managing `Idle`, `Infilling`, and `Preempted` states. |
| **DPT-Agent** | Dual-process human-AI collaboration with async reflection. | Treats slow reasoning as an uninterrupted background process. | Break deliberative execution into interruptible sub-nodes with intermediate state callbacks. |
| **Supervisor Pattern** | Real-time voice model delegates complex queries via tool calls. | Delegation is typically blocking; lacks context drift handling. | Build an asynchronous **Agent Bridge** that receives streaming arguments and cancels tasks upon user barge-in. |
| **ConvFill Interface** [[ConvFill]] | Streaming queue with 1.0s periodic `<|Sil|>` silence tokens for fillers. | Single-turn QA focus; 15–20% accuracy drop vs. standalone cloud model. | Extend `<|Sil|>` into structured **Workflow State Tokens** (`<|Searching|>`, `<|Verifying|>`) to guide infill. |
| **SwiftSage Heuristics** | Rule-based switching between fast and slow modules. | Brittle hand-tuned thresholds; ignores vocal prosody. | Lower-bound baseline for Phase 3 tool-invocation triggers. |

---

## 4. Phase 3: Dynamic Orchestration & Routing Intelligence

The routing policy determines *when to respond immediately locally* vs. *when to defer to the slow graph*:

* **Non-Autoregressive Intent Heads (Jev / Laya):** Single-pass discriminative classification head mapping internal embeddings directly to intent probabilities in ~33ms ($O(1)$ latency).  
  *Adaptation:* Replicate head on audio-native latent embeddings (e.g., MiniCPM-o / UltraVox) with multi-turn conversation context.
* **RouteLLM (LMSYS):** Supervised preference routing (matrix factorization/BERT) optimizing quality vs. cost.  
  *Adaptation:* Add a **Disruption Cost Term** ($C_{\text{disrupt}} = \text{Loss}(\text{Context Reset}) + \text{Cost}(\text{Wasted Graph})$) penalizing false or interrupted delegations.
* **Learning to Defer (ICML):** Consistent mathematical framework modeling primary classifier error against expert cost.  
  *Adaptation:* Define expert cost as real-time conversational disruption and route hierarchically to specific graph sub-nodes.
* **Predictive Entropy & Uncertainty Routing:** Gating based on the fast model's latent entropy/logprobs; escalates to the slow layer when semantic ambiguity is high.
* **Acoustic / Heuristic Safety Net:** Fast, zero-compute energy/pitch and keyword checks ("stop", "wait") acting as an override layer.

---

## 5. Phase 4: Conversational Latency Optimization & Delay Hiding

Deliberative reasoning takes 2–8 seconds, whereas natural conversational pauses average ~200ms ([[CANDOR]]). To bridge this window without awkward silence:
1. **Speculative Sub-Graph Pre-execution:** The bridge monitors incoming audio and speculatively dispatches high-confidence sub-graphs before the user finishes speaking, pruning or committing once the turn ends.
2. **Context-Sensitive Infill ([[ConvFill]] Paradigm):** Fast layer uses streamed workflow state tokens from the queue to emit natural, grounded transitional acknowledgments (*"Looking up error 50.1 in the manual..."*) rather than generic fillers ("uh/um").
3. **Dynamic Latency Prediction:** Estimates expected node execution duration to dynamically choose between micro-pauses (<800ms) and explicit status updates (>2s).


