### Full Duplex Model
- 3 time-aligned streams:
	- env-visual: live camera / visual environment
	- env-audio: all acoustic input, including user speech
	- out-stream: assistant text + speech
- **Proactive behaviour**:
	- model can speak without an explicit prompt
	- e.g. continuous scene description, reminders, context-based comments
	- `[listen]` trained output token not external VAD silence detection
- In each chunk (Time Divisioned 1s)
	- first consumes newly arrived visual/audio tokens
	- then decides output
	- hence the response is conditioned on the newest environment state
- User Inputs
	- Not privileged turn
	- enters the same environment stream
	- model is always observing, not waiting

### Working Principle
- Different systems but closely coupled
- Audio + Vision -> LLM -> text -> TTS
- Whisper for Audio, SigLip for Vision
- Qwen-9B as LLm backbone
- LLama Decoder for Speech (~0.3B)
- The hidden states are projected to the next component.

### Compressions in Modality
- Audio Compression
	- Whisper produces 50 tokens/second
	- Compresses to 10 tokens/second - fed to LLM
- Vision Compression
	- Patch based Processing like viT, SigLIp-2 model
	- 1024 token to 64 token


### Design tradeoffs
- Listen Speak: Model produces **listen/speak** control token before generation 
- Chunk Size: 1.0 s shows best results (Compared to 1s, 0.1s and 0.2s)
- Explicit marking of chunk boundary


### Latency Savers
- instead of using text to speech : it takes LLM internal state and directly decode audio tokens from it.


### TAIL (Time aligned Interleaving)
- The text generation is way faster than the speech generation.
- So the model state would have changed for the model, even though the speech has not reached user.
- If the user interrupts here, the model generates for the updated state.
- There are 3 kinds of Interleaving
	- A : Non Interleaving:  Maintain a large text lead, then synthesize speech
	- B : Fixed Text to Speech Ratio:  assumes fixed correspondence between text and speech tokens
	- C:  chunk wise speech generation with adaptive control over how much text to generate
		-  if the $k^{th}$ chunk sees delay in speech generation, $(k+1)^{th}$ generates fewer text tokens
		- This is associated during the training data
		- **Look Ahead Speech Generation** : 
			- pronunciation of “the” depends on the following word, as in “the apple” versus “the car”.
			- Solution: Bounded Look Ahead mechanism:  Allows looking up for extra text token for added context for speech generation, but future speech gen is deferred
