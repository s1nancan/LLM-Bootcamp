Youtube link: https://www.youtube.com/watch?v=JnBHR_yL2w8&list=PL1T8fO7ArWleyIqOy37OVXsP4hFXymdOZ&index=3

### PART1: 

- LLMs are statistical models of text. They assign a probability to every suffix of a prompt, token-by-token. 
- What does it mean to be statistical model?
- Statistical pattern matcher gives bad intuitions: linear regression, autocomplete from google/phone. #type/content-idea 
- Probabilistic programs. Dohan et al. Language model cascades #type/paper2read How can you explain some of the tricks. Is it old paper? 
- Arhur Clarke's Third Law: Any suffciently advanced technology looks like a magic. 

Prompts:
1- For pretrained models: gpt3, llama: prompt is a portal
2- Instruction tuned: chatgpt, alpaca: prompt is a wish
3- Agent simulation: prompt creates a golem. 

1: Prompting re-weights documents, i.e.. we are conditioning our probabilistic model. We delete potential worlds with each token so it is mostly subtractive. 

- LMs Google for "nearby" universes. 

2: You can literally just ask. 

- If you ask I saw a grandson and grandfather trying to book Uber. Who is having hard time using the phone? -> LLM returns answer the grandfather. 
- Then if you say, please ensure the answer is unbiased and does not rely on stereotypes -> Answer becomes cant be determined. 

arxiv 2302.07459, Gnaguli et. al #type/paper2read 
Reframing Instructional Prompts 2109.07830 #type/paper2read 
Training language moddels to follow instructions 2203.02155 

3: Models can take on personas. 

Prompt programming 2102.07350 #type/paper2read 
Generative agents: 2304.03442 

In order to model the text, if we have agents that model the logic that produces the text. Language models as agent models 2212.01681 explains this. 

Language model can simulate median redditor? Cannot simulate human thinking for minutes/hours. Live API call: Cannot do. That is the promise of LAM/rabbit.tech in these days. 
Why simulate python kernel, if you can just run it. So replace LM's weakest simulators with real thing. 

### PART 2: Prompting Techniques

###### Ugly Bits

Few-shot learning is not a great model. 

Language models are few shot learners: arxiv: 2005.14165

Permuted label task: Best models can do it, but not super well. The idea is if you flip the labels of your sentences for whether they are positive or negative and ask with that context (not retrain) the model still goes back to its originals, and mostly ignore the prompt. You need lots of examples and lot of training. 

Models don't see characters, they see tokens. hello world: 2 tokens 11 characters, uryyb jbegq: 6 tokens, 11 characters. It is good at seeing tokens, not characters so that it fails to reverse a word. If you add spaces in between, each becomes a token, so it succeeds. (GPT-4 actually resolved this, it was from an 2022 tweet) 

##### Emerging Playbook

```
Triple backticks is a trick that tells it is a code, or pseudo code.
```

:  So using 3x  puts the LLM to the near of this "world".
: Add decomposition, so break the task into smaller task
: Automate, ie. Question: Follow up: Follow up.  arxiv: 2210.03350 #type/paper2read 
: Few shot prompting with CoT (chain of thought) prompting: arxiv: 2201.11903
Put your thinking into the prompt.
 : Just asking for it reasoning: Let's think step by step: arxiv: 2205/11916
 : Ask the model to check its work. self criticism: Say: "Review your answer and find problems, based on problems improve your answer"  arxvi: 2303.17491

*Compose of these tricks and matches the average human annotator performance.* 

But costs?  Takes longer to generate, more tokens. You can tune quality-cost tradeoff. 

