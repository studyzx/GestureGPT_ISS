## GestureGPT: Toward Zero-Shot Free-Form Hand Gesture Understanding with Large Language Model Agents

🎉✨ We are **thrilled** to announce that our paper, **"GestureGPT: Toward Zero-Shot Free-Form Hand Gesture Understanding with Large Language Model Agents"**, has been officially **accepted** to the 2024 ACM Interactive Surfaces and Spaces Conference (ACM ISS 2024) and has been awarded the **Best Paper**! 🏆🥇🎉

### Introduction

![](teaser_withoutmy.jpg)

Existing gesture interfaces only works with a fixed gesture set defined either by interface designers or by users themselves, which introduces learning or demonstration effort that diminishes their naturalness. 
Humans, on the other hand, understands free-form gestures by synthesizing the gesture, context, experience, and common sense.
In this way, the use does not need to learn, demonstrate, or associate gestures. 
So we introduce GestureGPT, a free-form hand gesture understanding framework that mimic human gesture understanding procedures to make natural free-form gestural interface possible. 
Our framework leverages multiple large language model (LLM) agents to manage and synthesize gestural and context information, then infers the interaction intent by associating the gesture to a function provided by the interface.
More specifically, our triple-agent framework involves a Gesture Description Agent that automatically segments and formulates natural language descriptions of hand poses and movements based on hand landmark coordinates. 
The description is deciphered by a Gesture Inference Agent through self-reasoning and querying about the interaction context (\eg interaction history, gaze data), which is managed by a Context Management Agent. 
Following iterative exchanges, the Gesture Inference Agent discerns the user intent by grounding it to an interactive function. 
We validated our framework offline under two real-world scenarios: smart home controlling and online video streaming. 
The average zero-shot Top-1/Top-5 grounding accuracies are 44.79\%/83.59\% for smart home tasks and 37.50\%/73.44\% for video streaming. 
We also provided an extensive discussion including model selection rationale, generalizability, and future research directions for a practical system etc.

### Reference 📚

If you would like to **cite** our work, please use the following format:

```bibtex
@article{10.1145/3698145,
author = {Zeng, Xin and Wang, Xiaoyu and Zhang, Tengxiang and Yu, Chun and Zhao, Shengdong and Chen, Yiqiang},
title = {GestureGPT: Toward Zero-Shot Free-Form Hand Gesture Understanding with Large Language Model Agents},
year = {2024},
issue_date = {December 2024},
publisher = {Association for Computing Machinery},
address = {New York, NY, USA},
volume = {8},
number = {ISS},
url = {https://doi.org/10.1145/3698145},
doi = {10.1145/3698145},
journal = {Proc. ACM Hum.-Comput. Interact.},
month = oct,
articleno = {545},
numpages = {38},
keywords = {Free-Form Gesture, Gesture Recognition, Interaction Context, Zero-Shot}
}