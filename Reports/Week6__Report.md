### Literature Log Consolidation
I tried breaking down the whole project into 4 different phase/parts

### Phase 1 : [[Baseline - Fast and Slow layers]]
To address the voice vs  reasoning gap on multi-step troubleshooting.
1. Monolithic Fast - High conversational setups with smaller accuracy
2. Deep reasoning based setup - High Accuracy with high latency wrt to conv. latency

### Phase 2 : [[Core Handoff Protocol]]
When handoff happens, what information to be shared to-and -fro between slow and fast layers
1. Interrupts
2. Complete State
3. Multimodal Inputs/Text Inputs

### Phase 3: [[Dynamic Orchestration]]]]
 To have fast layer + slow layer with  routing intelligence and a state machine that can manage  interruptions and drift. When to switch and when to defer
 1. LLM Tool call based
 2. Static
 3. Heuristic based
 4. Jev/Laya Based
 
### Phase 4: [[Latency Optimization]]
How to hide latency during conversations. (To-do)
1. Speculative Processing
2. Filler Conversation
3. Latency Prediction
