- 499 scenarios in 4 conditions 
    - user_interruption - 200
    - user_backchannel - 99
    - talking_to_other - 100
    - background_speech - 100

- Every scenario has 
	- **paired clean_input.wav** - same timeline, overlap removed
	- gives a free with/without-overlap ablation, v1 does not have this
	- All overlaps are **pre-mixed into input.wav**, not delivered live
	- Timing precision exact by construction
	
- Uniform audio:
	- 16 kHz PCM16 for input/clean/context/overlap 


### Audit

- Overlaps are pre-mixed and inject at fixed time
	- Every overlap begins 4 s after the context file ends (Was 7s in [[Full Duplex Bench v1]])
	- Each scenario adds 5 s of silence.
- Overlap type by duration.
	- Interruption: 3.42 s median.
	- Talking-to-other: 2.04 s. median
	- Background speech: 1.72 medians.
	- Backchannel: 0.79 s median
- Backchannel 
	- around 4x times longer than [[CANDOR]] (0.79 vs 0.22)
	- 99 sceanrios with 30 unique strings
- 
