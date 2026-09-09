Crux: a light-weight on device model generates contextually appropriate dialogue while incorporating seamlessly streaming knowledge from powerful cloud based backend model.
This is the part of paradigm [[Conversation Infill]]

- 360M parameters
- Trained on Synthetic multi-domain conversations
- sub-200ms latency, 36-42% increase in acc wrt similar size model

### Models
[[SmolLM2-360M-Instruct]]  for Infill
[[Gemini-2.5 Pro]] for Backend Model

### Dataset
- GPT-4o based synthetic dataset based on PersonaHub persona
- [[NaturalQuestions]] : For Evaluation
- 


Limitations of Existing Arch
- [[Cascaded Systems]]  
	- Modular
	- Compounding latencies
	- Higher TTFT
- [[Speech to Speech Models]]
	- single tightly coupled LLM backbone
	- Limited reasoning and domain knowledge

#### Conversation Fillers
- "um","uh","hmm" etc
- sentence-level responses - for ack of query
- Paper models it as "CONTEXT SENSITIVE, SENTENCE LEVEL RESPONSES"


### Problem Formulation [[ConvFill.pdf#search=Problem Formulation|ConvFill, p.2]]
- Infill model gets : external textual knowledge chunks, potentially delayed in time in a streaming manner
- **Entailment:**   a → b : premise guarantees the truth of a corresponding hypothesis
- For evaluation of entailment, they use **DeBERTaV3 fine-tuned on MNLI dataset**  
- Backend Model: sees entire history (user responses, infill model responses), responsible for coherent information generation and context management
- The Infill task
	- External streamed knowledge chunks : E
		- knowledge chunk :  $l_i \in L$   from backend model
		- silence token : $s_i \in S$ , generated when $d$ seconds passed without an update from backend model
	- Input to Infill Model:
		- external inputs : $(e_1, e_2, ...., e_i)$  + own conv phrase $(c_i)$  in same turn $(c_1,c_2,....,c_i-1)$ 
		- Len(S) + Len(L) = len(C) = n = # of conversation phrases generated
		- if $e_i \to c_i$  - intelligent fillers, if $e_i \in S$ - Conversation filler

### Training and Inference

#### Data
- [[CANDOR]] is intuitive but bad, substantial utilization of simple filler word negatively impact user perception
- Synthetic conversation by GPT-4o (5997 conversations , 46151 user infills)
	- 1000 domain specific personas
	- 1000 per domain
	- 8-12 turns each
	- Each conversation (JSON)
		- user
		- responder
		- responder_thoughts

#### Training Pipeline
- [[SmolLM2-360M-Instruct]] as Infill model : fp16, using ollama on M2 pro, 16GB
- *user*, *assistant* tags are already present, authors add new tag *knowledge*. The infill model conditionally generate over this
- *knowledge* is either backend utterance or silence <|sil|> ![[ConvFill.pdf#page=4&rect=73,700,298,773|ConvFill, p.4]]
- L40 GPU, 1e-5, 500 warmup steps, 5 epochs, cosine lr scheduler


#### Inference Pipeline
- 2 parallel threads : 1 for backend , 1 for convfill
- thread share 1 common streamed knowledge queue
- User query - goes to both, convfill and backend
- Backend System gives a Silence token <|Sil|> every one second, if response is still under process.

### Evaluation
- On [[NaturalQuestions]]
- TTFT  ![[ConvFill.pdf#page=5&rect=311,511,521,694|ConvFill, p.5]]
	- 4-67x faster response time compared to backend models (Sonnet 4.5 : 2.16s , Gemini2.5 Pro: 10.9s TTFT, GPT-5 0.74s, ConvFill: 0.17s)
- QA Accuracy ![[ConvFill.pdf#page=6&rect=91,522,267,705|ConvFill, p.6]]
	- the standalone SmolLM2 - 10%
	- ConvFill SmolLM2: 45-52%
	- The ConvFill cause drop in overall accuracy when compared to Standalone backend
		- Claude : 80% -> 52%
		- Gemni : 69% to 49%
		- 
- Turn Level Entailment ![[ConvFill.pdf#page=6&rect=306,644,528,706|ConvFill, p.6]]
	- entailment around 33% in general, contradiction around 6%, neutral around 60%
	- Entailment interpretation : responds only relying on its context, not changing meaning, in about 1/3 of cases.
	- Author claim , some neutrals are entailments (but marked neutral)


