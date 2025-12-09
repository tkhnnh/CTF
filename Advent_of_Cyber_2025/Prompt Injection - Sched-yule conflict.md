# Large Language models
- They are trained on massive collections of text and code, which allows them to produce human-like answers, summaries, and even generate programs or stories.
- have restrictions that prevent them from going beyond their built-in abilities, which limits them

LLM main traist: 
    - Text generation: They predict the next word step by step to form complete responses.
    - Stored knowledge: They hold a wide range of information from training data.
    - Follow instructions: They can be tuned to follow prompts in ways closer to what people expect.

# Agentic AI
-> Not strict to narrow instructions, but rather capable of acting to accomplish a goal with minimal supervision
 
- Plan multi-step plans to accomplish goals.
- Act on things (run tools, call APIs, copy files).
- Watch & adapt, adapting strategy when things fail or new knowledge is discovered.

# ReAct Prompting & Context-Awareness
AI agentics use chain-of-thought(CoT) which providing them with capabilities to perform complex, multi-step tasks autonomously

 CoT is a prompt-engineering method designed to improve the reasoning capabilities of large language models (LLMs)

 CoT has a critical limitation: because it operates in isolation, without access to external knowledge or tools, it often suffers from fact hallucination, outdated knowledge, and error propagation.

 Instead of producing only an answer or a reasoning trace, a ReAct-enabled LLM alternates between:

- Verbal reasoning traces: Articulating its current thought process.
- Actions: Executing operations in an external environment (e.g., searching Wikipedia, querying an API, or running code).
