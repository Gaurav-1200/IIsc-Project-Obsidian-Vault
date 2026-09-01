- inserting reasoning steps only during natural speech generation
- interleaved SFT
- Reinforcement learning
	- TA Balance reward : timing  + thinking answer ratio
	- Linguistic Quality reward : expression and tone


Datasets used:  
Benchmarks used : [[Spoken MQA]], [[Meta-audio]], [[KK-Audio]], [[SATA-Bench]] 
Model : [[Qwen2.5-Omni-3B]], [[[CosyVoice2]]]
### Key Results
- +13% on Spoken MQA (Spoken Maths QA)
- 

#### Challenges for thinking before speaking models
- Unpredictable reasoning length
- high TTFT  (as think all before speaking )

#### Challenges for interleaved (thinking while speaking)
- rigid alignment between the thinking and speaking - verbal fluency vs reasoning depth
- No effective control over thinking answer ratio - long replies/ long pauses

### Methodology

##### Response sequence modelled as 
	S  = (T1 A1), (T2 A2) (T3 A3) .....
	
	T1 is reasoning, A1 is speaking. The training is done in the similar fashion.
	Advantage : While the playback is happening, the thinking for next part completes.

##### Thinking and Answer
- <|thinking|>  : verbalization to logical deduction
-  <|answer|> : deduction to conversation generation
-  Other system typically have  \<think> and \<\think> :  Paper says it avoids to compress overall sequence length 

#### Training
- Uses [[GRPO]] (Reinforcement learning) (Todo: Read)
- Rewards 
	- **TA Balance** : manage TA ratio (token wise)  
		- $$r_i = \max \left( 0, 1 - \left( \frac{L_i - L_{\text{target}}}{L_{\text{target}}/2} \right)^2 \right),$$
		- Ensures thinking remains close to $L_{target}$
	- **Accuracy** : $y_{pred}$ == $y_{gt}$
	- **Linguistic Quality**
		-  Since thinking and answer are interleaved, to avoid it generating robotic tone in answer part
		-  There is a reference model for conversation with prob distribution $p_{ref}$ for Question Q and Answer A , $p_{ref}(Q|A)$
		- $$p_{ref}(A \vert{} Q) = p_{ref}(a_1\vert{}Q) \cdot p_{ref}(a_2\vert{}Q, a_1) \cdot p_{ref}(a_3\vert{}Q, a_1, a_2) \dots$$
		- Taking log : We want it to be higheer$$\log p_{ref}(A \vert{} Q) = \sum_{t=1}^{T} \log p_{ref}(a_t \vert{} Q, a_{<t})$$
		-  Normalized log likelihood , $\tilde{L}_k$  as longer sentences can lead to higher LL.
		- For Candidate k, gate check if it is correct (Indicator function), then fluency check from reference ( +ve reward only when correct + more fluent)$$R_k^{LQ} = \mathbb{I}(k \in C) \cdot \max(0, \beta \cdot (\tilde{L}_k - \text{mean}_G(\tilde{L})))$$
### Evaluation
1. Fluency :  DeepSeek - V3 (check contradiction in spoken text)
2. Accuracy : weighted scores (complexity) + accuracy
3. Category wise TTFT : hard task more time

### Baselines
1. Vanilla [[Qwen2.5-Omni-3B]] (Thinking)
2. Vanilla [[Qwen2.5-Omni-3B]]  (Direct , No thinking)
3.  Fast CoT
4. InterRS (this paper)
![[Pasted image 20260901070200.png]]