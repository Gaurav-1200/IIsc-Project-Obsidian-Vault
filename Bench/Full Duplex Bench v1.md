
- 727 scenarios in 5 conditions, ~3.3 hours of stimulus audio
	- synthetic_user_interruption - 200
	- synthetic_pause_handling - 137
	- candor_pause_handling - 216
	- candor_turn_taking - 119
	- icc_backchannel - 55
- Two generators of the audio
	- **Synthetic**: GPT-4o : dialogue text, ChatTTS speaks it (interruption + synthetic pause subsets)
	- **Natural**: CANDOR excerpts (2 pause/turn-taking subsets) and ICC monologues + human listeners (backchannel subset)
- Timing precision is exact by construction - the overlap spans are done by the makers while crearing.


  
### Audit
- Only synthetic_user_interruption  have barge-ins (200 scenarios)
- Interruptions are fixed-offset injections.
	- All begin exactly 7.0 s after the preceding user question ends.
	- Median interruption duration is 3.42 s. Mini-Max (1.7-5.5s)
	- This is much longer than natural CANDOR overlaps: median 0.22 s; p95 1.10 s
- Covers topic-shift or new-request barge-ins.
	- 185/200 are questions.
	- None are corrections or retractions.
	- It does not test clarification, addition, or stop requests
- 335/727 are from [[CANDOR]] , however the exact identifiers are not given. 
- `[TURN-TAKING]` marks turn end, not interruption starting.
	- All 119 examples append exactly 5.0 s of silence after the marker.
	- Marker duration has a 0.19 s median.
	- Median user-turn end is 6.80 s.
- ICC BACKCHANNEL
	- 12,636 listener-backchannel intervals across 55 stimuli.
	- Longer monlogues from 1 st speaker( median 29.2s) (min-max)(19.3-78.7s)
	- The backchannel are short (0.47 s median)
-  Different channel have different audio format
	- Interruption: 24 kHz PCM16.
	- CANDOR: 16 kHz.
	- ICC: 48 kHz.
	- Synthetic pause: 24 kHz float32.