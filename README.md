# SC4052-Cloud-Computing-Course-Project

# Debate Preparation Coach

A SaaS-based agentic AI system built on the Nemobot platform that prepares
podium-ready debate briefs using LLM-driven task decomposition and iterative
self-refinement.

## Link
https://nemobot-neue-experiment.vercel.app/playground/73


## Video Demo

<div align="center">
  <a href="https://youtu.be/8OeYULr9Ep8">
    <b>▶️ Click to Watch Demo!</b>
    <img src="https://img.youtube.com/vi/8OeYULr9Ep8/maxresdefault.jpg" alt="Watch Nemobot Demo" style="width:100%; max-width:600px; border-radius: 10px;">
    <br>
  </a>
</div>

<br>

<div align="center">
  <a href="https://youtu.be/XzthE86HIdE">
    <b>▶️ Click to Watch Demo!</b>
    <img src="https://img.youtube.com/vi/XzthE86HIdE/maxresdefault.jpg" alt="Watch Tech Stack Demo" style="width:100%; max-width:600px; border-radius: 10px;">
    <br>
  </a>
</div>


## Overview

Given a debate topic and position, the system produces a complete debate brief
containing core arguments, anticipated counterarguments, prepared rebuttals,
key vulnerabilities, and opening/closing statements.

The system implements two techniques from recent research:

- **Prompt Decomposition** (Khot et al., ICLR 2023) — A decomposer LLM
  dynamically plans which tools to call rather than following a hardcoded
  pipeline.
- **Prompt Differentiation / TextGrad** (Yuksekgonul et al., Nature 2024) —
  An evaluator LLM scores outputs and produces "textual gradients" that are
  fed back to refine earlier stages until quality exceeds a threshold.

## Architecture

The system runs entirely on Nemobot as 6 LLM functions plus a chat
orchestrator:

| Component | Role |
|---|---|
| `decomposer` | Plans which tools to call and in what order |
| `extract_arguments` | Generates 3 core arguments with evidence |
| `generate_counter` | Produces adversarial counterarguments |
| `generate_rebuttal` | Crafts rebuttals using acknowledge-redirect-reinforce |
| `evaluate_brief` | Scores materials and produces textual gradients |
| `final_synthesiser` | Produces the final podium-ready brief |
| Chat orchestrator | Executes the decomposer's plan + manages TextGrad loop |

## How It Works

1. User provides a debate topic and position
2. **Decomposer** generates a JSON execution plan
3. Orchestrator executes each tool in the plan, passing outputs forward
4. **Evaluator** scores the materials across 5 dimensions (1-10)
5. If score < threshold (default 8/10), gradients are fed back to refine
   arguments, counterarguments, and rebuttals
6. Loop repeats until threshold is met or max rounds (default 3) is reached
7. **Synthesizer** produces the final debate brief

## Example Output

For the topic above, the system produced a final brief scoring 8/10 after 2
TextGrad refinement rounds, including:

- 3 arguments with cited evidence (mental health, misinformation,
  polarization)
- Anticipated counterarguments with prepared rebuttals
- Key vulnerabilities and mitigation strategies
- Opening and closing statements ready to deliver

## Limitations

- **Hallucinated evidence**: Without external search API access (Nemobot's
  sandbox blocks outbound HTTP), the LLM may generate plausible but
  unverifiable statistics.
- **Token limits**: Long outputs can be truncated; the system uses 3
  arguments to stay within limits.
- **Single evaluator bias**: Evaluation uses the same underlying LLM, which
  may have systematic blind spots.

## References

- Khot, T., et al. *Decomposed Prompting: A Modular Approach for Solving
  Complex Tasks*. ICLR 2023. https://arxiv.org/abs/2210.02406
- Yuksekgonul, M., et al. *TextGrad: Automatic "Differentiation" via Text*.
  Nature 2024. https://arxiv.org/abs/2406.07496

## Author

[Your Name] — School of Computer Science and Engineering, NTU
