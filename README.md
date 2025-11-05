# Short Story: In‑Context Learning with Long‑Context Models

Medium Article: Link

Presentation Slides: Link

Name: Parth Maradia

Course: CMPE-255 : Data Mining, Fall 2025

Professor: Vijay Eranti

### Overview
This short story distills the main ideas, findings, and implications of “In‑Context Learning with Long‑Context Models: An In‑Depth Exploration” presented at NAACL‑HLT 2025.
The paper investigates how scaling the number of in‑context demonstrations within very long prompts changes performance, robustness, and the relative value of example selection strategies compared to traditional few‑shot prompting and finetuning baselines.

### Paper
- Title: In‑Context Learning with Long‑Context Models: An In‑Depth Exploration.
- Venue: NAACL‑HLT 2025, Long Papers.
- Authors: Amanda Bertsch, Maor Ivgi, Emily Xiao, Uri Alon, Jonathan Berant, Matthew R. Gormley, and Graham Neubig.

### TL;DR
As context windows grow, many‑shot in‑context learning continues improving with hundreds to thousands of demonstrations, becomes less sensitive to example order, and reduces the relative advantage of retrieval over random example pools while approaching the performance of lightweight finetuning on several tasks.

### Key insights
- Scaling demonstrations well beyond few‑shot yields consistent accuracy gains across tasks, sometimes persisting into the thousands of in‑context examples.
- Retrieval helps most at small k, but its margin over random selection shrinks as k grows, making large, reusable demonstration caches surprisingly competitive.
- Sensitivity to prompt order diminishes with longer contexts, though adversarial structures like grouping by label can still hurt performance at high k.
- Compared to parameter‑efficient finetuning, long‑context ICL can be highly competitive at similar data scales while avoiding additional training, shifting optimization toward inference‑time prompt construction.

### Why it matters
Long‑context ICL offers a practical path to strong task performance without task‑specific finetuning, which can simplify deployment by relying on a single general model and cached prompts.
This reframes common engineering trade‑offs by emphasizing prompt library design, retrieval or sampling policies, and cost‑aware serving over repeated model updates.

### Limitations and scope
The study primarily evaluates open‑source mid‑sized models and a set of classification and generation tasks, so conclusions may vary for other model families, larger scales, or different task types.
Performance gains can saturate depending on task complexity and model strength, and serving very long prompts has latency and memory costs that must be managed.

### Citation
Bertsch, A., Ivgi, M., Xiao, E., Alon, U., Berant, J., Gormley, M. R., and Neubig, G., 2025, In‑Context Learning with Long‑Context Models: An In‑Depth Exploration, NAACL‑HLT 2025, Long Papers.

### 