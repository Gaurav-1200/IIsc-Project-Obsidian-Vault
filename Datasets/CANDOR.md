[[The CANDOR corpus- Insights from a large multimodal dataset of naturalistic conv_3961.pdf]]

1. Requested for Access from BetterInc
2. Transcript Access , No raw files
3. 1656  unscripted conversation , 850+ hours
4. Have used 3 models for labelling and transcription
	1. AudioPhile
	2. CliffHanger
	3. BackBiter
5.  Have metrics for overlap and gaps between turns (can help in modelling the delays)
6. High Temporal precision of 10ms



### Data Audit
1. **Natural overlaps are mostly TERMINAL :**
	1. 38% of all overlaps (399k) happens in last 1/10 of interrupted turn. (This includes all short (1-2 ) and long (5-10+) word interrupt).
	2. For Meaningful overlaps (10+ words) : 88.5% overlaps in last 1/10 of last turn.
	3. Median of when the second speaker entered is at 0.977 with overlaps (10+ words)
	4. For all overlaps (short and long) 0.788
	5. Human mostly anticipate turn endings rather than speaking before end.
2. **Deep Barge-In (Rude interruptions) are RARE:**
	1. Any substantiative interrupt (5+ words), where the second speaker entered before halfway (<0.5) and for 500+ ms  - 1089 times in 850 hours
	2. IHBench - tests for these kind of scenario, where  AI assistants are aggressively interrupted through stress test.
3. Backchanneling 
	1. Backchannels: supportive listener behaviors - show engagement but do not attempt to steal the floor
		1. ~1000 per hour of speech, 452.5k
		2. For en-US the backchannel distribution : Yeah(40%,) Mhm(15%), Oh(7%), Okay(7%), Right, Un Huh, Mm, Yes, Wow(1.5%)
		3. Median position of 0.318 of active speaker turn.
4. **Overlap duration distribution** 
	1. vast majority of overlaps are just fleeting micro-events
		1. 0 ms : 399k : everything cough, some random noise etc
		2. 80ms : 322k 
		3. 200ms : 220k : No quick backchannels - yeah, okay etc
		4. 500ms : 79k : Sustained talking
	2. 