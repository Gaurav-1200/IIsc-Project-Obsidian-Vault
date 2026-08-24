[*Towards Expert-level Medical AI for Real-time Video Consultations](https://arxiv.org/pdf/2608.09861)
[[AMIE_2608.09861.pdf]]
- Gemini based multi agent  system (Gemini 3.1 Pro & Gemini 3 Flash)
- Three major agents
	- Talker 
	- Planner
	- Perception agents
- Goal : must balance low-latency dialogue with thorough clinical reasoning and real-time audio-visual perception while maintaining robust performance across diverse patient encounters

### Talker Agent
	- Generates rapid patient facing responses
	- EPD : then start processing
	- listens even while speaking (if interruption by user -  silent listening mode)
	- processes only MOST RECENT 5 seconds of video for formulating immediate response
	- For Response generation, relies on
		- most recent states from Perception Agent
		- Planner agent goals and reasoning
		- full dialogue history
		- recent video frames
	- Inputs
		- Conversation from Video Interface 
		- Audio-visual cues from Perception Agent 
		- Clinical goals from Planner Agent
	- Outputs
		- Real time audio response
	
	
### Planner Agent
	- Managing clinical goals
	- Maintains the persistent memory of clinical state
	- Tracks the active conversational milestones
	- Executes the multi-step reasoning chain 
	- The planner internal state is kept separate from Talker
	- State updates are triggered whenever new user input is received / or Ocaasionally by Plnner
	- rate limiting of deep reasoning invocation (1/per 10 seconds)
	- Inputs
		- Conversation Content from talker
	- Output
		- Continously re-prioritized clinical goals
		- Running summary of patient
		- internal diffeerntial diagnosis & management plan

### Perception Agent
	- Processing cont audio + video stream
	- Need for this : Talker (small reasoning budget, last 5 video frames) might miss audio-visual cuees due to focus on low-lateency dialogue management
	- The percwption engine have a persistent memory over extended video window.
	- The perception state is again kept different
	- Inputs
		- Visual data stream from video interface
		- Audio data stream from video interface
	- Outputs
		- Memory of all cues detected so far
		- Newly detectd audio-visual clinical cues


## Evaluations

Scope: 
- deals with only "telehealth feasible" observations
- "actable" and "non-actable" clinical signs

### Automated Evaluation
#### Single Turn
- //ar to unit test
- pre-recorded audio/video clips
- Each case has desired resason and response
- Queries like : "“what am I showing you?"
- LLM-based auto-rater grades the agent’s response

#### Multi-turn
-  audio setting (No video)
-  visual physical examination cues were conveyed through injected verbal stage directions - description fed  as response (patient presses on their lower right abdomen while simultaneously wincing)
- LLM based auto rater
- Judged on history-taking, clinical reasoning, communication and counseling, treatment planning, triage, and red-flag identification)



### SME Evaluation



	



